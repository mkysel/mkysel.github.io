---
title: "Demystifying Virtual Tables in C++ - Part 3 Virtual Tables"
date: "2016-06-24"
categories: 
  - "c"
---

## Introduction

This article is part of the C++ Virtual Table Series: [Part 1](https://www.martinkysel.com/demystifying-virtual-tables-in-c-part-1-trivial-constructors/), [Part 2](https://www.martinkysel.com/demystifying-virtual-tables-in-c-part-2-non-virtual-inheritance/), [Part 3](https://www.martinkysel.com/demystifying-virtual-tables-in-c-part-3-virtual-tables/).

In the previous parts of the series, we look at [Trivial](http://www.martinkysel.com/demystifying-virtual-tables-in-c-part-1-trivial-constructors/) cases and [Non-virtual](http://www.martinkysel.com/demystifying-virtual-tables-in-c-part-2-non-virtual-inheritance/) inheritance. Now, it is time to look at the actual content of the series. I repeat the citation we are verifying here:

> Whenever a class itself _contains virtual functions or overrides virtual functions from a parent class_ the compiler builds a vtable for that class. This means that _not all classes have a vtable_ created for them by the compiler. The vtable contains function pointers that point to the virtual functions in that class. There can only be one vtable per class, and all objects of the same class will share the same vtable. [\[1\]](http://www.programmerinterview.com/index.php/c-cplusplus/how-vtables-work/)

We have shown that classes without a virtual function indeed contain no virtual pointer and no virtual table is constructed. A virtual table is an array of **function pointers** although other data types are also possible. The layout is generally compiler-specific (or ABI-specific where multiple C++ compilers share an ABI) and somewhat stable. All the virtual function tables are in the memory associated with your process. In case of GDB all your virtual function tables are stored in **read-only memory** which protects it from unintentional overwrites. The functions themselves (their assembly instructions) are stored in the .text section of the elf binary.

## Structure of a virtual table

```cpp
class Base {
    public:
        virtual ~Base() { }
        virtual void method() = 0;
};

class Derived: public Base{
    public:
        virtual ~Derived() {}
        void method() {}
};

int main() {
    Base* m = new Derived();
    delete m;
}
```

```
(gdb) info vtbl m
vtable for 'Base' @ 0x400af0 (subobject @ 0x603010):
[0]: 0x400986 [Derived::~Derived()]
[1]: 0x4009c0 [Derived::~Derived()]
[2]: 0x4009e6 [Derived::method()]
```

The entries for virtual destructors are actually pairs of entries. The first destructor, called the _complete object destructor_, performs the destruction without calling delete() on the object. The second destructor, called the _deleting destructor_, calls delete() after destroying the object. Both destroy any virtual bases; a separate, non-virtual function, called the base object destructor, performs destruction of the object but not its virtual base subobjects, and does not call delete().

Non-virtual functions are dispatched statically, as we have shown in the first blog entry, and therefore do not require an entry in the virtual table.

## Location in memory

Let us examine where the virtual table is located. We will ignore many intricacies of ELF that are not relevant to this explanation. First, let us examine the borders of each section in the file.

### Layout of the ELF binary

```
readelf --sections a.out
There are 36 section headers, starting at offset 0x6420:

Section Headers:
  [Nr] Name              Type             Address           Offset
       Size              EntSize          Flags  Link  Info  Align
  [ 0]                   NULL             0000000000000000  00000000
       0000000000000000  0000000000000000           0     0     0
  [13] .text             PROGBITS         00000000004007a0  000007a0
       0000000000000302  0000000000000000  AX       0     0     16
  [14] .fini             PROGBITS         0000000000400aa4  00000aa4
       0000000000000009  0000000000000000  AX       0     0     4
  [15] .rodata           PROGBITS         0000000000400ac0  00000ac0
       00000000000000d0  0000000000000000   A       0     0     32
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), l (large)
  I (info), L (link order), G (group), T (TLS), E (exclude), x (unknown)
  O (extra OS processing required) o (OS specific), p (processor specific)
```

The address space boundaries of this particular ELF binary are as follows:

- \[0x04007a0-0x0400aa4\] - is the text section containing **disassembly of functions** (0x400986)
- \[0x0400aa4-0x0400ac0\]
- \[0x0400ac0-0x0400b90\] - is the read only section containing the **vtables** (0x400af0)

### Read-only memory

Let us look at the objdump of the read-only memory containing the virtual table. We will later compare it with the disassembly from gdb. Somewhere we should see the value 0x400af0.

```
objdump -s -j .rodata ./a.out 

./a.out:     file format elf64-x86-64

Contents of section .rodata:
 400ac0 01000200 00000000 00000000 00000000  ................
 400ad0 00000000 00000000 00000000 00000000  ................
 400ae0 00000000 00000000 600b4000 00000000  ........`.@.....
 400af0 86094000 00000000 c0094000 00000000  ..@.......@.....
 400b00 e6094000 00000000 00000000 00000000  ..@.............
 400b10 00000000 00000000 00000000 00000000  ................
 400b20 00000000 00000000 800b4000 00000000  ..........@.....
 400b30 32094000 00000000 60094000 00000000  2.@.....`.@.....
 400b40 80074000 00000000 37446572 69766564  ..@.....7Derived
 400b50 00000000 00000000 00000000 00000000  ................
 400b60 f0206000 00000000 480b4000 00000000  . `.....H.@.....
 400b70 800b4000 00000000 34426173 65000000  ..@.....4Base...
 400b80 90206000 00000000 780b4000 00000000  . `.....x.@.....
```

When looking at the line 0x400af0 we notice that the values are not what we expect. The byte order is reversed in objdump compared to the disassembly. The raw bytes are \[0x86, 0x9, 0x40, 0x0\] with big-endian byte order this results in 0x400986 and in little-endian byte order this results in 0x860940. Is it the same we are seeing in GDB?

```
(gdb) x/6x 0x400af0
0x400af0 [_ZTV7Derived+16]:     0x00400986      0x00000000      0x004009c0      0x00000000
0x400b00 [_ZTV7Derived+32]:     0x004009e6      0x00000000
```

It indeed is. Those are our function pointers.

### Text section

Amongst other things, this section of the ELF binary contains the disassembly of all the virtual (and non-virtual functions). If you look at their addresses you will realize, that the values correspond to the entries in the virtual table.

```
objdump -d a.out
a.out:     file format elf64-x86-64

Disassembly of section .text:

0000000000400986 [_ZN7DerivedD1Ev]
00000000004009c0 [_ZN7DerivedD0Ev]
00000000004009e6 [_ZN7Derived6methodEv]
```

```
(gdb) disas 0x400986
Dump of assembler code for function Derived::~Derived():
   0x0000000000400986 [+0]:     push   %rbp
   0x0000000000400987 [+1]:     mov    %rsp,%rbp
   0x000000000040098a [+4]:     sub    $0x10,%rsp
   0x000000000040098e [+8]:     mov    %rdi,-0x8(%rbp)
   0x0000000000400992 [+12]:    mov    -0x8(%rbp),%rax
   0x0000000000400996 [+16]:    movq   $0x400af0,(%rax)
   0x000000000040099d [+23]:    mov    -0x8(%rbp),%rax
   0x00000000004009a1 [+27]:    mov    %rax,%rdi
   0x00000000004009a4 [+30]:    callq  0x400932 [Base::~Base()]
   0x00000000004009a9 [+35]:    mov    $0x0,%eax
   0x00000000004009ae [+40]:    test   %eax,%eax
   0x00000000004009b0 [+42]:    je     0x4009be [Derived::~Derived()+56]
   0x00000000004009b2 [+44]:    mov    -0x8(%rbp),%rax
   0x00000000004009b6 [+48]:    mov    %rax,%rdi
   0x00000000004009b9 [+51]:    callq  0x400730 [_ZdlPv@plt]
   0x00000000004009be [+56]:    leaveq
   0x00000000004009bf [+57]:    retq
End of assembler dump.
```

## Writing over the VTBL

Why is it essential that the virtual table is in the read-only memory? The virtual pointer is located at the beginning of each object. It is possible to hack this pointer to point to any other table (maliciously created by an adversary). Is it possible to manipulate the table itself? If this were possible, we would be able to corrupt ALL objects. Let us attempt that.

```cpp
int main() {
    Derived* m = new Derived();
    long ***mVtable = (long ***)&m;
    printf("Derived VTABLE: %p\n", **mVtable);
    printf("First entry of Derived VTABLE: %p\n", (void*) mVtable[0][0][0]);
    printf("Second entry of Derived VTABLE: %p\n", (void*) mVtable[0][0][1]);
    printf("Third entry of Derived VTABLE: %p\n", (void*) mVtable[0][0][2]);
    printf("Address of FCT: %p\n", (void*) &assignableFct1);
    mVtable[0][0][2] = (long)&assignableFct1;
}
```

```
Derived VTABLE: 0x400af0
First entry of Derived VTABLE: 0x400986
Second entry of Derived VTABLE: 0x4009c0
Third entry of Derived VTABLE: 0x4009e6
Address of FCT: 0x4007ad
Segmentation fault
```

The code above might look a bit complicated but it is not. mVtable is dereferenced three times to reach the first entry in the vtable.

- the value of the pointer is the address of the variable on the stack
    - (long \*\*\*) 0x7fffffffe3b0
- the first dereference of mVtable gives us the address of the actual object on the heap
    - (long \*\*) 0x603010
- the second dereference of mVtable gives us the address of the vtable we are looking for
    - (long \*) 0x400af0 \[vtable for Derived+16\]
- the third dereference of mVtable gives us the address of the first entry in the vtable
    - (void \*) 0x400986 \[Derived::~Derived()\]

Fortunately, this function will segfault. **Writing over read-only memory is not allowed**. Sweet.
