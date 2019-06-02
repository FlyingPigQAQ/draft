测试代码
```java
package com.pigcanfly.jvm;


import com.pigcanfly.concurrency.annotations.NotThreadSafe;
import com.pigcanfly.concurrency.annotations.ThreadSafe;

import java.util.Random;
import java.util.concurrent.ThreadLocalRandom;

/**
 * Class Describe
 *
 * @author Tobby Quinn
 * @date 2019/02/21
 */
public class RandomExamples {

    public static void main(String[] args) {
        Random random = new Random();
        random.nextInt(5);
        //两种方法异常的最大区别是：
        //线程不安全：只会调用一次localInit()方法，会导致多个线程的生成的随机数一致
        //线程安全：每个线程都会执行一次localInit()方法来保证每个线程生成的随机数不一致
        threadUnSafe();
        threadSafe();


    }

    @NotThreadSafe
    public static  void threadUnSafe() {
        //不能这样使用，线程不安全，多个线程的种子数一致(伪随机算法)
        ThreadLocalRandom threadLocalRandom = ThreadLocalRandom.current();
        for (int i = 0; i < 100; i++) {
            new Thread(() -> {
                System.out.println(threadLocalRandom.nextInt());
            }).start();
        }
    }
    @ThreadSafe
    public static void threadSafe(){
        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                System.out.println(ThreadLocalRandom.current().nextInt());
            }).start();
        }
    }
}

```

源码解析
```java
/**
   * Returns the current thread's {@code ThreadLocalRandom}.
   *
   * @return the current thread's {@code ThreadLocalRandom}
   */
  public static ThreadLocalRandom current() {
      if (UNSAFE.getInt(Thread.currentThread(), PROBE) == 0)
          localInit();
      return instance;
  }

  /**
    * Initialize Thread fields for the current thread.  Called only
    * when Thread.threadLocalRandomProbe is zero, indicating that a
    * thread local seed value needs to be generated. Note that even
    * though the initialization is purely thread-local, we need to
    * rely on (static) atomic generators to initialize the values.
    */
   static final void localInit() {
       int p = probeGenerator.addAndGet(PROBE_INCREMENT);
       int probe = (p == 0) ? 1 : p; // skip 0
       long seed = mix64(seeder.getAndAdd(SEEDER_INCREMENT));
       Thread t = Thread.currentThread();
       UNSAFE.putLong(t, SEED, seed);
       UNSAFE.putInt(t, PROBE, probe);
   }

```
