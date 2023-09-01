
在并发编程中常用的实用程序类。该包包括一些小型标准化可扩展框架，以及一些提供有用功能且繁琐或难以实现的类。以下是主要组件的简要说明。另 java.util.concurrent.locks 请参阅 和 java.util.concurrent.atomic 包。
Executors
接口。Executor是一个简单的标准化接口，用于定义自定义的类线程子系统，包括线程池、异步 I/O 和轻量级任务框架。根据所使用的具体 Executor 类，任务可以在新创建的线程、现有任务执行线程或线程调用execute中执行，并且可以按顺序或并发执行。 提供了更完整的异步任务执行框架。 ExecutorService执行器服务管理任务的排队和计划，并允许受控关闭。ScheduledExecutorService子接口和关联的接口增加了对延迟和定期任务执行的支持。ExecutorServices 提供的方法安排异步执行任何函数，表示为 Callable，Runnable结果承载模拟。A Future 返回函数的结果，允许确定执行是否已完成，并提供取消执行的方法。A RunnableFuture 是一个Future拥有run在执行时设置其结果的方法。
实现。类ThreadPoolExecutor并提供ScheduledThreadPoolExecutor可调、灵活的线程池。该Executors类为最常见的执行程序类型和配置提供了工厂方法，以及一些用于使用它们的实用工具方法。其他基于 的实用程序包括提供 Futures 的通用可扩展实现的具体类FutureTask，以及 ，它ExecutorCompletionService有助于协调异步任务组的Executors处理。
类提供了一个执行器，主要用于处理 及其子类 ForkJoinPool 的 ForkJoinTask 实例。这些类采用窃取工作的计划程序，该调度程序为符合计算密集型并行处理中通常存在的限制的任务获得高吞吐量。
Queues
该 ConcurrentLinkedQueue 类提供高效的可扩展线程安全非阻塞 FIFO 队列。该 ConcurrentLinkedDeque 类类似，但另外支持 java.util.Deque 接口。
中的java.util.concurrent五个实现支持扩展BlockingQueue接口，该接口定义了 put 和 take 的阻塞版本：LinkedBlockingQueue、、ArrayBlockingQueueSynchronousQueue、 PriorityBlockingQueue和 DelayQueue。不同的类涵盖了生产者-消费者、消息传递、并行任务和相关并发设计的最常见使用上下文。
扩展接口 TransferQueue和实现 LinkedTransferQueue 引入了一个同步 transfer 方法（以及相关功能），其中生产者可以选择阻塞等待其消费者。
该 BlockingDeque 接口扩展 BlockingQueue 以支持 FIFO 和 LIFO（基于堆栈）操作。类 LinkedBlockingDeque 提供了一个实现。
定时
该 TimeUnit 类提供多个粒度（包括纳秒）来指定和控制基于超时的操作。包中的大多数类除了包含无限期等待之外，还包含基于超时的操作。在使用超时的所有情况下，超时指定方法在指示超时之前应等待的最短时间。实现会“尽最大努力”在超时发生后尽快检测超时。但是，在检测到超时和线程在该超时后再次实际执行之间可能会经过无限期的时间。所有接受超时参数的方法都将小于或等于零的值视为根本不等待。要“永远”等待，可以使用值 Long.MAX_VALUE。
Synchronizers
五个类有助于常见的专用同步习惯用法。
Semaphore 是一个经典的并发工具。
CountDownLatch 是一个非常简单但非常常见的实用程序，用于阻塞，直到给定数量的信号、事件或条件保持不变。
A CyclicBarrier 是可复位的多路同步点，在某些样式的并行编程中很有用。
A Phaser 提供了一种更灵活的屏障形式，可用于控制多个线程之间的分阶段计算。
A Exchanger 允许两个线程在集合点交换对象，在多个管道设计中很有用。
并发集合
除了队列之外，此包还提供专为在多线程上下文中使用而设计的集合实现：ConcurrentHashMap、 CopyOnWriteArrayListConcurrentSkipListMapConcurrentSkipListSet和 。CopyOnWriteArraySet当许多线程需要访问给定的集合时，a 通常优于 sync，a ConcurrentHashMap ConcurrentSkipListMap 通常优于 HashMapTreeMapsync。当预期的读取和遍历数大大超过列表的更新数时，A CopyOnWriteArrayList 比同步ArrayList更可取。
此包中某些类使用的“并发”前缀是一个简写，表示与类似的“同步”类的几个差异。例如 java.util.Hashtable ，并且 Collections.synchronizedMap(new HashMap()) 是同步的。但是 ConcurrentHashMap 是“并发”的。并发集合是线程安全的，但不受单个排除锁的约束。在ConcurrentHashMap的特殊情况下，它安全地允许任意数量的并发读取以及可调数量的并发写入。当您需要阻止通过单个锁对集合的所有访问时，“同步”类可能很有用，但代价是可伸缩性较差。在多个线程需要访问公共集合的其他情况下，“并发”版本通常更可取。当集合未共享或仅在持有其他锁时才能访问时，最好使用未同步集合。
大多数并发集合实现（包括大多数队列）也与通常 java.util 的约定不同，因为它们的 迭代器和 Spliterator 提供 弱一致性 而不是快速失败的遍历：
它们可以与其他操作同时进行
他们永远不会扔 ConcurrentModificationException
它们保证遍历施工时存在的元素一次，并且可能（但不保证）反映施工后的任何修改。
内存一致性属性
Java语言规范的第17章 定义了内存操作（如共享变量的读取和写入）的发生前关系。仅当写入操作发生在读取操作之前，保证一个线程的写入结果对另一个线程的读取可见。和构造以及 Thread.start() and Thread.join() 方法可以形成发生前关系。volatile synchronized特别：
线程中的每个操作都会在线程中的每个操作 之前发生， 该操作按程序的顺序稍后出现。
监视器的解锁（synchronized 阻止或方法退出） 发生 - 在同一 监视器的每个后续锁定（synchronized 块或方法进入）之前。由于发生 前 关系是可传递的，因此线程在解锁之前的所有操作都会在任何线程锁定该监视器之后的所有操作 之前发生 。
对字段的 volatile 写入发生在每次后续读取同一字段 之前 。字段的 volatile 写入和读取具有与进入和退出监视器类似的内存一致性效果， 但不涉及互 斥锁定。
在线程上调用 - start 在启动的线程中的任何操作 之前发生 。
线程中的所有操作 都会在 任何其他线程从该线程成功 join 返回之前发生。
中 java.util.concurrent 所有类及其子包的方法将这些保证扩展到更高级别的同步。特别：
在将对象放入任何并发集合之前，线程中的操作 发生在 另一个线程中的集合中访问或删除该元素之后的操作。
在将 提交到 Executor a Runnable 之前线程中的操作 - 在其执行开始之前。同样，对于Callables提交到 ExecutorService.
异步计算执行的操作由在另一个线程中检索Future.get()结果之后的发生之前操作表示Future。
“发布”同步器方法（如 、）之前的操作，以及成功“获取”方法（如Lock.unlockLock.lock、、Semaphore.releaseSemaphore.acquireCondition.await）CountDownLatch.await之后的发生前操作，以及CountDownLatch.countDown另一个线程中同一同步器对象之后的操作。
对于通过 成功Exchanger交换对象的每对线程，在每个线程中之前发生的操作发生在另一个线程中相应的exchange()操作exchange()之后。
调用CyclicBarrier.await之前的操作和Phaser.awaitAdvance（及其变体）在屏障操作执行的操作之前发生，屏障操作执行的操作发生在从其他线程中的相应await线程成功返回之后的操作之前发生。
因为：
1.5