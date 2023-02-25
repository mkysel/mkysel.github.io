---
title: "Demystifying Virtual Tables in C++ - Part 1 Trivial Constructors"
date: "2016-05-21"
categories: 
  - "c"
  - "programming"
tags: 
  - "featured"
---

## Introduction

This article is part of the C++ Virtual Table Series: [Part 1](https://www.martinkysel.com/demystifying-virtual-tables-in-c-part-1-trivial-constructors/), [Part 2](https://www.martinkysel.com/demystifying-virtual-tables-in-c-part-2-non-virtual-inheritance/), [Part 3](https://www.martinkysel.com/demystifying-virtual-tables-in-c-part-3-virtual-tables/).

Don't expect this article to be an easy read. After each claim, I will provide supporting code in both C++ and assembly.

Recently I was working on a particularly sneaky bug. A thing happened to me in C++, that should not be possible under normal circumstances. By normal, I mean that 1+1 is guaranteed to 2; the sun rises in the East and sets in the West; all water is wet, and the earth is dry. It still happened. Namely: the **vptr** of a fully constructed virtual object was pointing to the **virtual table** of its **base** class. To debug this particular issue one needs to dive deep into the compiler world and into the dreaded waters of _assembly_.

I hope that I do not have to point out that when I started, I had no clue what is going on. I knew what virtual pointers do and that the compilers are reasonably smart about them. As every modern software engineer, I consulted the web oracle only to realize that there is _very little written_ online. I wanted to know **how vptrs and vtables are implemented**. Not your typical high-level theory explaining how dynamic dispatch works. I wanted to know what makes it tick. And while doing so, I might improve the world a bit by sharing my journey. So I hopped on my trusted GDB and rode off to the assembly land.

## A Bit of Theory

Now, what is this virtual table? It’s worth remembering that a vtable is known by many different names: virtual function table, virtual method table, and even as a dynamic dispatch table. I will use all of those names based on context.

> When working with virtual functions in C++, it’s the vtable that’s being used behind the scenes to help achieve polymorphism. And, although you can understand polymorphism without understanding vtables, many interviewers like to ask this question just to see if you really know your stuff…

I strongly disagree with the quote. My understanding has only brought me so far and I am not even interviewing. We already understand polymorphism. Next, let us understand vtables.

> Whenever a class itself _contains virtual functions or overrides virtual functions from a parent class_ the compiler builds a vtable for that class. This means that _not all classes have a vtable_ created for them by the compiler. The vtable contains function pointers that point to the virtual functions in that class. There can only be one vtable per class, and all objects of the same class will share the same vtable. [\[1\]](http://www.programmerinterview.com/index.php/c-cplusplus/how-vtables-work/)

The quote above contains a few complicated sentences. Let's break it up into smaller claims. It will take a few posts to verify them all so I will start from the simplest.

## Trivial Constructors

> _....not all classes have a vtable_ created for them by the compiler

A trivial constructor is not a term you will find in the C++ specification. Yet often people talk about non-trivial constructors as if there was some simple distinction. Therefore let us work with this definition: _For a default constructor and destructor being "trivial" means literally "do nothing at all". For copy-constructor and copy-assignment operator, being "trivial" means literally "be equivalent to simple raw memory copying" (like copy with `memcpy`)._ 

Being trivial inherently also means that the constructor can be inlined in the form of memcpy or malloc and no constructor call is necessary. The following C++ class _Trivial_ has no constructor defined so the compiler is free to do as it likes. We know that it will create a default constructor and that this constructor might be trivial. The code is compiled with -O0 (no optimisations), otherwise printf would be inlined directly without the need to create the class.

```cpp
#include <stdio.h>
#include <iostream>
 
class Trivial {
public:
    void method() {
        printf("method\n");
    }
};
 
int main() {
    Trivial t;
    t.method();
}
```

```
(gdb) disas main
Dump of assembler code for function main():
   0x000000000040070d <+0>:     push   %rbp
   0x000000000040070e <+1>:     mov    %rsp,%rbp
   0x0000000000400711 <+4>:     sub    $0x10,%rsp
   0x0000000000400715 <+8>:     lea    -0x1(%rbp),%rax
   0x0000000000400719 <+12>:    mov    %rax,%rdi
   0x000000000040071c <+15>:    callq  0x40077a <Trivial::method()>;
   0x0000000000400721 <+20>:    mov    $0x0,%eax
   0x0000000000400726 <+25>:    leaveq
   0x0000000000400727 <+26>:    retq
End of assembler dump.
```

As we can see there is no constructor call. The class also has no members so no allocation is necessary.

If you define a constructor yourself, it is considered non-trivial, even if it doesn't do anything, so a trivial constructor must be implicitly defined by the compiler.

## Default Constructors

Let's force the compiler to call a constructor of our class.

```cpp
#include <stdio.h>
#include <iostream>
 
class Trivial {
public:
    Trivial() {}
    void method() {
        printf("method\n");
    }
};
 
int main() {
    Trivial t;
    t.method();
}
```

```
(gdb) disas main
Dump of assembler code for function main():
   0x000000000040070d <+0>:     push   %rbp
   0x000000000040070e <+1>:     mov    %rsp,%rbp
   0x0000000000400711 <+4>:     sub    $0x10,%rsp
   0x0000000000400715 <+8>:     lea    -0x1(%rbp),%rax
   0x0000000000400719 <+12>:    mov    %rax,%rdi
   0x000000000040071c <+15>:    callq  0x400786 <Trivial::Trivial()>
   0x0000000000400721 <+20>:    lea    -0x1(%rbp),%rax
   0x0000000000400725 <+24>:    mov    %rax,%rdi
   0x0000000000400728 <+27>:    callq  0x400790 <Trivial::method()>
   0x000000000040072d <+32>:    mov    $0x0,%eax
   0x0000000000400732 <+37>:    leaveq
   0x0000000000400733 <+38>:    retq
End of assembler dump.
(gdb) disas 0x400786
Dump of assembler code for function Trivial::Trivial():
   0x0000000000400786 <+0>:     push   %rbp
   0x0000000000400787 <+1>:     mov    %rsp,%rbp
   0x000000000040078a <+4>:     mov    %rdi,-0x8(%rbp)
   0x000000000040078e <+8>:     pop    %rbp
   0x000000000040078f <+9>:     retq
End of assembler dump.
```

sWe effectively forced the compiler to create an empty constructor. It does not do much besides modifying the stack pointer. We have proven that even with all optimizations turned off, in both default and trivial constructors there is **no additional space wasted** on virtual tables. Interestingly the size of the object is not zero as it might seem from the argumentation. It is not possible for two objects of zero size to share the same space and therefore the minimum size is always at least 1 byte — more on this topic on [Stroustrup's C++ FAQ](http://www.stroustrup.com/bs_faq2.html#sizeof-empty).

Next we will look at \[[Non-Virtual Inheritance](http://www.martinkysel.com/demystifying-virtual-tables-in-c-part-2-non-virtual-inheritance/)\].
