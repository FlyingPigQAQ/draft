### 基于JDK1.8
[The Java® Virtual Machine Specification](https://docs.oracle.com/javase/specs/jvms/se8/html/index.html)
### 查看字节码指令信息
`javap -verbose XXX.class > XXX.txt`
### 指令相关解释
- 源文件

```java
package com.pigcanfly.niukewang;

/**
 * Class Describe
 *
 * @author Tobby Quinn
 * @date 2019/02/14
 */
public class Demo2 {

    public boolean demo2(){
        try{
            return true;
        }
        catch (Exception e){

        }finally {
            return false;
        }
    }

}
```

- 字节码指令文件

```java
Classfile /Users/tobbyquinn/IdeaProjects/concurrency/src/main/java/com/pigcanfly/niukewang/Demo2.class
  Last modified 2019年2月18日; size 380 bytes
  MD5 checksum e89d2bf85af37ac0776ce22d3e4d511e
  Compiled from "Demo2.java"
public class com.pigcanfly.niukewang.Demo2
  minor version: 0
  major version: 54
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #3                          // com/pigcanfly/niukewang/Demo2
  super_class: #4                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #4.#15         // java/lang/Object."<init>":()V
   #2 = Class              #16            // java/lang/Exception
   #3 = Class              #17            // com/pigcanfly/niukewang/Demo2
   #4 = Class              #18            // java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Utf8               Code
   #8 = Utf8               LineNumberTable
   #9 = Utf8               demo2
  #10 = Utf8               ()Z
  #11 = Utf8               StackMapTable
  #12 = Class              #19            // java/lang/Throwable
  #13 = Utf8               SourceFile
  #14 = Utf8               Demo2.java
  #15 = NameAndType        #5:#6          // "<init>":()V
  #16 = Utf8               java/lang/Exception
  #17 = Utf8               com/pigcanfly/niukewang/Demo2
  #18 = Utf8               java/lang/Object
  #19 = Utf8               java/lang/Throwable
{
  public com.pigcanfly.niukewang.Demo2();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 9: 0

  public boolean demo2();
    descriptor: ()Z                //()空参 Z:返回类型boolean
    flags: (0x0001) ACC_PUBLIC     //标识符为public
    Code:
      stack=1, locals=3, args_size=1     //操作数栈为1，本地局部变量表为3，参数为1（this指针）
         0: iconst_1                    //iconst_<i>,常量类型1进栈
         1: istore_1                    //istore_<n>,<n>为当前局部变量表的数组索引.此时操作数栈的栈顶元素必须为int类型;操作数栈pop->存放到局部变量表<n>位置
         2: iconst_0                    //常量类型0，进栈
         3: ireturn                    //return type boolean, byte, short, char, or int.如果没有报异常，则将本地操作数栈出栈pop，并入栈push到调用者的操作数栈中
         4: astore_1                   //栈顶ref对象出栈,并存入局部变量表索引为1位置
         5: iconst_0
         6: ireturn
         7: astore_2
         8: iconst_0
         9: ireturn
      Exception table:
         from    to  target type
             0     2     4   Class java/lang/Exception
             0     2     7   any
      LineNumberTable:               //字节码指令和源码文件行数对应表
        line 13: 0
        line 18: 2
        line 15: 4
        line 18: 5
      StackMapTable: number_of_entries = 2
        frame_type = 68 /* same_locals_1_stack_item */
          stack = [ class java/lang/Exception ]
        frame_type = 66 /* same_locals_1_stack_item */
          stack = [ class java/lang/Throwable ]
}
SourceFile: "Demo2.java"

```


### 相关属性参考文档
[Descriptors](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-4.html#jvms-FieldType)
