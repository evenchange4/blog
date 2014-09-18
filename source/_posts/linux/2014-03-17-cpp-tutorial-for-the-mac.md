title: C++ Tutorial for the Mac User: Include self-defined header files
date: 2014-03-17 08:11:26
tags: [linux, c++]
---
For the course of NTU [Programming Design, Spring 2014](http://www.im.ntu.edu.tw/~lckung/courses/PDSp14/)

If you want to compile a C++ program that includes self-defined header files on Mac or other Linux environment. We suggest you the following two ways to do that. One way is using g++ to compile and run programs in terminal (Recommended). Another way is using Xcode to new a project (i.e., a file with filename extension *.xcodeproj*).
Here, we provide two *cpp* files, and one *header* file to you for demonstrating how you can run it on Mac. Just the same as windows’ Dev-C++. Download [example code](https://github.com/evenchange4/102-2_PD_Cpp-Tutorial-for-the-Mac/archive/master.zip) that was mentioned in the class.

<!-- more -->

## Using Commands in Terminal (Recommended)
In Figure 1, find terminal in the Spotlight results, and then click it to open a new Terminal windows (Figure 2).

![▲ Figure 1: Search your Terminal on Mac.](http://media-cache-ec0.pinimg.com/736x/a2/ce/2c/a2ce2ca0ce15d4723e114adcdf4782c3.jpg)

![▲ Figure 2: Terminal windows view.](http://media-cache-ec0.pinimg.com/originals/31/ae/31/31ae31f1aaeb260267263faee8a433b2.jpg)

Before starting, some basic knowledge you need to know is about unix-like system command-line commands. Here we list some useful commands.
First, we need to change directory `cd path` to the right one which your code is available under this folder. We assume that the codes are under the path

** *~/Downloads/project* **

And then we need to to switch our path to the destination path of project.

```
$ cd Downloads/project
```

Then we type the command below to list `li` what files are under the folder project.

```
$ ls
```

And, we can compile our C++ codes as below. The two cpp files will be linked together automatically (i.e., It links all the object files that are separated by a white space.).  Here we will get one executable file (e.g., run). `-o filename` is an argument of output file name.

```
$ g++ main.cpp myMax.cpp -o run
```

Finally, execute `./program` the program, and the results will be printed on the Terminal windows. The above steps are shown in Figure 3.

```
$ ./run
```

![▲ Figure 3: Screenshot of final results.](http://media-cache-ec0.pinimg.com/originals/0c/ce/39/0cce398d637b44bc189ef32d8d6b2f33.jpg)

## Using Xcode
We use the Xcode in Mac which is almost the same as windows’ Dev-C++. In Figure 4, we create a new Xcode project, and then select the ** *OSX > Application > Command Line Tool* ** option (Figure 5).

![▲ Figure 4: Create a new Xcode project.](http://media-cache-ak0.pinimg.com/originals/4e/99/11/4e9911c6a8f276155b9076785ec407b5.jpg)

![▲ Figure 5: OSX > Application > Command Line Tool](http://media-cache-ec0.pinimg.com/736x/85/aa/1b/85aa1b5598a9008a7ae5a4a66aec2f0b.jpg)

In Figure 6, you need to name the product first, and keep the product type as C++ (of course).  Then, we put all of the downloaded source codes in the project, but we need some tips to do that. Now, create two empty files manually (***File > new > File***), and those will be your *cpp* file and *header* file (Figure 7 and Figure 8).

![▲ Figure 6: Name the Product.](http://media-cache-cd0.pinimg.com/originals/67/7d/75/677d75783e85548b6b3995235da870f8.jpg)

![▲ Figure 7: Create two empty files manually.](http://media-cache-ec0.pinimg.com/originals/c7/03/36/c70336e5b0bc319cac47cf053948b02a.jpg)

![▲ Figure 8: Those are your cpp file and header file.](http://media-cache-ak0.pinimg.com/originals/38/7c/87/387c8791e7deb2c373f9e082eb6e5efd.jpg)

Finally, we can execute your project, and we will get the results in the console window in the bottom of Xcode (Figure 9).

![▲ Figure 9: Execute Project.](http://media-cache-ak0.pinimg.com/originals/0b/65/78/0b6578094de1329f9eb5ff60bdf06d67.jpg)

## 後記
第一次用英文打這種 Tutorial，因為當了 全英文授課的 PD 助教所以需要全英文的文件，希望一學期過後破爛的英文可以稍微有些長進ＱＱ 特別感謝 Lynn 從旁協助。

## Reference
- [ADT List implement sample code](https://github.com/evenchange4/102-1_DS_TA_Sample-code/tree/master/ADT%20List%20code)
- [2012.09.11 程式編輯與編譯環境安裝 Summary](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxudHVjc2llYzIwMTJ8Z3g6MWE1NzkzNzMxNzI0ODVmZQ)
- [Class 範例程式](https://gist.github.com/evenchange4/52ba298788b23bda9ac3)