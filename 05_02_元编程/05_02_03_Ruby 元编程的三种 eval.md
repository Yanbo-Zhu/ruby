
今天让我们来学习 Ruby 元编程的三种 eval：eval、instance_eval、class_eval。

今天我们学习了 Ruby 中的三个 eval，其中`eval`的扩展性最强，但最危险，`instance_eval`用于扩展类方法，`class_eval`用于扩展类的实例方法。
# 1 eval

eval可以将字符串作为代码进行执行，并返回代码的返回值。
它的使用方法是eval(code_string)。
code_string可以是完整的Ruby代码、或表达式。

```ruby
p eval("1 + 1")

# ---- 输出结果 ----
2
```

解释：在这里"1 + 1"是一个表达式的字符串，我们使用eval来将这个字符串当做表达式进行执行，得到返回值2.
当然，如果字符串里面代码运行报错也会抛出异常。


```ruby
eval(" puts a ")

# ---- 输出结果 ----

Traceback (most recent call last):
	2: from ruby.rb:1:in `<main>'
	1: from ruby.rb:1:in `eval'
ruby.rb:1:in `eval': undefined local variable or method `a' for main:Object (NameError)

```

让我们使用eval来定义一个类。
```ruby
eval <<-DEFINED_CLASS
  class Person
    def name
      'Andrew'
    end
  end
DEFINED_CLASS
person = Person.new
p person.name

# ---- 输出结果 ----
"Andrew"

```

**解释**：这里我们使用heredoc来定义了一个多行字符串，里面的内容是定义一个类`Person`，并在类里面定义了一个实例方法`name`。从最后输出结果来看，我们成功的创建了这个类以及对应的实例方法。

> **Tips**：`eval`功能非常强大，灵活度也很高，但是它很危险，有点类似于SQL注入。


比如我们定义了一个方法，可以打印出我们传入的东西。

```ruby
str = "Hi"

eval %Q{
  def method 
    puts #{str}
  end
}
method

# ---- 输出结果 ----
Hi

```

但是如果这个str可以被外界篡改，就会产生非常大的问题。

```ruby
str = "'Hi'; system('touch i_am_hack && echo \"I am hack\" > i_am_hack'); system('ls');"

eval %Q{
  def method 
    puts #{str}
  end
}
method

# ---- 输出结果 ----
Hi
你当前所在的目录结构!!!!

```

**解释**：当输出Hi的同时，我们创建了一个i_am_hack的文件，并且获取了执行脚本所在文件夹目录结构。

所以，请轻易不要使用`eval`。

> **Tips**：有时，eval = evil。



# 2 instance_eval

从字面意思上来看，我们可以理解为是专门为实例对象做的eval，这个想法是对的。


```ruby
class Person
end

person1 = Person.new
person2 = Person.new
person1.instance_eval do 
  def name
    'Andrew'
  end
end
p person1.name
p person2.name

# ---- 输出结果 ----
"Andrew"
Traceback (most recent call last):
ruby.rb:12:in `<main>': undefined method `name' for #<Person:0x00007fa0d0047f68> (NoMethodError)

```

**解释**：从上面的例子我们可以看到，我们对实例`person1`增加了一个`name`方法，增加的这个方法只作用到了person1上面，没有作用到person2上面，由此我们可以推断出，`instance_eval`的修改只是针对被操作的对象。

之前我们学习到，Ruby的类也是对象，那么我们可以对类进行`instance_eval`操作吗，答案是可以的


解释：因为所有的类都是Class类的实例，所有，我们在对类进行instance_eval时，相当于拓展了它的类方法。这种也是instance_eval最常用的功能。
```ruby
class Person
end

Person.instance_eval do 
  def name
    'Andrew'
  end
end

p Person.name

# ---- 输出结果 ----
"Andrew"

```


# 3 class_eval

根据方法名来看，我们猜测了这个是对类做的eval，没错确实是这样的，它常常用于给类添加实例方法。

```ruby
class Person
end

Person.class_eval do 
  def name
    'Andrew'
  end
end

person1 = Person.new
person2 = Person.new

p person1.name
p person2.name

# ---- 输出结果 ----
"Andrew"
"Andrew"

```

**解释**：上面示例中，我们为`Person`这个类添加了`name`的实例方法，由输出可知，我们成功的在`Person`的每个实例下都添加了`name`方法。

我们也可以从定义的方法中获取到对应类的实例变量。

```ruby
class Person
  def initialize name
    @name = name
  end
end

Person.class_eval do 
  def name
    @name
  end
end

person1 = Person.new 'Tom'
person2 = Person.new 'Bob'

p person1.name
p person2.name

# ---- 输出结果 ----
"Tom"
"Bob"

```


**Tips**：class_eval 和 instance_eval 后面不仅可以使用 block，还可以直接添加字符串，类似于eval，但是同样不建议使用，同样会导致不安全的问题。

```ruby
class Person
end

Person.class_eval <<-DOC
  def name
   'Andrew'
  end
DOC

p Person.new.name

# ---- 输出结果 ----
"Andrew"

```

