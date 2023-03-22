# 总结stream

 *参考:https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html*

   
><font size = 4>我们先从一个例子入门，这个例子是api文档中的，widgets是一个Collection<Widget>,通过 Collection.stream()方法获得stream，然后调用filter()方法并接受一个断言函数式接口参数过滤数据，之后通过mapToInt转换为Stream<Integer>流，最后通过sum()求和。</font>

```java
int sum = widgets.stream()
        .filter(w -> w.getColor() == RED)
        .mapToInt(w -> w.getWeight())
        .sum(); 
```

><font size = 4>看通过stream的这种链式操作的特性，处理数据是不是更得心应手？其中filter、map、mapToInt、skip这些称之为中间操作，其返回结果都是stream类型;
而collect、anyMatch、findFirst等终结操作才会真正开始操作数据，并且该stream将不可用。</font>
<font size = 4>如重复使用流则会返回异常：stream has already been operated upon or closed


>接下来让我们总结这里面的方法吧。<br>官方文档中对Stream的定义：A sequence of elements supporting sequential and parallel aggregate operations.</font>

```java
//All Superinterfaces:AutoCloseable, BaseStream<T,Stream<T>>
public interface Stream<T>
extends BaseStream<T,Stream<T>>
```

<h2> 1、Static Method</h2>

 <h3> 1.1、static <T> Stream.Builder<T> builder()</h3>

   ><font size = 4>返回一个构造器;改内置类有build()、add()、accept()方法</font>

 ```java
     /**add方法内部调用accept方法，返回this；build()之后，此builder不可用 */
     Stream.Builder<Integer> builder = Stream.builder();
        builder.add(1).add(2).add(3);
        builder.accept(5);
        Stream<Integer> intStream = builder.build();
        intStream.forEach(System.out::println);
 ```

 <h3> 1.3 static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)</h3>


  ><font size = 4>创建一个延迟连接的流，其元素是第一个流的所有元素，然后是第二个流的全部元素。
  ```java
        Stream.Builder<Integer> builder1 = Stream.builder();
        Stream.Builder<Integer> builder2 = Stream.builder();
        builder1.add(1).add(2).add(3);
        builder1.accept(5);
        Stream<Integer> stream1 = builder.build();
        builder2.add(4).add(9).add(11);
        Stream<Integer> stream2 = builder2.build();
        Stream<Integer> stream3 = Stream.concat(stream1,stream2);
        stream3.forEach(System.out::println);
  ```
 <h3>1.4、 static <T> Stream<T> empty()</h3>

  ><font size = 4>返回一个空的序列流</font>

  ```java
     ///空流可能有助于避免空指针异常
   // Creating an empty Stream
        Stream<String> stream = Stream.empty();
   // Displaying elements in Stream
        stream.forEach(System.out::println);
  ```
 <h3>1.5 static <T> Stream<T> generate(Supplier<T> s) </h3>

  ><font size = 4>generate方法返回一个无限连续的无序流，其中每个元素由提供的供应商(Supplier)生成。generate方法用于生成常量流和随机元素流</font>

  ```java
   //
   Stream<Integer> stream4 = Stream.generate(() -> new Random().nextInt(10));
   stream4.forEach(e -> System.out.println(e));

   System.out.println("--- IntStream ---");
   IntStream.generate(() -> new Random().nextInt()).limit(3)
	   .forEach(e -> System.out.println(e));
  ```
 <h3>1.6 static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)</h3>

  ><font size = 4>返回通过将函数f迭代应用于初始元素种子而产生的无限顺序流，产生由种子、f（种子）、f（f（种子（seed））等组成的流</font>

   ```java
     IntStream
        .iterate(2,t->t+2)
        .limit(5)
        .forEach(System.out::println);
   ```
 <h3>1.7 static <T> Stream<T> of(T... values)</h3>

  <font size = 4>Returns a sequential ordered stream whose elements are the specified values.</font>
 <h3>1.8 static <T> Stream<T> of(T values) </h3>

  >返回包含单个元素的顺序流
  ```java
   /**
    * of
   */
     IntStream.of(1,2,3,4,5,6).forEach(System.out::println);
     IntStream.of(5).forEach(System.out::println);
  ```

<h2>2、Instance Methods </h2>

 <h3>2.1 boolean allMatch(Predicate<? super T> predicate)、boolean anyMatch(Predicate<? super T> predicate)、boolean noneMatch(Predicate<? super T> predicate)</h3>

