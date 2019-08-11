# decaf编译器报告

标签（空格分隔）： 操作系统

Author： 计62 徐晟 2016011253

---

## decaf编译器目标

- 完成一个decaf-mips32编译器
- 编译decaf程序到MIPS32程序，并且在ucore上运行这个mips32程序

## decaf编译器完成过程

- 因为decaf编译器是基于编译原理课的作业的，而我在编译原理课上已经完成到了decaf lab5，即通过简单的图染色来分配寄存器，然后通过三地址码生成mips程序，所以这个学期的需要新加的内容并不太多
- 首先对`fib.decaf`进行编译得到`fib.S`，这个程序可以在SPIM上运行并输出
- 但是`fib.S`因为缺少链接和ucore-thumips相关的系统调用，所以并不能在ucore上运行，我参考ucore的piggy的方法，调用了`kernel.ld`，并且替换了头文件，在ucore-thumips的`Makefile`文件中加入了转成二进制的`fib`文件，就能够成功运行了

### decaf程序运行结果
`fib.decaf`的源代码为
```decaf
class Main {

    // the main entry
    static void main() {
        int i;
        class Fibonacci F;

        F = new Fibonacci();    // creates a new Fibonacci object
        i = 0;                  // for i from 0 to 9, prints F_i
        while (i < 10) {
            Print (F.get(i), "\n");
            i = i + 1;
        }
    }
}

class Fibonacci {

    // gets F_i
    int get(int index) {
        if (index < 2) {
            return 1;
        }
        return get(index - 1) + get(index - 2);
    }
}
```

`fib`程序在ucore-thumips上运行的结果为
```
1
1
2
3
5
8
13
21
34
55
89
144
233
377
610
987
1597
2584
4181
6765
```
说明fib程序编译并且链接成功，可以运行