##### 测试代码
```java
package com.pigcanfly.niukewang;

/**
 * Class Describe
 *
 * @author Tobby Quinn
 * @date 2019/02/22
 */
public class IntegerCache {
    @SuppressWarnings("all")
    public static void main(String[] args) {
        Integer i1 = new Integer(122);
        Integer i2 = new Integer(122);
        System.out.println(i1==i2); //false；new 指令每次生成新的引用地址
        //Auto Boxing.Invoke method valueOf()
        Integer i3 = 123;
        Integer i4 = 123;
        System.out.println(i3==i4); //true; Integer内部类IntegerCache缓存了[-128,127]的整型数据,此时i3与i4都是相同对象
        Integer i5 = 128;
        Integer i6 = 128;
        System.out.println(i5==i6); //false;
        //Auto Unboxing. Invoke method intValue()
        int i7 = new Integer(12);
    }
}
```
##### Integer源码
```java
/**
 * 自动装箱(int基本类型)
 */
public static Integer valueOf(int i) {
      if (i >= IntegerCache.low && i <= IntegerCache.high)
          return IntegerCache.cache[i + (-IntegerCache.low)];
      return new Integer(i);
  }

/**
 * 自动拆箱(int基本类型)
 */
 public int intValue() {
      return value;
  }
/**
 * IntegerCache源码
 */
 private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k < cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
        }

        private IntegerCache() {}
    }
```
##### 测试代码字节码
```txt
Classfile /Users/tobbyquinn/IdeaProjects/concurrency/src/main/java/com/pigcanfly/niukewang/IntegerCache.class
  Last modified 2019年2月22日; size 875 bytes
  MD5 checksum 097e7822dd679b5d897c20206340428f
  Compiled from "IntegerCache.java"
public class com.pigcanfly.niukewang.IntegerCache
  minor version: 0
  major version: 54
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #8                          // com/pigcanfly/niukewang/IntegerCache
  super_class: #9                         // java/lang/Object
  interfaces: 0, fields: 0, methods: 2, attributes: 1
Constant pool:
   #1 = Methodref          #9.#21         // java/lang/Object."<init>":()V
   #2 = Class              #22            // java/lang/Integer
   #3 = Methodref          #2.#23         // java/lang/Integer."<init>":(I)V
   #4 = Fieldref           #24.#25        // java/lang/System.out:Ljava/io/PrintStream;
   #5 = Methodref          #18.#26        // java/io/PrintStream.println:(Z)V
   #6 = Methodref          #2.#27         // java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
   #7 = Methodref          #2.#28         // java/lang/Integer.intValue:()I
   #8 = Class              #29            // com/pigcanfly/niukewang/IntegerCache
   #9 = Class              #30            // java/lang/Object
  #10 = Utf8               <init>
  #11 = Utf8               ()V
  #12 = Utf8               Code
  #13 = Utf8               LineNumberTable
  #14 = Utf8               main
  #15 = Utf8               ([Ljava/lang/String;)V
  #16 = Utf8               StackMapTable
  #17 = Class              #31            // "[Ljava/lang/String;"
  #18 = Class              #32            // java/io/PrintStream
  #19 = Utf8               SourceFile
  #20 = Utf8               IntegerCache.java
  #21 = NameAndType        #10:#11        // "<init>":()V
  #22 = Utf8               java/lang/Integer
  #23 = NameAndType        #10:#33        // "<init>":(I)V
  #24 = Class              #34            // java/lang/System
  #25 = NameAndType        #35:#36        // out:Ljava/io/PrintStream;
  #26 = NameAndType        #37:#38        // println:(Z)V
  #27 = NameAndType        #39:#40        // valueOf:(I)Ljava/lang/Integer;
  #28 = NameAndType        #41:#42        // intValue:()I
  #29 = Utf8               com/pigcanfly/niukewang/IntegerCache
  #30 = Utf8               java/lang/Object
  #31 = Utf8               [Ljava/lang/String;
  #32 = Utf8               java/io/PrintStream
  #33 = Utf8               (I)V
  #34 = Utf8               java/lang/System
  #35 = Utf8               out
  #36 = Utf8               Ljava/io/PrintStream;
  #37 = Utf8               println
  #38 = Utf8               (Z)V
  #39 = Utf8               valueOf
  #40 = Utf8               (I)Ljava/lang/Integer;
  #41 = Utf8               intValue
  #42 = Utf8               ()I
{
  public com.pigcanfly.niukewang.IntegerCache();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 9: 0

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=3, locals=8, args_size=1
         0: new           #2                  // class java/lang/Integer
         3: dup
         4: bipush        122
         6: invokespecial #3                  // Method java/lang/Integer."<init>":(I)V
         9: astore_1
        10: new           #2                  // class java/lang/Integer
        13: dup
        14: bipush        122
        16: invokespecial #3                  // Method java/lang/Integer."<init>":(I)V
        19: astore_2
        20: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
        23: aload_1
        24: aload_2
        25: if_acmpne     32
        28: iconst_1
        29: goto          33
        32: iconst_0
        33: invokevirtual #5                  // Method java/io/PrintStream.println:(Z)V
        36: bipush        123
        38: invokestatic  #6                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
        41: astore_3
        42: bipush        123
        44: invokestatic  #6                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
        47: astore        4
        49: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
        52: aload_3
        53: aload         4
        55: if_acmpne     62
        58: iconst_1
        59: goto          63
        62: iconst_0
        63: invokevirtual #5                  // Method java/io/PrintStream.println:(Z)V
        66: sipush        128
        69: invokestatic  #6                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
        72: astore        5
        74: sipush        128
        77: invokestatic  #6                  // Method java/lang/Integer.valueOf:(I)Ljava/lang/Integer;
        80: astore        6
        82: getstatic     #4                  // Field java/lang/System.out:Ljava/io/PrintStream;
        85: aload         5
        87: aload         6
        89: if_acmpne     96
        92: iconst_1
        93: goto          97
        96: iconst_0
        97: invokevirtual #5                  // Method java/io/PrintStream.println:(Z)V
       100: new           #2                  // class java/lang/Integer
       103: dup
       104: bipush        12
       106: invokespecial #3                  // Method java/lang/Integer."<init>":(I)V
       109: invokevirtual #7                  // Method java/lang/Integer.intValue:()I
       112: istore        7
       114: return
      LineNumberTable:
        line 12: 0
        line 13: 10
        line 14: 20
        line 16: 36
        line 17: 42
        line 18: 49
        line 19: 66
        line 20: 74
        line 21: 82
        line 23: 100
        line 24: 114
      StackMapTable: number_of_entries = 6
        frame_type = 255 /* full_frame */
          offset_delta = 32
          locals = [ class "[Ljava/lang/String;", class java/lang/Integer, class java/lang/Integer ]
          stack = [ class java/io/PrintStream ]
        frame_type = 255 /* full_frame */
          offset_delta = 0
          locals = [ class "[Ljava/lang/String;", class java/lang/Integer, class java/lang/Integer ]
          stack = [ class java/io/PrintStream, int ]
        frame_type = 255 /* full_frame */
          offset_delta = 28
          locals = [ class "[Ljava/lang/String;", class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer ]
          stack = [ class java/io/PrintStream ]
        frame_type = 255 /* full_frame */
          offset_delta = 0
          locals = [ class "[Ljava/lang/String;", class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer ]
          stack = [ class java/io/PrintStream, int ]
        frame_type = 255 /* full_frame */
          offset_delta = 32
          locals = [ class "[Ljava/lang/String;", class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer ]
          stack = [ class java/io/PrintStream ]
        frame_type = 255 /* full_frame */
          offset_delta = 0
          locals = [ class "[Ljava/lang/String;", class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer, class java/lang/Integer ]
          stack = [ class java/io/PrintStream, int ]
}
SourceFile: "IntegerCache.java"

```
