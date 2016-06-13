---
layout: post
title:  "mac下编译C和C++文件"
date:   2016-06-06 12:23:11
categories: iosre kaupelot android
---
#### 1简介

GCC 的意思也只是 GNU C Compiler 而已。经过了这么多年的发展，GCC 已经不仅仅能支持 C 语言；它现在还支持 Ada 语言、C++ 语言、Java 语言、Objective C 语言、Pascal 语言、COBOL语言，以及支持函数式编程和逻辑编程的 Mercury 语言，等等。而 GCC 也不再单只是 GNU C 语言编译器的意思了，而是变成了 GNU Compiler Collection 也即是 GNU 编译器家族的意思了。另一方面，说到 GCC 对于操作系统平台及硬件平台支持，概括起来就是一句话：无所不在。

#### 2简单编译

示例程序如下：

    //test.c
    #include <stdio.h>
    int main(void)
    {
        printf("Hello World!\n");
    return0;
    }

这个程序，一步到位的编译指令是:

    gcc test.c -o test

实质上，上述编译过程是分为四个阶段进行的，即预处理(也称预编译，Preprocessing)、编译(Compilation)、汇编 (Assembly)和连接(Linking)。

##### 2.1预处理

    gcc -E test.c -o test.i 或 gcc -E test.c

 

可以输出test.i文件中存放着test.c经预处理之后的代码。打开test.i文件，看一看，就明白了。后面那条指令，是直接在命令行窗口中输出预处理后的代码.

gcc的-E选项，可以让编译器在预处理后停止，并输出预处理结果。在本例中，预处理结果就是将stdio.h 文件中的内容插入到test.c中了。

##### 2.2编译为汇编代码(Compilation)

预处理之后，可直接对生成的test.i文件编译，生成汇编代码：

    gcc -S test.i -o test.s

gcc的-S选项，表示在程序编译期间，在生成汇编代码后，停止，-o输出汇编代码文件。

##### 2.3汇编(Assembly)

对于上一小节中生成的汇编代码文件test.s，gas汇编器负责将其编译为目标文件，如下：

    gcc -c test.s -o test.o

##### 2.4连接(Linking)

gcc连接器是gas提供的，负责将程序的目标文件与所需的所有附加的目标文件连接起来，最终生成可执行文件。附加的目标文件包括静态连接库和动态连接库。

对于上一小节中生成的test.o，将其与Ｃ标准输入输出库进行连接，最终生成程序test

    gcc test.o -o test

 

在命令行窗口中，执行./test, 让它说HelloWorld吧！

#### 3多个程序文件的编译

通常整个程序是由多个源文件组成的，相应地也就形成了多个编译单元，使用GCC能够很好地管理这些编译单元。假设有一个由test1.c和 test2.c两个源文件组成的程序，为了对它们进行编译，并最终生成可执行程序test，可以使用下面这条命令：

gcc test1.c test2.c -o test

如果同时处理的文件不止一个，GCC仍然会按照预处理、编译和链接的过程依次进行。如果深究起来，上面这条命令大致相当于依次执行如下三条命令：

    gcc -c test1.c -o test1.o
    gcc -c test2.c -o test2.o
    gcc test1.o test2.o -o test

#### 4检错

    gcc -pedantic illcode.c -o illcode

-pedantic编译选项并不能保证被编译程序与ANSI/ISO C标准的完全兼容，它仅仅只能用来帮助Linux程序员离这个目标越来越近。或者换句话说，-pedantic选项能够帮助程序员发现一些不符合 ANSI/ISO C标准的代码，但不是全部，事实上只有ANSI/ISO C语言标准中要求进行编译器诊断的那些情况，才有可能被GCC发现并提出警告。

除了-pedantic之外，GCC还有一些其它编译选项也能够产生有用的警告信息。这些选项大多以-W开头，其中最有价值的当数-Wall了，使用它能够使GCC产生尽可能多的警告信息。

    gcc -Wall illcode.c -o illcode

