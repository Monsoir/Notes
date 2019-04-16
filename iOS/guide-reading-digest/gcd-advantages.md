# 基于 GCD 的特点

- 简单直接的编程接口
- 自动化以及完善的线程池管理
- provide the speed of tuned assembly ?? 高速？
- 内存高效利用（线程栈信息不会遗留在应用内存空间）
- 不会引起内核陷入(trap)
- 异步分派任务到某个队列不会引起死锁问题
- 能够根据情况进行伸缩
- 维护数据同步时，串行队列提供了比使用同步原语技术更加高效的解决方案
