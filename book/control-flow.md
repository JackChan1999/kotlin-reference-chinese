---
type: doc
layout: reference
category: "Syntax"
title: "控制流：if、when、for、while"
---

# 控制流：if、when、for、while

## If 表达式

在 Kotlin 中，*if*{: .keyword }是一个表达式，即它会返回一个值。
因此就不需要三元运算符（条件 ? 然后 : 否则），因为普通的 *if*{: .keyword } 就能胜任这个角色。

``` kotlin
// 传统用法
var max = a 
if (a < b) max = b

// With else 
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}
 
// 作为表达式
val max = if (a > b) a else b
```

*if*{: .keyword }的分支可以是代码块，最后的表达式作为该块的值：

``` kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```

如果你使用 *if*{: .keyword } 作为表达式而不是语句（例如：返回它的值或者<!--
-->把它赋给变量），该表达式需要有 `else` 分支。

参见 [*if*{: .keyword } 语法](grammar.html#if)。

## When 表达式

*when*{: .keyword } 取代了类 C 语言的 switch 操作符。其最简单的形式如下：

``` kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // 注意这个块
        print("x is neither 1 nor 2")
    }
}
```

*when*{: .keyword } 将它的参数和所有的分支条件顺序比较，直到某个分支满足条件。
*when*{: .keyword } 既可以被当做表达式使用也可以被当做语句使用。如果它被当做表达式，
符合条件的分支的值就是整个表达式的值，如果当做语句使用，
则忽略个别分支的值。（像 *if*{: .keyword } 一样，每一个分支可以是一个代码块，它的值<!--
-->是块中最后的表达式的值。）

如果其他分支都不满足条件将会求值 *else*{: .keyword } 分支。
如果 *when*{: .keyword } 作为一个表达式使用，则必须有 *else*{: .keyword } 分支，
除非编译器能够检测出所有的可能情况都已经覆盖了。

如果很多分支需要用相同的方式处理，则可以把多个分支条件放在一起，用逗号分隔：

``` kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

我们可以用任意表达式（而不只是常量）作为分支条件

``` kotlin
when (x) {
    parseInt(s) -> print("s encodes x")
    else -> print("s does not encode x")
}
```

我们也可以检测一个值在（*in*{: .keyword }）或者不在（*!in*{: .keyword }）一个[区间](ranges.html)或者集合中：

``` kotlin
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```

另一种可能性是检测一个值是（*is*{: .keyword }）或者不是（*!is*{: .keyword }）一个特定类型的值。注意：
由于[智能转换](typecasts.html#智能转换)，你可以访问该类型的方法和属性而无需<!--
-->任何额外的检测。

```kotlin
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```

*when*{: .keyword } 也可以用来取代 *if*{: .keyword }-*else*{: .keyword } *if*{: .keyword }链。
如果不提供参数，所有的分支条件都是简单的布尔表达式，而当一个分支的条件为真时则执行该分支：

``` kotlin
when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}
```

参见 [*when*{: .keyword } 语法](grammar.html#when)。


## For 循环

*for*{: .keyword } 循环可以对任何提供迭代器（iterator）的对象进行遍历，这相当<!--
-->于像 C# 这样的语言中的 `foreach` 循环。语法如下：

``` kotlin
for (item in collection) print(item)
```

循环体可以是一个代码块。

``` kotlin
for (item: Int in ints) {
    // ……
}
```

如上所述，*for*{: .keyword } 可以循环遍历任何提供了迭代器的对象。即：

* 有一个成员函数或者扩展函数 `iterator()`，它的返回类型
  * 有一个成员函数或者扩展函数 `next()`，并且
  * 有一个成员函数或者扩展函数 `hasNext()` 返回 `Boolean`。

这三个函数都需要标记为 `operator`。

对数组的 `for` 循环会被编译为并不创建迭代器的基于索引的循环。

如果你想要通过索引遍历一个数组或者一个 list，你可以这么做：

``` kotlin
for (i in array.indices) {
    print(array[i])
}
```

注意这种“在区间上遍历”会编译成优化的实现而不会创建额外对象。

或者你可以用库函数 `withIndex`：

``` kotlin
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

参见[*for*{: .keyword } 语法](grammar.html#for)。

## While 循环

*while*{: .keyword } 和 *do*{: .keyword }..*while*{: .keyword } 照常使用

``` kotlin
while (x > 0) {
    x--
}

do {
  val y = retrieveData()
} while (y != null) // y 在此处可见
```

参见[*while*{: .keyword } 语法](grammar.html#while).

## 循环中的Break和continue

在循环中 Kotlin 支持传统的 *break*{: .keyword } 和 *continue*{: .keyword } 操作符。参见[返回和跳转](returns.html)。