GCC给出的警告信息虽然从严格意义上说不能算作错误，但却很可能成为错误的栖身之所。一个优秀的Linux程序员应该尽量避免产生警告信息，使自己的代码始终保持标准、健壮的特性。所以将警告信息当成编码错误来对待，是一种值得赞扬的行为！所以，在编译程序时带上-Werror选项，那么GCC会在所有产生警告的地方停止编译，迫使程序员对自己的代码进行修改，如下：

    gcc -Werror test.c -o test

 

#### 5库文件连接

开发软件时，完全不使用第三方函数库的情况是比较少见的，通常来讲都需要借助许多函数库的支持才能够完成相应的功能。从程序员的角度看，函数库实际上就是一些头文件（.h）和库文件（so、或lib、dll）的集合。。虽然Linux下的大多数函数都默认将头文件放到/usr/include/目录下，而库文件则放到/usr/lib/目录下；Windows所使用的库文件主要放在Visual Stido的目录下的include和lib，以及系统文件夹下。但也有的时候，我们要用的库不再这些目录下，所以GCC在编译时必须用自己的办法来查找所需要的头文件和库文件。

例如我们的程序test.c是在linux上使用c连接mysql，这个时候我们需要去mysql官网下载MySQL Connectors的C库，下载下来解压之后，有一个include文件夹，里面包含mysql connectors的头文件，还有一个lib文件夹，里面包含二进制so文件libmysqlclient.so

其中inclulde文件夹的路径是/usr/dev/mysql/include,lib文件夹是/usr/dev/mysql/lib

 

##### 5.1编译成可执行文件

首先我们要进行编译test.c为目标文件，这个时候需要执行

    gcc &ndash;c &ndash;I /usr/dev/mysql/include test.c &ndash;o test.o

##### 5.2链接

最后我们把所有目标文件链接成可执行文件:

    gcc &ndash;L /usr/dev/mysql/lib &ndash;lmysqlclient test.o &ndash;o test

Linux下的库文件分为两大类分别是动态链接库（通常以.so结尾）和静态链接库（通常以.a结尾），二者的区别仅在于程序执行时所需的代码是在运行时动态加载的，还是在编译时静态加载的。

##### 5.3强制链接时使用静态链接库

默认情况下， GCC在链接时优先使用动态链接库，只有当动态链接库不存在时才考虑使用静态链接库，如果需要的话可以在编译时加上-static选项，强制使用静态链接库。

在/usr/dev/mysql/lib目录下有链接时所需要的库文件libmysqlclient.so和libmysqlclient.a，为了让GCC在链接时只用到静态链接库，可以使用下面的命令:

    gcc &ndash;L /usr/dev/mysql/lib &ndash;static &ndash;lmysqlclient test.o &ndash;o test

 

静态库链接时搜索路径顺序：

1. ld会去找GCC命令中的参数-L
2. 再找gcc的环境变量LIBRARY_PATH
3. 再找内定目录 /lib /usr/lib /usr/local/lib 这是当初compile gcc时写在程序内的

动态链接时、执行时搜索路径顺序:

1. 编译目标代码时指定的动态库搜索路径
2. 环境变量LD_LIBRARY_PATH指定的动态库搜索路径
3. 配置文件/etc/ld.so.conf中指定的动态库搜索路径
4. 默认的动态库搜索路径/lib
5. 默认的动态库搜索路径/usr/lib

有关环境变量：
LIBRARY_PATH环境变量：指定程序静态链接库文件搜索路径
LD_LIBRARY_PATH环境变量：指定程序动态链接库文件搜索路径

**gcc编译C++程序**

单个源文件生成可执行程序

下面是一个保存在文件 helloworld.cpp 中一个简单的 C++ 程序的代码：

    /* helloworld.cpp */

    #include <iostream>

    int main(int argc,char *argv[])

    {

        std::cout << "hello, world" << std::endl;

        return(0);

    }

