
我们学习了在Ruby中学习了创建块了两种方式：`{}`以及`do...end`和如何在方法使用`yield`以及使用`call`来调用一个块。
今天我们学习将块保存到变量的两种形式：Labmbda 和Proc。

本章节我们学习了将块保存到变量的两种形式 Lambda 和 Proc，可以使用`proc`和`Proc.new`创建Proc，可以使用`lambda`和`->`创建 Lambda，调用它们要使用`call`，了解了 Proc 和 Lambda 本质都是一样的，都是`Proc`类的实例。以及如何接收参数，对`return`语句的处理。
# 1 Proc

## 1.1 定义

Proc 有两种表达方式：
```ruby
say_something = Proc.new { puts "This is a proc" }
```

另外一种是：
```ruby
say_something = proc { puts "This is a proc" }
```

## 1.2 调用

我们使用`call`来执行 Proc 的代码。

**实例：**

```ruby
say_something = -> { puts "This is a proc" }
say_something.call

# ---- 输出结果 ----
This is a proc

```

**注意事项**：Proc有很多种调用方式，但是我们只使用`call`，不使用其它的。

**实例：**
```ruby
my_proc = -> { puts "Proc called" }
my_proc.call
my_proc.()
my_proc[]
my_proc.===
  
# ---- 输出结果 ----
Proc called
Proc called
Proc called
Proc called

```
## 1.3 接收参数

第一种写法接收参数的形式为：
```ruby
times_two = Proc.new {|x| x * 2 }
times_two.call(10)

# ---- 输出结果 ----
20

```

第二种形式为：
```ruby
times_two = proc {|x| x * 2 }
times_two.call(10)

# ---- 输出结果 ----
20

```




# 2 Lambda

## 2.1 定义

Lambda 有两种表达方式：
say_something = -> { puts "This is a lambda" }

另外一种是：
say_something = lambda { puts "This is a lambda" }


## 2.2 调用

定义 Lambda 不会在其中运行代码，就像定义方法不会运行该方法一样，您需要为此使用`call`方法

```ruby
say_something = -> { puts "This is a lambda" }
say_something.call

# ---- 输出结果 ----
This is a lambda

```

**注意事项**：Lambda同样有很多种调用方式，但是我们依然只使用`call`。

```ruby
my_lambda = -> { puts "Lambda called" }
my_lambda.call
my_lambda.()
my_lambda[]
my_lambda.===
  
# ---- 输出结果 ----
Lambda called
Lambda called
Lambda called
Lambda called

```


## 2.3 接收参数

Lambda 也可以接受参数。


```ruby
times_two = ->(x) { x * 2 }
times_two.call(10)

# ---- 输出结果 ----
20

```

注意，两种 Lambda 的参数表现形式是不同的。
```ruby
times_two = lambda {|x| x * 2 }
times_two.call(10)

# ---- 输出结果 ----
20

```

如果将错误数量的参数传递给 Lambda，则将引发异常，就像常规方法一样。
```ruby
times_two = ->(x) { x * 2 }
times_two.call()

# ---- 输出结果 ----
ArgumentError (wrong number of arguments (given 0, expected 1)) 

```


# 3 比较 Proc 和 Lambda

## 3.1 创建形式不同，调用方法相同。

Lambda 和 Proc 本质上都`Proc`类的对象，Proc 类有一个实例方法名为`lambda?`

```ruby
puts proc { }.class
puts Proc.new {}.class
puts lambda {}.class
puts -> {}.class
  
# ---- 输出结果 ----
Proc
Proc
Proc
Proc

```


## 3.2 对于接收参数后是否传值反应不同

Proc 不在意是否真正传入参数，Lambda 要求调用时传入的参数必须和接收的参数保持一致，否则会抛出异常。

## 3.3 对于return语句的处理

Lambda 中使用`return`语句和常规方法一样。

**实例：**

```ruby
my_lambda = lambda { return 1 } 
puts "Lambda result: #{my_lambda.call}"

# ---- 输出结果 ----
Lambda result: 1

```

Proc 则在当前上下文中使用`return`。相当于在调用位置执行`return`。

**实例：**

```ruby
my_proc = proc { return 1 } 
puts "Proc result: #{my_proc.call}"
  
# ---- 没有输出结果 ----  

```

相当于：
```ruby
puts "Proc result: #{return 1}" 

# ---- 没有输出结果 ----

```



> **Tips**：在低于Ruby 2.5 版本中，我们这种写法会抛出异常
>
> unexpected return (LocalJumpError)。

所以我们在 Proc 中使用`return`的时候，一般结合方法来使用。

```ruby
def proc_method
  my_proc = proc { return 1 } 
  my_proc.call
end

puts "Proc result: #{proc_method}"
  
# ---- 输出结果 ---- 
Proc result: 1

```