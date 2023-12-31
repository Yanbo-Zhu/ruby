
本章中我们学习了作用域，使用`class`、`module`、`def`会开启作用域门 ，使用`Class.new`、`Module.new`、`define_method`可以跨越作用域门。]
了解了闭包的概念，在闭包外定义的变量可以进入闭包内部使用，以及可以使用分号让外部的变量不可以进入闭包。

# 1 Ruby 的作用域 Scope

作用域存在于任何编程语言中，如果不够了解作用域，经常会出现变量未定义、错误分配变量值等等问题，本章节中会对 Ruby 的作用域做深度剖析。

## 1.1 作用域是什么

作用域就是变量的有效使用范围。当提到作用域的时候，您应该了解，在Ruby中任何一行在该上下文中，哪些变量可用，哪些变量不可用。

有人会说，那么让变量在全局的任何地方都可用不就好了，如使用全局变量（_Global Variable_），这样就不会因为作用域而烦恼。

但是当您从事了编程工作一段时间后，您会发现，**全局变量非常地不可控**，任何人都有能力修改这个变量，这个变量究竟是被谁读了、被谁写了，问题一单产生很难追踪。并且全局变量要求我们的**命名必须不同**，这样的话在一个项目中，可能会出现上千个变量名。

在之前Ruby的变量章节中我们讲解了4种变量的类型。现在按照作用域大小来划分他们：

1. 全局变量（_Global Variable_）的作用域包括顶级作用域、每一个类、实例、局部（任意地方）；
    
2. 类变量（_Class Variable_）的作用域包括类、实例、局部；
    
3. 实例变量（_Instance Variable_）作用域包括实例、局部；
    
4. 局部变量（_Local Variable_）作用域仅有局部。
    

## 1.2 顶级作用域

顶级作用域（_Top Level Scope_）意味着您未调用任何方法，或者所有的方法都已经返回。简单来讲，当我们刚打开`irb`或在一个没有任何类或方法的Ruby脚本之中，我们所处的就是顶级作用域。

在Ruby中一切皆为对象，即使您处于顶级作用域，也位于一个对象之中，它的名字叫做`main`，属于类`Object`。

下面是`irb`的示例：

```ruby
$irb
> puts self
main
=> nil
> puts self.class
Object
=> nil

```

或在一个空脚本中输入：

```ruby
p self
p self.class

# ---- 输出结果 ----
main
Object

```

## 1.3 作用域门

当我们执行下面三种操作的时候会打开作用域门（_Scope Gate_），进入一个全新的作用域（完全不同的上下文）：

1. 定义一个类（`class SomeClass`）；
2. 定义一个模块（`module SomeModule`）；
3. 定义一个方法（`def some_method`）。

我们用一个局部变量的例子来解释作用域门的概念。

局部变量有这样的特性，当我们输出一个一个未定义的局部变量会抛出 NameError 的异常。

**实例：**
```ruby
puts a

# ---- 输出结果 ----
undefined local variable or method `a' for main:Object (NameError)

```


> **Tips**：实例变量和全局变量拥有默认值，为`nil`。

当我们在作用域范围内为局部变量定义，不管代码是否执行，Ruby 的解释器都会将这个局部变量放入作用域。

**实例：**

```ruby
if false
  a = 1 # 代码不执行，但是Ruby的解释器将局部变量a放入了当前作用域
end
p a # nil 代码未执行，因此未初始化

# ---- 输出结果 ----
nil

```

我们可以通过`local_variables`这个方法来获取当前作用域中所有的局部变量。

**实例：**

```ruby 
v0 = 0
def a_method
  v1 = 1
  p local_variables
end
a_method
p local_variables

# ---- 输出结果 ----
[:v1]
[:v0]

```

**解释**：当我们定义 v0 的时候，v0 在顶级作用域中，然后我们定义了`a_method`开启了一个新的作用域，之后在`a_method`里面定义了 v1 变量，因为作用域门的限制，v0 并不会在`a_method`的作用域里面，因此局部变量列表只打印了 v1 变量。在调用完`a_method`方法之后，我们输出了顶级作用域的局部变量列表，显示了而当前作用域只有 v0 在作用域内部，所以只打印了变量 v0。

和`def`一样，`module`和`class`也会打开作用域门，创建一个新的作用域，外部局部变量无法进行访问。

## 1.4 跨越作用域门

当使用`module`、`class`、`def`来定义模块、类、方法的时候会产生作用域门，大大限制了局部变量的使用范围，那么我们有没有一种方式来跨越作用域门呢？

答案是有的，我们需要改变一下定义模块、类、方法的方式，不使用关键字，而使用方法去定义它们：

1. 定义类：`Class.new`；
2. 定义模块：`Module.new`；
3. 定义方法：`define_method`。

让我们改写一下上面的例子：

**实例：**

```ruby 
v0 = 0
define_method :a_method do
  v1 = 1
  p local_variables
end
a_method
p local_variables

# ---- 输出结果 ----
[:v1, :v0]
[:v0]

```

**解释**：从输出结果我们可以看到，v0 成功跨越了作用域门进入到了`a_method`里面。同时，变量 v1仍然只在方法的作用域里面。

## 1.5 闭包

什么是闭包（_Closure_），简言之在块的作用域外面定义的变量可以在块整个生命周期进行访问，Ruby 有三种形式的闭包：Block、Proc、Lambda。Block可以将代码块传给方法，Proc 和 Lambda 可以把代码块存储在变量之中。（关于 Block 请看 _Ruby 的块_ 章节，Proc 和 Lambda 请看 _Proc 和Lambda 章节_）。

因此闭包并不是作用域门。

从跨越作用域门的例子中可以看到，定义类、模块、方法的三种形式均使用到了块，它允许引用作用域外的变量并开启了新的作用域。

**实例：**

```ruby 
num = 1
(1..3).each do |i|
  num += i
end
p num
# ---- 输出结果 ----

7

```

**解释**：由上面的例子我们可以看到在块中我们拿到了变量`num`，并且执行了操作。

那要是不希望块访问到外部变量要怎么办呢，我们有下面这种形式。

**实例：**

```ruby 
hello = 'Hello'
hi = 'Hi'
1.times do |i; hi, hello|
  p i
  hello = 'Hello 2'
  hi = 'Hi 2'
end
p hello
p hi
# ---- 输出结果 ----

Hello
Hi

```

**解释**：从输出结果我们可以看到，我们并没有修改了外部变量。我们在块参数的末尾放置了一个分号（`;`），然后追加我们不希望访问到的外部变量名称，就可以做到不去访问外部变量。

如果我们去掉`; hi, hello`的话，我们会得到`Hello Hi`的结果。


