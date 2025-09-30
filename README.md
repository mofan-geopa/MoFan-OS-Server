# MoFan-OS-Server
# Util
## 切换到util分支
```git checkout util```
### 其中的文件如下：
* kernel文件夹：里面包含了操作系统内核空间的源代码，负责底层的硬件管理和系统功能实现。
* user文件夹：里面包含了用户空间的应用程序源代码，运行在操作系统内核提供的环境上。
* Makefile文件：用于自动化构建的脚本文件，在这个项目中用来构建xv6操作系统。
* grade-lab-xxx文件：用于给实验评分的脚本文件。
### 一些常用命令：
+ 运行并构建xv6操作系统：```make qemu```
+ 退出xv6：```Ctrl-a x``` (先按Ctrl+a，再按x)
+ 测试是否完成lab：```make grade```
+ 测试是否完成lab的子任务：```make GRADEFLAGS=<lab name> grade```
  - 如在util lab中，想测试是否完成子任务sleep，运行```make GRADEFLAGS=sleep grade```
+ gdb调试
  - 一个终端执行```make CPUS=1 qemu-gdb```
  - 在另一个终端执行 ```riscv64-unknown-elf-gdb kernel/kernel```

## 编写代码
```
// user/sleep.c
#include "kernel/types.h"
#include "kernel/stat.h"
#include "user/user.h"

int main(int argc, char **argv) {
    if(argc < 2) { //检查用户是否输入了足够的参数
        printf("usage: sleep <ticks>\n");
    }
    sleep(atoi(argv[1]));
    exit(0);
}
```
**​​解析​​：**
argc(argument count): 整数，表示命令行参数的数量。​**​至少为 1​**​，因为第一个参数总是程序自身的名称。

argv(argument vector): 指针的指针（或字符串数组），存储了所有的命令行参数。argv[0]是程序名，argv[1]是第一个真正的参数，以此类推。

```sleep(atoi(argv[1]));```这是程序的核心功能，让程序休眠。
1. argv[1]: 获取命令行参数数组中的​​第二个元素​​（即用户输入的时间参数），它的类型是 char*（字符串）。例如，用户输入 ./my_sleep 5，那么 argv[1]就是字符串 "5"。
2. atoi(argv[1]): 调用 atoi(ASCII to Integer) 函数，将字符串参数 argv[1]​​转换成一个整数​​。例如，将字符串 "5"转换成整数 5。
3. sleep(...): 调用 sleep函数。该函数接受一个整数作为参数，表示程序需要暂停执行的​​秒数​​。sleep(5)就是让程序休眠 5 秒。

## 编译
1. 在Makefile文件中加入```$U/_sleep\```
2. 执行```make qemu```进行编译
3. 输入```sleep 100```进行测试