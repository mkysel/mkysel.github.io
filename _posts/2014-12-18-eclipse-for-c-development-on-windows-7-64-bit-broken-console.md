---
title: "Eclipse for C++ Development on Windows 7 64-bit Broken Console"
date: "2014-12-18"
categories: 
  - "c"
  - "programming"
---

It is the year 2014 and for some reason Eclipse CDT (both 32 and 64bit version) since Ganimede won't output anything the programmer wants to print to the C++ console. The other side effect is, that it is impossible to start the gdb debugger. This problem has been posted all around the web since 2010 and yet it persists with Luna.

![EmptySpace](images/EmptySpace-300x225.png)

I was able to compile the source using Eclipse, but had to switch to the Cygwin console and execute the binary manually. Doing this is a tedious process and wastes a lot of time. The bad news is, that I have found no solution for Cygwin. The good news is, that the MinGW linker will accept static gcc libraries linkage. So, for the rest of this guide, I will be assuming that you can use the MinGw GNU.

To fix this, follow any tutorial on installing Eclipse CDT with MinGW such as [this](https://www.ics.uci.edu/~pattis/common/handouts/mingweclipse/mingweclipse.html "MinGW Eclipse Installation") and then

> Add “**\-static-libgcc -static-libstdc++**” as Linker flags for your new project. This text should be added to the Linker flags field, which can be found by right-clicking on the new Project in the Project Explorer and clicking on Properties. Under the Project Properties, expand the C/C++ Build menu and click on Settings. Under the Tool Settings tab, expand the MinGW C++ Linker menu and click on Miscellaneous. Add the text to the Linker flags field, then click the Apply button.

[](http://www.martinkysel.com/wp-content/uploads/2014/12/SuccessfulPrint.png)[![C++Linker](images/C-Linker1.png)](http://www.martinkysel.com/wp-content/uploads/2014/12/C-Linker1.png)

 

Cleaning the project, building it again results in a successful run. The debugger also started working.

[![](images/SuccessfulPrint1-300x225.png)![WorkingDbg](images/WorkingDbg-300x225.png)](http://www.martinkysel.com/wp-content/uploads/2014/12/SuccessfulPrint1.png)
