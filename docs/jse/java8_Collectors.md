# 都说java8特性Stream强大，可别只盯着Stream，而忽略了Collector收集器

><font color = black size = 4>在Stream实例方法中就有这么一个方法<R,A> R collect(Collector<? super T,A,R> collector)，是不是觉得有点陌生而又熟悉的感觉。
 实际生产中，我们基本不会直接创建这么一个让人头大的对象，更多是使用Collectors这个工具类去创建合适的Collector接口的实现类。呃，也不是不能创建，只能说迫不得已就别用吧.
 </font>
 
 ><font color = black size = 4>接下来让我们先看看Collector的官方解释：</font>

 ```java
  public interface Collector<T,A,R>
   //A mutable reduction operation that accumulates input elements into a mutable result container, optionally transforming the accumulated result into a 
   //final representation after all input elements have been processed. Reduction operations can be performed either sequentially or in parallel.
 ```

<h1> 1、method </h1>

 *  BiConsumer<A,T> accumulator()
 *  Set<Collector.Characteristics> characteristics()
 *  <font>BinaryOperator<A> combiner()</font>
 *  Function<A,R> finisher()
 *  static <T,A,R> Collector<T,A,R> 	of(Supplier<A> supplier, BiConsumer<A,T> accumulator, BinaryOperator<A> combiner, Function<A,R> finisher, Collector.Characteristics... characteristics)
 *  static <T,R> Collector<T,R,R> of(Supplier<R> supplier, BiConsumer<R,T> accumulator, BinaryOperator<R> combiner, Collector.Characteristics... characteristics)
 * Supplier<A> supplier()


<h1>2、Collectors</h1>

  * static <T> Collector<T,?,Double> averagingDouble(ToDoubleFunction<? super T> mapper)
  * static <T> Collector<T,?,Double> averagingInt(ToIntFunction<? super T> mapper)
  * static <T> Collector<T,?,Double> averagingLong(ToLongFunction<? super T> mapper)
  * static <T,A,R,RR> Collector<T,A,RR> collectingAndThen(Collector<T,A,R> downstream, Function<R,RR> finisher)
  * static <T> Collector<T,?,Long> counting()
  * static <T,K> Collector<T,?,Map<K,List<T>>> groupingBy(Function<? super T,? extends K> classifier)
  * static <T,K,A,D> Collector<T,?,Map<K,D>> groupingBy(Function<? super T,? extends K> classifier, Collector<? super T,A,D> downstream)
  * static <T,K,D,A,M extends Map<K,D>> Collector<T,?,M> groupingBy(Function<? super T,? extends K> classifier, Supplier<M> mapFactory, Collector<? super T,A,D> downstream)<
  * static <T,K> Collector<T,?,ConcurrentMap<K,List<T>>> groupingByConcurrent(Function<? super T,? extends K> classifier)
  * ……