程序使用定义在头文件 iostream 中的 cout，向标准输出写入一个简单的字符串。该代码可用以下命令编译为可执行文件：

 

    $  g++ helloworld.cpp

编译器 g++ 通过检查命令行中指定的文件的后缀名可识别其为 C++ 源代码文件。编译器默认的动作：编译源代码文件生成对象文件(object file)，链接对象文件和 libstdc++ 库中的函数得到可执行程序。然后删除对象文件。由于命令行中未指定可执行程序的文件名，编译器采用默认的 a.out。程序可以这样来运行：

    $ ./a.out

    hello, world

更普遍的做法是通过 
-o 选项指定可执行程序的文件名。下面的命令将产生名为 helloworld 的可执行文件： 

    $ g++; helloworld.cpp -o helloworld

在命令行中输入程序名可使之运行： 

    $ ./helloworld

    hello, world

程序 g++ 是将 gcc 默认语言设为 C++ 的一个特殊的版本，链接时它自动使用 C++ 标准库而不用 C 标准库。通过遵循源码的命名规范并指定对应库的名字，用 gcc 来编译链接 C++ 程序是可行的，如下例所示：

    $ gcc helloworld.cpp -lstdc++ -o helloworld

选项 -l (ell) 通过添加前缀 lib 和后缀 .a 将跟随它的名字变换为库的名字 libstdc++.a。而后它在标准库路径中查找该库。gcc 的编译过程和输出文件与 g++ 是完全相同的。

 

在大多数系统中，GCC 安装时会安装一名为 c++ 的程序。如果被安装，它和 g++ 是等同，如下例所示，用法也一致：

    $ c++ helloworld.cpp -o helloworld

*多个源文件生成可执行程序*

如果多于一个的源码文件在 g++ 命令中指定，它们都将被编译并被链接成一个单一的可执行文件。下面是一个名为 speak.h 的头文件；它包含一个仅含有一个函数的类的定义：

    /* speak.h */

    #include <iostream>

    class Speak
    {
        public:
            void sayHello(const char *);
    };

下面列出的是文件 speak.cpp 的内容：包含 sayHello() 函数的函数体：

    /* speak.cpp */

    #include "speak.h"

    void Speak::sayHello(const char *str)
    {

        std::cout << "Hello " << str << "/n";

    }

文件 hellospeak.cpp 内是一个使用 Speak 类的程序：

    /* hellospeak.cpp */

    #include "speak.h"

    int main(int argc,char *argv[])
    {
        Speak speak;

        speak.sayHello("world");

        return(0);
    }

下面这条命令将上述两个源码文件编译链接成一个单一的可执行程序：

    $ g++ hellospeak.cpp speak.cpp -o hellospeak

PS：这里说一下为什么在命令中没有提到“speak.h“该文件（原因是：在“speak.cpp“中包含有”#include"speak.h"“这句代码，它的意思是搜索系统头文件目录之前将先在当前目录中搜索文件“speak.h“。而”speak.h“正在该目录中，不用再在命令中指定了）。

*源文件生成对象文件*

选项 -c 用来告诉编译器编译源代码但不要执行链接，输出结果为对象文件。文件默认名与源码文件名相同，只是将其后缀变为 .o。例如，下面的命令将编译源码文件 hellospeak.cpp 并生成对象文件 hellospeak.o：

    $ g++ -c hellospeak.cpp

命令 g++ 也能识别 .o 文件并将其作为输入文件传递给链接器。下列命令将编译源码文件为对象文件并将其链接成单一的可执行程序：

$ 
g++ -c hellospeak.cpp

$ 
g++ -c speak.cpp

$ 
g++ hellospeak.o speak.o -o hellospeak

选项 -o 不仅仅能用来命名可执行文件。它也用来命名编译器输出的其他文件。例如：除了中间的对象文件有不同的名字外，下列命令生将生成和上面完全相同的可执行文件：

    $ g++ -c hellospeak.cpp -o hspk1.o

    $ g++ -c speak.cpp -o hspk2.o

    $ g++ hspk1.o hspk2.o -o hellospeak

