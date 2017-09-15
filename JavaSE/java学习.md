一.java 常见类，String,List,Set，Thread等源码分析与记录；

1.String 类；
1)String 类为final,不可被继承；
public final class String implements java.io.Serializable, Comparable<String>, CharSequence{
...
}
2）
/**
The value isused for character storage.
*/
private final char value[];
String 其实是基于字符数组 char[] 实现的。这是一个字符数组，并且是 final 类型，用于存储字符串内容。从 fianl 关键字可以看出，String 的内容一旦被初始化后，其不能被修改的。
3）
/**
Cache the hashcode for the string
*/
private int hash;  //Default to 0；
缓存字符串的 hash Code，其默认值为 0；
4）
/**
use serialVersionUID from JDK 1.0.2 for interoperability
*/
private static final long serialVersionUID = -6849794470754667710L;
/**
Class String is special cased with in the Serialization Stream Protocol.
*/
privates tatic final ObjectStreamField[] serialPersistentFields =  new ObjectStreamField[0]
因为 String 实现了 Serializable 接口，所以支持序列化和反序列化支持。Java 的序列化机制是通过在运行时判断类的 serialVersionUID 来验证版本一致性的。在进行反序列化时，JVM 会把传来的字节流中的 serialVersionUID 与本地相应实体（类）的
serialVersionUID 进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常(InvalidCastException)。

5）使用 StringBuffer 和 StringBuider 构造一个String，作为 String 的两个“兄弟”，StringBuffer 和 StringBuider 也可以被当做构造 String 的参数。关于效率问题，Java 的官方文档有提到说使用StringBuilder 的 toString 方法会更快一些，原因是StringBuffer 的 toString 方法是 synchronized 的，在牺牲了效率的情况下保证了线程安全。

6）一个特殊的保护类型的构造方法
String 除了提供了很多公有的供程序员使用的构造方法以外，还提供了一个保护类型的构造方法（Java 7），我们看一下他是怎么样的：
String(char[] value, boolean share) {
 // assert share : "unshared not supported";
 this.value = value;
}
从代码中我们可以看出，该方法和 String(char[] value) 有两点区别：
第一个，该方法多了一个参数：boolean share，其实这个参数在方法体中根本没被使用。注释说目前不支持 false，只使用 true。那可以断定，加入这个 share 的只是为了区分于 String(char[] value) 方法，不加这个参数就没办法定义这个函数，只有参数是不能才能进行重载。第二个区别就是具体的方法实现不同。我们前面提到过，String(char[] value) 方法在创建 String 的时候会用到 Arrays 的 copyOf 方法将value中的内容逐一复制到 String当中，而这个 String(char[] value, boolean share) 方法则是直接将value的引用赋值给String的value。那么也就是说，这个方法构造出来的 String 和参数传过来的 char[] value 共享同一个数组。为什么 Java 会提供这样一个方法呢？性能好。这个很简单，一个是直接给数组赋值（相当于直接将 String 的 value 的指针指向char[]数组），一个是逐一拷贝。当然是直接赋值快了。节约内存。该方法之所以设置为 protected，是因为一旦该方法设置为公有，在外面可以访问的话，如果构造方法没有对 arr 进行拷贝，那么其他人就可以在字符串外部修改该数组，由于它们引用的是同一个数组，因此对 arr 的修改就相当于修改了字符串，那就破坏了字符串的不可变性。安全的。对于调用他的方法来说，由于无论是原字符串还是新字符串，其 value 数组本身都是 String 对象的私有属性，从外部是无法访问的，因此对两个字符串来说都很安全。
7）Java7加入的新特性；
在 Java 7 之前有很多 String 里面的方法都使用上面说的那种“性能好的、节约内存的、安全”的构造函数。
比如：substring replace concat valueOf等方法。但是在 Java 7 中，substring已经不再使用这种“优秀”的方法了；public String substring(int beginIndex, int endIndex){
  if(beginIndex < 0){
    throw new StringIndexOutOfBoundsException(beginIndex);
  }
  if(endIndex > value.length){
    throw new StringIndexOutOfBoundsException(endIndex);
  }
  intsubLen = endIndex-beginIndex;
  if(subLen < 0){
    throw new StringIndexOutOfBoundsException(subLen);
  }
  return ((beginIndex == 0) && (endIndex == value.length)) ? this  : newString(value, beginIndex, subLen);
}
为什么呢？
虽然这种方法有很多优点，但是他有一个致命的缺点，对于 sun 公司的程序员来说是一个零容忍的
bug，那就是他很有可能造成内存泄露。
看一个例子，假设一个方法从某个地方（文件、数据库或网络）取得了一个很长的字符串，然后对其进行解析并提取其中的一小段内容，这种情况经常发生在网页抓取或进行日志分析的时候。
下面是示例代码。
String aLongString = "...averylongstring...";
StringaPart = data.substring(20, 40);
return aPart;