```java
    //allMatch、anyMatch
        IntStream stream1 = IntStream.of(1,3,5,7,8);
        IntStream stream2 = IntStream.of(1,3,5,7,8);
        Boolean allMatch = stream1.allMatch(t -> !(t/2==0));
        Boolean anyMatch = stream2.anyMatch(t -> !(t/2==0));
        System.out.println(allMatch);
        System.out.println(anyMatch);
```
 <h3>2.4 <R,A> R collect(Collector<? super T,A,R> collector)</h3>

  ><font size = 4>咦~这个参数什么鬼？收集者？我和大家一样，见到这个类又长又宽，像大碗面似的。Api文档中，定义是这样的；</font>

  ```java
    Interface Collector<T,A,R>
    Type Parameters:
     T - the type of input elements to the reduction operation
     A - the mutable accumulation type of the reduction operation (often hidden as an implementation detail)
     R - the result type of the reduction operation
    /**
     翻译过来就是：T:归约运算的输入元素类型;A:减少操作的可变积累操作；R：返回值
     */

     //static method
     static <T,A,R> Collector<T,A,R> of(Supplier<A> supplier, BiConsumer<A,T> accumulator, BinaryOperator<A> combiner, Function<A,R> finisher, Collector.Characteristics... characteristics)
     static <T,R> Collector<T,R,R> of(Supplier<R> supplier, BiConsumer<R,T> accumulator, BinaryOperator<R> combiner, Collector.Characteristics... characteristics)

     //除此之外，Collectors工具类中也有大量静态方法能生成常用的Collector对象，下次解读

     //Collector收集类有三种类型：恒等处理、规约处理、分组分区
  ```

  ```java
    /**
    * collect
    */
        Stream<Integer> stream3 = Stream.generate(() -> new Random().nextInt(10)).limit(100);
        //Double averaging = stream3.collect(Collectors.averagingInt(t -> t+5));
        Double averaging = stream3.collect(Collectors.averagingDouble(t -> t+5));
        System.out.println(averaging);
  ```
 <h3>2.5 <R> R collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner)</h3>

  ```java
   /**
         * <R> R collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner)
         */
        IntStream stream4 = IntStream.of(1,3,5,7,8);
        Integer result =  stream4.collect(
                () -> 5,
                (r,t) -> System.out.println(r+t),
                (r1,r2)->System.out.println(r1+r2));
        System.out.println(result);

         List<String> asList = stringStream.collect(ArrayList::new, ArrayList::add,                                             ArrayList::addAll);

         /*
          R result = supplier.get();  for (T element : this stream)      accumulator.accept(result, element);  return result;
         */

          Stream<String> stringStream = Stream.of("hello","world","we","are","family");
          String concat = stringStream.collect(StringBuilder::new, StringBuilder::append,StringBuilder::append)
                .toString();
          System.out.println(concat);
  ```
 <h3>2.6 long count()</h3>

  ```java
   IntStream stream1 = IntStream.of(1,3,5,7,8);
   long count = stream1.count();
   System.out.println(count);
  ```
 <h3>2.7 Stream<T> distinct()</h3>

  ```java
   IntStream stream1 = IntStream.of(1,3,3,7,7);
   stream1.distinct().forEach(System.out::println);
  ```
 <h3>2.8 Stream<T> filter(Predicate<? super T> predicate)</h3>

  ```java
   IntStream stream1 = IntStream.of(1,3,3,7,7);
   stream1.filter(t -> t==3).forEach(System.out::println);
  ```
 <h3>2.9 Optional<T> findAny()</h3>
 <h3>2.10 Optional<T> findFirst()</h3>

  ```java
   //IntStream stream1 = IntStream.of(1,3,3,7,7);
        IntStream stream1 = IntStream.empty();
        OptionalInt optional1 = stream1.findAny();
        System.out.println(optional1.orElse(10));

        IntStream stream2 = IntStream.of(900,3,3,7,7);
        OptionalInt optional2 = stream2.findFirst();
        System.out.println(optional2.orElse(10));
  ```
