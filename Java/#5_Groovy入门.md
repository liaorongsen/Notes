### Groovy


学习网址：https://www.ibm.com/developerworks/cn/education/java/j-groovy/j-groovy.html

无类型有什么意义
那么，Groovy 缺少类型意味着所需的输入更少。不可否认，将 String 替换成 def 并没有真正节约多少打字工作 — 三个字母并不值得如何夸耀！但是在更高的层次上看，在编写大量不仅仅包含变量声明的代码的时候，没有类型确实减少了许多打字工作。更重要的是，这意味着要阅读的代码要少得多。最后，Groovy 缺少类型能够带来更高的灵活性 — 不需要接口或抽象类。
所以，只需要使用 def 关键字就能在方法中声明一个独立变量，不需要将 def 关键字作为方法声明中的参数。在 for 循环声明中也不需要它，这意味着不用编写 (int x = 0; x < 5; x++)，相反，可以省略 int，保留空白。

能否看见闭包？
对于上面的代码，可以用更简洁的方式对集合进行迭代，如下所示：

def acoll = ["Groovy", "Java", "Ruby"]
         
acoll.each{
 println it
}
请注意，each 直接在 acoll 实例内调用，而 acoll 实例的类型是 ArrayList。在 each 调用之后，引入了一种新的语法 —{，然后是一些代码，然后是 }。由 {} 包围起来的代码块就是闭包。
闭包中的 it 变量是一个关键字，指向被调用的外部集合的每个值 — 它是默认值，可以用传递给闭包的参数覆盖它。下面的代码执行同样的操作，但使用自己的项变量：
def acoll = ["Groovy", "Java", "Ruby"]
         
acoll.each{ value ->
 println value
}
在这个示例中，用 value 代替了 Groovy 的默认 it。

闭包的更多使用方式
虽然在迭代上使用闭包的机会最多，但闭包确实还有其他用途。因为闭包是一个代码块，所以能够作为参数进行传递（Groovy 中的函数或方法不能这样做）。闭包在调用的时候才会执行这一事实（不是在定义的时候）使得它们在某些场合上特别有用。
例如，通过 Eclipse 创建一个 ClosureExample 对象，并保持它提供的默认类语法。在生成的 main() 方法中，添加以下代码：
def excite = { word ->
 return "${word}!!"
}
这段代码是名为 excite 的闭包。这个闭包接受一个参数（名为 word），返回的 String 是 word 变量加两个感叹号。请注意在 String 实例中替换 的用法。在 String 中使用 ${value}语法将告诉 Groovy 替换 String 中的某个变量的值。可以将这个语法当成 return word + "!!" 的快捷方式。

延迟执行
既然有了闭包，下面就该实际使用它了。可以通过两种方法调用闭包：直接调用或者通过 call() 方法调用。
继续使用 ClosureExample 类，在闭包定义下面添加以下两行代码：
1
2
assert "Groovy!!" == excite("Groovy")
assert "Java!!" == excite.call("Java")
可以看到，两种调用方式都能工作，但是直接调用的方法更简洁。不要忘记闭包在 Groovy 中也是一类对象 — 既可以作为参数传递，也可以放在以后执行。用普通的 Java 代码可以复制同样的行为，但是不太容易。现在不会感到惊讶了吧？