在这里 aLongString 只是临时的，真正有用的是
aPart，其长度只有 20个 字符，但是它的内部数组却是从 aLongString 那里共享的，因此虽然
aLongString 本身可以被回收，但它的内部数组却不能释放。
这就导致了内存泄漏。如果一个程序中这种情况经常发生有可能会导致严重的后果，如内存溢出，或性能下降。新的实现虽然损失了性能，而且浪费了一些存储空间，但却保证了字符串的内部数组可以和字符串对象一起被回收，从而防止发生内存泄漏，因此新的 substring 比原来的更健壮。

8）equals方法：
public boolean equals(Object anObject) {
if (this == anObject) {      
  return true;
     } 
    if (anObject instanceof String) {
       String anotherString = (String) anObject;
       int n = value.length;
if (n == anotherString.value.length) {
         char v1[] = value;
char v2[] = anotherString.value;  
int i = 0   
 while (n-- != 0) {
if (v1[i] != v2[i])
             
return false;        
 i++;
           }    
return true;
       }
   } 
   return false;
}

该方法首先判断this == anObject ？，也就是说判断要比较的对象和当前对象是不是同一个对象，如果是直接返回 true，如不是再继续比较，然后在判断
anObject 是不是 String
类型的，如果不是，直接返回 false，如果是再继续比较，到了能终于比较字符数组的时候，他还是先比较了两个数组的长度，不一样直接返回 false，一样再逐一比较值。 虽然代码写的内容比较多，但是可以很大程度上提高比较的效率。值得学习！！！
9）public int hashCode(){
  int h = hash;
  if(h == 0 && value.length > 0){
    char val[] = value;
    for(int i = 0; i < value.length; i++){
      h = 31 * h + val[i];
    }
    hash = h;
  }
  return h;
}
hashCode 的实现其实就是使用数学公式：s[0]31^(n-1) + s[1]31^(n-2) + ... + s[n-1]。所谓“冲突”，就是在存储数据计算hash地址的时候，我们希望尽量减少有同样的hash地址。如果使用相同 hash 地址的数据过多，那么这些数据所组成的 hash 链就更长，从而降低了查询效率。
所以在选择系数的时候要选择尽量长的系数并且让乘法尽量不要溢出的系数，因为如果计算出来的
hash 地址越大，所谓的“冲突”就越少，查找起来效率也会提高。现在很多虚拟机里面都有做相关优化，使用 31 的原因可能是为了更好的分配 hash 地址，并且 31 只占用 5 bits。在 Java 中，整型数是 32 位的，也就是说最多有2^32 = 4294967296 个整数，将任意一个字符串，经过 hashCode 计算之后，得到的整数应该在这
4294967296 数之中。那么，最多有 4294967297
个不同的字符串作 hashCode 之后，肯定有两个结果是一样的。hashCode 可以保证相同的字符串的 hash 值肯定相同，但是 hash 值相同并不一定是 value 值就相同。

10）字符串的switch是通过equals()和hashCode()方法来实现的。记住，switch中只能使用整型，比如byte，short，char(ackii码是整型)以及int。
还好hashCode()方法返回的是int而不是long。通过这个很容易记住hashCode返回的是int这个事实。仔细看下可以发现，进行switch的实际是哈希值，然后通过使用equals方法比较进行安全检查，这个检查是必要的，因为哈希可能会发生碰撞。因此性能是不如使用枚举进行switch或者使用纯整数常量，但这也不是很差。因为Java编译器只增加了一个equals方法，如果你比较的是字符串字面量的话会非常快，比如”abc” ==”abc”。如果你把hashCode()方法的调用也考虑进来了，那么还会再多一次的调用开销，因为字符串一旦创建了，它就会把哈希值缓存起来。
因此如果这个siwtch语句是用在一个循环里的，比如逐项处理某个值，或者游戏引擎循环地渲染屏幕，这里hashCode()方法的调用开销其实不会很大。其实swich只支持一种数据类型，那就是整型，其他数据类型都是转换成整型之后在使用switch的