*编译预处理*

选项 -E 使 g++ 将源代码用编译预处理器处理后不再执行其他动作。下面的命令预处理源码文件 helloworld.cpp 并将结果显示在标准输出中：

    $ g++ -E helloworld.cpp

本文前面所列出的 helloworld.cpp 的源代码，仅仅有六行，而且该程序除了显示一行文字外什么都不做，但是，预处理后的版本将超过 1200 行。这主要是因为头文件 iostream 被包含进来，而且它又包含了其他的头文件，除此之外，还有若干个处理输入和输出的类的定义。

预处理过的文件的 GCC 后缀为 .ii，它可以通过 -o 选项来生成，例如：

    $ gcc -E helloworld.cpp -o helloworld.ii

*生成汇编代码*

选项 -S 指示编译器将程序编译成汇编语言，输出汇编语言代码而后结束。下面的命令将由 C++ 源码文件生成汇编语言文件 helloworld.s：

    $ g++ -S helloworld.cpp

生成的汇编语言依赖于编译器的目标平台。 

*创建静态库
*

静态库是编译器生成的一系列对象文件的集合。链接一个程序时用库中的对象文件还是目录中的对象文件都是一样的。库中的成员包括普通函数，类定义，类的对象实例等等。静态库的另一个名字叫归档文件(archive)，管理这种归档文件的工具叫 ar 。

在下面的例子中，我们先创建两个对象模块，然后用其生成静态库。

头文件 say.h 包含函数 sayHello() 的原型和类 Say 的定义：

    /* say.h */

    #include <iostream>

    void sayhello(void);

    class Say {
        private:
            char *string;

        public:
            Say(char *str)
        {
            string = str;
        }
        
        void sayThis(const char *str)
        {
            std::cout << str << " from a static library/n";
        }

        void sayString(void);
    };

下面是文件 say.cpp 是我们要加入到静态库中的两个对象文件之一的源码。它包含 Say 类中 sayString() 函数的定义体；类 Say 的一个实例 librarysay 的声明也包含在内：

    /* say.cpp */

    #include "say.h"

    void Say::sayString()

    {

        std::cout << string << "/n";

    }

Say librarysay("Library instance of Say");源码文件 syshello.cpp 是我们要加入到静态库中的第二个对象文件的源码。它包含函数 sayhello() 的定义：

    /* sayhello.cpp */

    #include "say.h"

    void sayhello()

    {

        std::cout << "hello from a static library/n";

    }

下面的命令序列将源码文件编译成对象文件，命令 ar 将其存进库中：

    $ g++ -c sayhello.cpp

    $ g++ -c say.cpp

    $ ar -r libsay.a sayhello.o say.o

程序 ar 配合参数 -r 创建一个新库 libsay.a 并将命令行中列出的对象文件插入。采用这种方法，如果库不存在的话，参数 -r 将创建一个新的库，而如果库存在的话，将用新的模块替换原来的模块。

下面是主程序 saymain.cpp，它调用库 libsay.a 中的代码：

    /* saymain.cpp */
    #include "say.h"

    int main(int argc,char *argv[])

    {

        extern Say librarysay;

        Say localsay = Say("Local instance of Say");

        sayhello();
        librarysay.sayThis("howdy");
        librarysay.sayString();
        localsay.sayString();
        return(0);
    }

该程序可以下面的命令来编译和链接： 

    $ g++ saymain.cpp libsay.a -o saymain

程序运行时，产生以下输出： 

hello from a static library

howdy from a static library

Library instance of SayLocal instance of Say

参考链接:
http://blog.csdn.net/shanxiao528/article/details/5578743
http://www.cnblogs.com/ggjucheng/archive/2011/12/14/2287738.html
http://objccn.io/issue-6-3/