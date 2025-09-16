---
title: JAVA学习笔记
tags: JAVA
abbrlink: 48651
date: 2019-08-08 15:12:52
---

java俺也只是略有了解，不常用，做个笔记备忘，说不定以后要用到。
<!-- more -->

# JAVA基本语法

  ## 输入输出

* 输入：  
```java
 import java.util.Scanner;
public class Practice {
    public static void main(String[] args) {
        Scanner i = new Scanner(System.in);
         a = i.nextInt();/小数的话f = i.nextFloat();双精度则i.nextDouble()
        int b = i.nextInt();
        System.out.println(a*b);
    }
}
```
 若要输入字符串则直接用i.next() or i.nxetLine()
  ```text
    next():
  1.一定要读取到有效字符后才可以结束输入。
  2.对输入有效字符之前遇到的空白，next()方法会自动将其去掉。
  3.只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。
  4.next()不能得到带有空格的字符串。
  
  nextLine()：
  1.以Enter为结束符,nextLine()方法返回的是输入回车之前的所有字符。
  2.可以获得空白。
  ```
* 输出:  
  (1) print将它的参数显示在命令窗口，并将输出光标定位在所显示的最后一个字符之后。  
  (2) println 将它的参数显示在命令窗口，并在结尾加上换行符，将输出光标定位在下一行的开始。  
  (3)printf 是格式化输出的形式。(和python，matlab的%格式化输出差不多)  
  `System.out.println("Hello World") `  
  (4)输出个量，用+号相连。字符串必须用“ ”

  ---

  
  
  ## 基本运算
  
* 算术运算符：+ - * / %（求余）***操作数是都是整数时，相除结果也为整数***

* 数学函数：Math,使用时加上`Math.`即可，如`Math.sin(Math.PI)`

  | 常用函数  | 三角函数                  | 指数函数                               | 取整函数 |
  | --------- | ------------------------- | -------------------------------------- | -------- |
  | min(x)    | sin(r)、asin(r)           | exp(x) e的x次方                        | ceil(x)  |
  | max(x)    | cos(r)、acos(r)           | log(x)自然对数、log10(x)以10为底的对数 | floor(x) |
  | abs(x)    | tan(r)、atan(r)           | pow(x,y)                               | rint(x)  |
  | random(x) | toDegree(r)、toRadians(d) | sqrt(x)                                | round(x) |

  另外，`PI,E`分别表示π，e。 

* 关系运算符:==,!=,<,>,<=,>=

* 逻辑运算符:&&,| |,！ (与，或，非)

* 赋值运算符: 

| 操作符 | 描述 | 例子 |
| ------ | ---- | ---- |
|=	|简单的赋值运算符，将右操作数的值赋给左侧操作数|	C = A + B将把A + B得到的值赋给C|
|\+ =	|加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数|	C + = A等价于C |
|\- =	|减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数|  C - = A等价于C = C -A|
|\* =	|乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数|	C * = A等价于C = C * A|
|/=	|除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数| C / = A等价于C = C / A |
|（％）=	|取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数| C％= A等价于C = C％A |
|<< =	|左移位赋值运算符| C << = 2等价于C = C << 2 |
|>> =	|右移位赋值运算符| C >> = 2等价于C = C >> 2 |
|＆=	|按位与赋值运算符| C＆= 2等价于C = C＆2 |
|^ =	|按位异或赋值操作符| C ^ = 2等价于C = C ^ 2 |
|\|=	|按位或赋值操作符| C \ |
* 自增：  
  (1) 前缀自增自减法(++a,--a): 先进行自增或者自减运算，再进行表达式运算。  
  (2) 后缀自增自减法(a++,a--): 先进行表达式运算，再进行自增或者自减运算 

  ---

  
## 基本语句
### 循环语句
* while 循环  
`while( 布尔表达式 ) {
  //循环内容
}`
* do…while 循环  
`do {
       //代码语句
}while(布尔表达式);`
* for循环  
`for(初始化; 布尔表达式; 更新) {
    //代码语句
}` 
* 增强 for 循环(迭代循环)
`for(声明语句 : 表达式)
{
   //代码句子
}`
* break 关键字:终止该循环语句
* continue 关键字：跳过本次迭代，转跳至下一次迭代

### 判断语句
* `if(布尔表达式)
  {
   //如果布尔表达式为true将执行的语句
  }`

* `f(布尔表达式){
   //如果布尔表达式的值为true
}else{
   //如果布尔表达式的值为false
}`
   
*  
    ```java
        if(布尔表达式 1){//如果布尔表达式 1的值为true执行代码}
        else if(布尔表达式 2){//如果布尔表达式 2的值为true执行代码}
        else if(布尔表达式 3){//如果布尔表达式 3的值为true执行代码}
        else {//如果以上布尔表达式都不为true执行代码}
    ```
    
