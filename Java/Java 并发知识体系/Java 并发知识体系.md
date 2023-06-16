## 知识导读



#### 理论基础

1. 多线程的出现是为了解决什么问题？
2. 线程不安全是指什么？举例说明。
3. 并发出现线程不安全的本质是神什么？ 可见性，原子性和有序性
4. Java是怎么解决并发问题的？ 3个关键字，JMM和Happens-Before
5. 线程安全是不是非真即假？ 不是的！
6. 线程安全有哪些实现的思路？
7. 如何理解并发和并行的区别？
8. 线程有哪几种状态，分别说明从一种状态到另一种状态转变有哪些方式？
9. 通常线程有哪种几种使用方式？
10. 基础线程机制有哪些？
11. 线程中断方式有哪些？
12. 线程的互斥同步方式有哪些？如何比较和选择？
13. 线程之间有哪些协作方式？
14. 关键字： synchronized？
15. 关键字： volatile
16. final详解



### 为什么需要多线程？

总所周知，CPU、内存、I/O设备的速度是有极大的差异的，为了合理利用CPU的高性能，平衡这三种的速度的差异，计算机体系结构、操作系统、编译程序都做出了贡献。

* CPU增加了缓存，以均衡与内存的速度差异； // 导致 `可见性`问题
* 操作系统增加了进城、线程，以分时复用CPU，进而均衡CPU与I/O设备的速度差异； // 导致`原子性`问题
* 编译程序优化指令执行次序，使得缓存能够得到更加合理地利用； // 导致`有序性`问题



### 线程不安全示例

如果多个线程对同一个共享数据进行访问而不采取同步操作的话，那么操作的值结果可能是不一致的。

```java
public class ThreadUnsafeExample {
  private int cnt = 0;
  public void add() {
    cnt++;
  }
  
  public int get() {
    return cnt;
  }
}
```

```java
public static void main(String[] args) throws InterruptedException {
  final int threadSize = 1000;
  ThreadUnsafeExample example = new ThredUnsafeExample();
  final CountDownLatch countDownLatch = new CountDownLatch(threadSize);
  ExecutorService executorService = Executors.newCatchedThreadPool();
  for(int i = 0; i < threadSize; i++) {
    executorService.execute(() -> {
      example.add();
      countDownLatch.countDown();
    })
  }
  countDownLatch.await();
  executorService.shutdown();
  System.out.println(example.get());
  
}
```