11)String 总结：

一旦string对象在内存(堆)中被创建出来，就无法被修改。特别要注意的是，String类的所有方法都没有改变字符串本身的值，都是返回了一个新的对象。

如果你需要一个可修改的字符串，应该使用StringBuffer 或者 StringBuilder。否则会有大量时间浪费在垃圾回收上，因为每次试图修改都有新的string对象被创建出来。

如果你只需要创建一个字符串，你可以使用双引号的方式，如果你需要在堆中创建一个新的对象，你可以选择构造函数的方式；


二 List框架

一、 ArrayList概述：

public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable

ArrayList是基于数组实现的，是一个动态数组，其容量能自动增长，类似于C语言中的动态申请内存，动态增长内存。
ArrayList不是线程安全的，只能用在单线程环境下，多线程环境下可以考虑用Collections.synchronizedList(List l)函数返回一个线程安全的ArrayList类，也可以使用concurrent并发包下的CopyOnWriteArrayList类。
ArrayList实现了Serializable接口，因此它支持序列化，能够通过序列化传输，实现了RandomAccess接口，支持快速随机访问，实际上就是通过下标序号进行快速访问，实现了Cloneable接口，能被克隆。
每个ArrayList实例都有一个容量，该容量是指用来存储列表元素的数组的大小。它总是至少等于列表的大小。随着向ArrayList中不断添加元素，其容量也自动增长。自动增长会带来数据向新数组的重新拷贝，因此，如果可预知数据量的多少，可在构造ArrayList时指定其容量。在添加大量元素前，应用程序也可以使用ensureCapacity操作来增加ArrayList实例的容量，这可以减少递增式再分配的数量。

注意，此实现不是同步的。如果多个线程同时访问一个ArrayList实例，而其中至少一个线程从结构上修改了列表，那么它必须保持外部同步；

关于ArrayList的源码，给出几点比较重要的总结：

    1、注意其三个不同的构造方法。无参构造方法构造的ArrayList的容量默认为10，带有Collection参数的构造方法，将Collection转化为数组赋给ArrayList的实现数组elementData。
    2、注意扩充容量的方法ensureCapacity。ArrayList在每次增加元素（可能是1个，也可能是一组）时，都要调用该方法来确保足够的容量。当容量不足以容纳当前的元素个数时，就设置新的容量为旧的容量的1.5倍加1，如果设置后的新容量还不够，则 直接新容量设置为传入的参数（也就是所需的容量），而后用Arrays.copyof()方法将元素拷贝到新的数组（详见下面的第3点）。从中可以看出，当容量不够时，每次增加元素，都要将原来的元素拷贝到一个新的数组中，非常之耗时，也因此建议在事先能确定元素数量的情况下，才使用ArrayList，否则建议使用LinkedList。
    3、ArrayList的实现中大量地调用了Arrays.copyof()和System.arraycopy()方法。我们有必要对这两个方法的实现做下深入的了解。
    4、ArrayList基于数组实现，可以通过下标索引直接查找到指定位置的元素，因此查找效率高，但每次插入或删除元素，就要大量地移动元素，插入删除元素的效率低。
    5、在查找给定元素索引值等的方法中，源码都将该元素的值分为null和不为null两种情况处理，ArrayList中允许元素为null。


二：LinkedList；

public class LinkedList<E>  
    extends AbstractSequentialList<E>  
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable;


LinkedList的特性：
  1、是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。
  2、实现 List 接口，能对它进行队列操作。
  3、实现 Deque 接口，即能将LinkedList当作双端队列使用。
  4、实现了Cloneable接口，即覆盖了函数clone()，能克隆。
  5、实现java.io.Serializable接口，这意味着LinkedList支持序列化，能通过序列化去传输。
  6、是非同步的。
为什么要继承自AbstractSequentialList ?
    AbstractSequentialList 实现了get(int index)、set(int index, E element)、add(int index, E element) 和 remove(int index)这些骨干性函数。降低了List接口的复杂度。这些接口都是随机访问List的，LinkedList是双向链表；既然它继承于AbstractSequentialList，就相当于已经实现了“get(int index)这些接口”。
    此外，我们若需要通过AbstractSequentialList自己实现一个列表，只需要扩展此类，并提供 listIterator() 和 size() 方法的实现即可。若要实现不可修改的列表，则需要实现列表迭代器的 hasNext、next、hasPrevious、previous 和 index 方法即可。




