* 嵌套的 if…else 语句

当为判断条件后只有一句执行语句时，可不加{ }

### switch case 语句

 ```java
  switch(expression){
     case value :
        //语句
        break; //可选
     case value :
        //语句
        break; //可选
     //你可以有任意数量的case语句
     default : //可选， 无case匹配时默认执行
        //语句
 }
 ```
 (1) switch 语句中的变量类型可以是： byte、short、int 或者 char。从 Java SE 7 开始，switch 支持字符串 String 类型了，同时 case 标签必须为字符串常量或字面量。  
 (2) 当变量的值与 case 语句的值相等时，那么 case 语句之后的语句开始执行，直到 break 语句出现才会跳出 switch 语句。

  ## 数据类型

  #### 整数

  * byte (8位)
  * short (16位)
  * int (32位)
  * long (64位)

   #### 浮点数

  * float 单精度（32位）
  * double 双精度（64位）
    + 科学计数法110.23=1.1023e4，0.2234=2.234e-1

  ***括号里的位数代表它占存储的大小***

  #### 字符、字符串(char/String)

| 字符串方法                       | 字符串比较                            | 字符串、数值互转             |
| -------------------------------- | ------------------------------------- | :--------------------------- |
| length()                         | equals(s1)/equalsIgnoreCase(s1)       | Double.parseDouble(s1)       |
| trim() 去掉两边空白字符          | compareTo(s1)/compareIgnoreCase(s1)   | Integer.parseInt(s1)         |
| concat(s1) 连接字符串，相当于‘+’ | startsWith(s1)/endsWith(s1)           | String s = num + “” 转字符串 |
| charAt(index) 索引指定位置的字符 | contains(s1)                          |                              |
| toUpperCase()、toLowerCase()     | substring(start-index,stop-index)切片 |                              |

  索引字符串所在位置`indexOf(ch,form-index)`

  

  #### 一维数组

  **一个储存着相同类型数据的变量的列表**所以声明变量类型时在其后加`[]`，如`double[]`

  * 声明变量数组：`double[] mylist`
  
  * 创建数组：`double[] mylist = new double[10]`创建具有10个双精度数据的空数组，当然声明和创建可以一块做，就像变量一样`double[] mylist = new double[10] `
  
  * 数组赋值（初始化）:`mylist[0]=1;mylist[1]=2;...;mylist[9]=10`
  
  * 数组初始化语法：`double[] mylist = {1,2,3,4,5,6,7,8,9}`更简洁，不用加个new
  
  * 数组长度：`mylist.length`
  
  * foreach循环：`for (double e: mylist){System.out.println(e);}`步长只能为1，即历遍数组

  #### 二维数组

  * ​	声明，创建，初始化,赋值，索引以及处理和一维数组一样，把`[]`改为`[][]`即可
  
  * 初始化语法:
  
          ```java
    double[][] matrix = {{1,2,3},{4,5,6},{7,8,9}}
    ```

  #### 多维数组

  * 

  ## 方法

1. 定义方法：

   修饰符  返回值类型   方法名(参数列表){ 方法体 }      

   `public static int max(int num1,int num2){//执行语句}`

2. 调用方法

   方法名(参数列表)

   `max(2,3)`

3. 注意点
   * void表示无返回值类型
   * 形参与实参

  ## 其他注意事项

* 使用变量要先声明变量类型  
  `type identifier [ = value][, identifier [= value] ...] ;`  
  格式说明：type为Java数据类型。identifier是变量名。可以使用逗号隔开来声明多个同类型变量。

* 声明常量:` final datatype name`
  
* Java大小写敏感，方法名第一个单词小写往后的单词首字母大写；常量名全大写；类名首字母大写

* 特殊字符

  | 字符 |     描述     | 备注                                                         |
  | ---- | :----------: | ------------------------------------------------------------ |
  | { }  |    语句块    | 循环体中，若执行语句只有一句，可省略{ }                      |
  | （） | 和方法一起用 | { }do while();      for( ){}         while( ){ }   注意do while后的 ‘ ; ’ |
  | [ ]  |     数组     | 声明变量类型在类型后加[ ],初始化赋值用{1,23,3,4}的形式，表示大小用[10] |
  | ‘ ’  |     字符     | char                                                         |
  | “ ”  |    字符串    | String 变量类型中唯一一个大写开头的(其实他是java的一个内置类) |
  | //   |    行注释    | / 转义字符，\n表示换行，//表示注释行，/*  block */表示注释块 |
  | ；   | 标识语句结束 | 每一句结束后都要有，import也不例外                           |
  