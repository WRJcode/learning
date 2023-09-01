
接口和类提供用于锁定和等待与内置同步和监视器不同的条件的框架。该框架允许在使用锁和条件时具有更大的灵活性，但代价是语法更加笨拙。
该 Lock 接口支持语义不同的锁定规则（重入、公平等），可用于非块结构上下文，包括交接和锁定重新排序算法。主要实现是 ReentrantLock。
该 ReadWriteLock 接口同样定义了可以在读取器之间共享但专用于写入器的锁。仅提供单个实现 ReentrantReadWriteLock，因为它涵盖了大多数标准使用上下文。但是程序员可以创建自己的实现来满足非标准需求。
该 Condition 接口描述可能与锁关联的条件变量。这些监视器的用法与使用 访问 Object.wait的隐式监视器类似，但提供了扩展功能。特别是，多个 Condition 对象可能与单个 Lock.为避免兼容性问题，方法的名称 Condition 与相应的 Object 版本不同。
该 AbstractQueuedSynchronizer 类充当有用的超类，用于定义依赖于排队阻塞线程的锁和其他同步器。该 AbstractQueuedLongSynchronizer 类提供相同的功能，但将支持扩展到 64 位同步状态。两者都扩展类，一个简单的类 AbstractOwnableSynchronizer，帮助记录当前保持独占同步的线程。该 LockSupport 类提供较低级别的阻止和取消阻止支持，这对于实现自己的自定义锁类的开发人员非常有用。
因为：
1.5

/*
 *
 *
 *
 *
 *
 * Written by Doug Lea with assistance from members of JCP JSR-166
 * Expert Group and released to the public domain, as explained at
 * http://creativecommons.org/publicdomain/zero/1.0/
 */

/**
 * Interfaces and classes providing a framework for locking and waiting
 * for conditions that is distinct from built-in synchronization and
 * monitors.  The framework permits much greater flexibility in the use of
 * locks and conditions, at the expense of more awkward syntax.
 *
 * <p>The {@link java.util.concurrent.locks.Lock} interface supports
 * locking disciplines that differ in semantics (reentrant, fair, etc),
 * and that can be used in non-block-structured contexts including
 * hand-over-hand and lock reordering algorithms.  The main implementation
 * is {@link java.util.concurrent.locks.ReentrantLock}.
 *
 * <p>The {@link java.util.concurrent.locks.ReadWriteLock} interface
 * similarly defines locks that may be shared among readers but are
 * exclusive to writers.  Only a single implementation, {@link
 * java.util.concurrent.locks.ReentrantReadWriteLock}, is provided, since
 * it covers most standard usage contexts.  But programmers may create
 * their own implementations to cover nonstandard requirements.
 *
 * <p>The {@link java.util.concurrent.locks.Condition} interface
 * describes condition variables that may be associated with Locks.
 * These are similar in usage to the implicit monitors accessed using
 * {@code Object.wait}, but offer extended capabilities.
 * In particular, multiple {@code Condition} objects may be associated
 * with a single {@code Lock}.  To avoid compatibility issues, the
 * names of {@code Condition} methods are different from the
 * corresponding {@code Object} versions.
 *
 * <p>The {@link java.util.concurrent.locks.AbstractQueuedSynchronizer}
 * class serves as a useful superclass for defining locks and other
 * synchronizers that rely on queuing blocked threads.  The {@link
 * java.util.concurrent.locks.AbstractQueuedLongSynchronizer} class
 * provides the same functionality but extends support to 64 bits of
 * synchronization state.  Both extend class {@link
 * java.util.concurrent.locks.AbstractOwnableSynchronizer}, a simple
 * class that helps record the thread currently holding exclusive
 * synchronization.  The {@link java.util.concurrent.locks.LockSupport}
 * class provides lower-level blocking and unblocking support that is
 * useful for those developers implementing their own customized lock
 * classes.
 *
 * @since 1.5
 */