<h3>2.11 <R> Stream<R> flatMap(Function<? super T,? extends Stream<? extends R>> mapper)</h3>

  ```java
    /**
         * flatMap
         */
        Stream<String> stringStream = Stream.of("hello","world","we","are","family");
        Stream<Character> stringStream2 = stringStream
                .flatMap(t -> {
                    Stream.Builder builder = Stream.builder();
                    char[] chars = t.toCharArray();
                    for (char c:chars){
                        builder.add(c);
                    }
                    return builder.build();
                });
        stringStream2.forEach(System.out::println);
  ```
 <h3>2.12 DoubleStream flatMapToDouble(Function<? super T,? extends DoubleStream> mapper)</h3>
 <h3>2.13 IntStream flatMapToInt(Function<? super T,? extends IntStream> mapper)</h3>
 <h3>2.14 LongStream flatMapToLong(Function<? super T,? extends LongStream> mapper)</h3>
 <h3>2.15 void forEach(Consumer<? super T> action)</h3>
 <h3>2.16 void forEachOrdered(Consumer<? super T> action)</h3>

  ```java
   /**
         * forEach不保证顺序执行，后者保证按顺序执行。（顺序非排序哦）
         */
        Stream<Integer> stream3 = Stream.generate(() -> new Random().nextInt(10)).limit(15);
        stream3.forEach(System.out::print);
        System.out.println("--------");
        Stream<Integer> stream4 = Stream.generate(() -> new Random().nextInt(10)).limit(15);
        stream4.forEachOrdered(System.out::print);

        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        numbers.stream()
                .map(i -> i * i)
                .forEachOrdered(System.out::println);
  ```
 <h3>2.17 Stream<T> limit(long maxSize)</h3>
 <h3>2.18 <R> Stream<R> map(Function<? super T,? extends R> mapper)</h3>
 <h3>2.19 DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper)</h3>
 <h3>2.20 IntStream mapToInt(ToIntFunction<? super T> mapper)</h3>
 <h3>2.21 LongStream mapToLong(ToLongFunction<? super T> mapper)</h3>
 <h3>2.22 Optional<T> max(Comparator<? super T> comparator)</h3>
 <h3>2.23 Optional<T> min(Comparator<? super T> comparator)</h3>

  ```java
    /**
    * max\min
    */
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        Optional<Integer> min = numbers.stream().min(Integer::compareTo);
        System.out.println(min.orElse(null));
        Optional<Integer> max = numbers.stream().max(Integer::compareTo);
        System.out.println(max.orElse(null));
  ```
<h3>2.24 Stream<T> peek(Consumer<? super T> action)</h3>

  ```java
    /**
     * peek
     */
        Stream.of("one", "two", "three", "four")
                .filter(e -> e.length() > 3)
                .peek(e -> System.out.println("Filtered value: " + e))
                .map(String::toUpperCase)
                .peek(e -> System.out.println("Mapped value: " + e))
                .collect(Collectors.toList());
  ```
 <h3>2.25 Optional<T> reduce(BinaryOperator<T> accumulator)</h3>

  ```java
   /**
    * reduce
    */
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    Optional<Integer> optional = numbers.stream().reduce((integer, integer2) -> integer + integer2);
    System.out.println(optional.orElse(null));
    //输出25
  ```
 <h3>2.26 T reduce(T identity, BinaryOperator<T> accumulator)</h3>

  ```java
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    Integer sum1 = numbers.stream().reduce(10,(integer, integer2) -> integer + integer2);
    System.out.println(sum1);
    //输出25
  ```
 <h3>2.27 <U> U reduce(U identity, BiFunction<U,? super T,U> accumulator, BinaryOperator<U> combiner)</h3>

  ```java
   List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
   Integer sum2= numbers.parallelStream().reduce(10,(integer,integer2) -> integer + integer2,(integer, integer2) -> integer+integer2);
   System.out.println(sum2);
   //输出65
   //注意：在串行流中第三个参数不生效
  ```
 <h3>2.28 Stream<T> skip(long n)</h3>
 <h3>2.29 Stream<T> sorted()</h3>
 <h3>2.30 Stream<T> sorted(Comparator<? super T> comparator)</h3>

  ```java
   /**
    * sorted
    */
    List<Integer> numbers1 = Arrays.asList(6, 2, 8, 4, 3);
    //numbers1.stream().sorted().forEach(System.out::println);
    numbers1.stream().sorted(Integer::compareTo).forEach(System.out::println);
  ```
<h3>2.31 Object[] toArray()</h3>
<h3>2.32 <A> A[] toArray(IntFunction<A[]> generator)</h3>

 ```java
  /**
   * toArray
   */
    // Creating a Stream of Strings
    Stream<String> stream = Stream.of("Geeks", "for", "Geeks", "GeeksQuiz");
    // Using Stream toArray()
    Object[] arr = stream.toArray();
    // Displaying the elements in array arr
    System.out.println(Arrays.toString(arr));

    List<String> strs = Arrays.asList("a", "b", "c");
    String[] dd = strs.stream().toArray(str -> new String[strs.size()]);
    String[] dd1 = strs.stream().toArray(String[]::new);
    String[] dd2 = strs.toArray(new String[strs.size()]);
    System.out.println(Arrays.toString(dd));
 ```