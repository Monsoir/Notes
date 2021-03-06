# Locks

Lock(锁) 是最常用的同步工具

通常用于保护一段关键的代码，这段代码在同一时刻只能有一条线程可以执行

## 锁的种类

- Mutex
- Recursive lock
- Read-write lock
- Distributed lock
- Spin lock
- Double-checked lock

### Mutex

互斥锁，全称：**MUT**ually **EX**clusive lock

- 其工作原理可以看作成是一个信号量，但这个信号量的库存只有 1
- 保证了一次只有一个线程可以访问到被保护的资源（代码）
- 当锁正在被使用时，若有另一个线程 T2 需要使用到被保护的资源，那么 T2 就会被阻塞，直至锁被释放
- 当同时有多个线程争夺受保护的资源时，也只会有一个线程可以申请到访问权

### Recursive lock

递归锁，是 Mutex 的一种变体

- 允许同一个线程，在释放锁之前多次申请同一个锁
    - 使用场合就是，在线程执行过程中需要进行循环迭代。这就出现了同一个线程中，多次申请同一个锁的情况
- 当有其他多个线程想要访问受保护资源时，还是需要等到锁被释放，在此期间，这些线程出于阻塞状态
- 对于同一个线程申请了 n 次递归锁，要彻底释放这个锁，就需要释放同样的 n 次

### Read-write lock

读写锁，这种锁适用于频繁读，偶尔写的程序，相对于其他锁，可以极大提高性能

工作原理

- 对于多个读操作，可以同时进行
- 对于写操作，在之前的读操作完成之前，都会一直处于阻塞状态
- 当有写操作在等待锁被释放时，此时若有新的读操作，那么这些读操作是需要等到读操作完成之后才能执行

> macOS 和 iOS 只支持 POSIX 的读写锁

### Distributed lock

分布式锁

Distributed lock 提供的是进程级别(process level)的 Mutex, 但与上面所说的 Mutex 不同，distributed lock 并不会阻塞或组织一个进程的运行，而是将锁的状态报告回对应的进程，由进程自己决定后面怎么继续

### Spin lock

自旋锁

工作原理

- 不断主动地查询锁状态(polling)，直到锁可用

适用于

- 经常使用于多处理器系统
- 且该系统对锁的等待时间很短
- 在这种情况下，主动查询锁状态比阻塞线程更高效
    - 阻塞线程需要「上下文切换」和「线程数据更新」的操作

但由于 polling 的特点， macOS 并不提供 Spin lock 的实现，而是交由开发者根据实际情况进行实现

### Double-checked lock

双重检查锁

Double-checked lock 目的在于获得锁之前减少开销，做法是通过测试锁的一些标准条件来判断是否可以获得锁

但是 Double-checked lock 有潜在的不安全性，因此并不鼓励使用，macOS 也没有提供明确的支持


