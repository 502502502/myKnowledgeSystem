





#### [点击链接跳转](https://blog.csdn.net/NSJim/article/details/109045914?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2-109045914-blog-83715440.pc_relevant_aa_2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2-109045914-blog-83715440.pc_relevant_aa_2&utm_relevant_index=5)





[点击跳转](https://blog.csdn.net/wait_for_eva/article/details/84307306)

可以根据声明的信息填写符号表，以下是可能的一份符号表信息：

| 标识符      | 类型           | 存储单元 | 作用域        | 偏移量 |
| ----------- | -------------- | -------- | ------------- | ------ |
| a           | int  [10] [10] | 100      | 全局数据区    | off0   |
| b           | student[10]    | 1000     | 全局数据区    | off100 |
| number      | int            | 1        | 结构体student | 0      |
| name        | char[20]       | 20       | 结构体student | 4      |
| achievement | float          | 2        | 结构体student | 24     |

其中，a和b是数组类型标识符，分别存储了整型二维数组和结构体数组。对于结构体中的变量，可以按照其在结构体中的相对偏移量进行存储，例如number的偏移量为0，name的偏移量为4（假设char类型占1个存储单元），achievement的偏移量为24（假设float类型占2个存储单元）。

需要注意的是，上述符号表信息可能并不完全准确，实际编译器实现中可能存在差异。



1、

| 标识符 | 类型   | 层数 | 偏移量 |
| ------ | ------ | ---- | ------ |
| main   | int () | 0    | -      |
| a      | int    | 1    | off0   |
| b      | int    | 1    | off1   |
| a      | float  | 2    | off2   |
| x      | float  | 3    | off3   |
| b      | float  | 2    | off4   |

2、

| 标识符 | 类型   | 层数 | 偏移量 |
| ------ | ------ | ---- | ------ |
| main   | int () | 0    | -      |
| a      | int    | 1    | off0   |
| b      | int    | 1    | off1   |

3、

| 标识符 | 类型   | 层数 | 偏移量 |
| ------ | ------ | ---- | ------ |
| main   | int () | 0    | -      |
| a      | int    | 1    | off-2  |
| b      | int    | 1    | off-1  |
| a      | float  | 2    | off-2  |
| x      | float  | 3    | off-2  |
| b      | float  | 2    | off-1  |



```c
x := 0;
s := x + 1;
i := 2 * s;

t1 := A[3];
t2 := t1[1];
t3 := t2 * A[i][1];
t4 := 1;
L1: if t4 < 100 goto L2;
     goto L3;
L2: k := t4;
     t5 := 2 * k;
     if t5 >= 50 goto L4;
          t6 := A[3];
          t7 := t6[t4];
          t8 := A[i];
          t9 := t8[t4];
          t10 := t7 * t9;
          s := s + t10;
     L4: t11 := 2;
          t4 := t5 * t11;
          goto L1;
L3: i := i + 1;
     if i < 10 goto L1;
```

在常量表达式优化和公共子表达式优化的基础上，计算 `A[3][k]` 的结果被存入了临时变量 `t6` 中，并在第二层循环中被重复利用，避免了重复计算；

同时，在第二层循环中，由于 `k` 的值每次乘以2，因此可以用移位运算来优化乘法操作，并将乘法语句的计算结果存入 `t10` 中，避免了重复计算。