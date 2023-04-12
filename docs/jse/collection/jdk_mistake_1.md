# HashMap实现了Map接口，同时继承了AbstractMap类?

>细心的人可能对HashMap类的定义有疑问，JDK源码中是这样定义的：
>public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable
>AbstractMap已经实现了Map接口，HashMap继承了AbstractMap，为什么还要实现Map接口呢？网络上有些细心的人对这样的写法有疑问，解答者大部分都站在了java一边，认为JDK这样写，肯定有他的道理。但是真正的原因，又很难找到。
>其实据java集合框架的创始人Josh Bloch描述，这样的写法是一个失误。在java集合框架中，类似这样的写法很多。stack overflow上面也进行过提问，而且找到了真正的答案，[答案出处](https://stackoverflow.com/questions/2165204/why-does-linkedhashsete-extend-hashsete-and-implement-sete?spm=a2c6h.12873639.article-detail.3.18612d444iTJt4)从这个问题我们可以看到，Josh Bloch承认这是一个失误，最开始写java集合框架的时候，他认为这样写，在某些地方可能是有价值的，直到他意识到错了。显然的，JDK的维护者，后来不认为这个小小的失误值得去修改。所以就这样存在下来了。