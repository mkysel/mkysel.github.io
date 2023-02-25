---
title: "HackerRank 'Box It!' Solution"
date: "2020-08-21"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
---

##### Short Problem Definition:

Design a class named _Box_ whose dimensions are integers and private to the class.

...

##### Link

[Box It!](https://www.hackerrank.com/challenges/box-it/problem)

##### Complexity:

Does not apply.

##### Execution:

Does not apply.

##### Solution:

```cpp
// The class should have the following functions : 
class Box {
    private:
        int l=0;
        int b=0;
        int h=0;
    public:
// Constructors: 
        // Box();
        Box() = default;
        // Box(int,int,int);
        Box(int len, int bre, int hei):
            l(len), b(bre), h(hei) {};
        // Box(Box);
        Box(Box &other):
            l(other.l), b(other.b), h(other.h) {};

        // int getLength(); // Return box's length
        int getLength() { return l;}
        // int getBreadth (); // Return box's breadth
        int getBreadth() { return b;}
        // int getHeight ();  //Return box's height
        int getHeight() { return h;}

        // long long CalculateVolume(); // Return the volume of the box
        long long CalculateVolume() { return (long long) l*b*h;}

        //Overload operator < as specified
        //bool operator<(Box& b)
        bool operator<(Box& b) {
            return this->l < b.l ||
                (this->b < b.b && this->l == b.l) ||
                (this->h < b.h && this->b == b.b && this->l == b.l);
        }
};

ostream& operator<<(ostream& out, Box& b) {
    out << b.getLength() << " ";
    out << b.getBreadth() << " ";
    out << b.getHeight() << " ";

    return out;
}
```
