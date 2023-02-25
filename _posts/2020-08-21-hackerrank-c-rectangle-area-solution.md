---
title: "HackerRank 'C++ Rectangle Area' Solution"
date: "2020-08-21"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
---

##### Short Problem Definition:

Create two classes:

**Rectangle**  
The _Rectangle_ class should have two data fields-_width_ and _height_ of _int_ types. The class should have _display()_ method, to print the _width_ and _height_ of the rectangle separated by space.

**RectangleArea**  
The _RectangleArea_ class is derived from _Rectangle_ class, i.e., it is the sub-class of _Rectangle_ class. The class should have _read\_input()_ method, to read the values of _width_ and _height_ of the rectangle. The _RectangleArea_ class should also overload the _display()_ method to print the area width\*height of the rectangle.

##### Link

[Rectangle Area](https://www.hackerrank.com/challenges/rectangle-area/problem)

##### Complexity:

Does not apply.

##### Execution:

##### Solution:

```cpp
// because cstdio is superior!
#include <cstdio>
/*
 * Create classes Rectangle and RectangleArea
 */

class Rectangle {
    public:
        int width = 0;
        int height = 0;

        virtual void display() {
            printf("%d %d\n", width, height);
        }
};

class RectangleArea: public Rectangle {
    public:
        void read_input() {
            scanf("%d %d", &width, &height);
        }

        void display() override {
            printf("%d\n", width*height);
        }
};
```
