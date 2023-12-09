
Ruby模块是方法和常量的集合。 模块方法可以是实例方法或模块方法。实例方法是包含模块的类中的方法。
可以在不创建封装对象的情况下调用模块方法，但是实例方法不能这么直接调用。
Ruby模块类似于类，因为它们包含方法，类定义，常量和其他模块的集合。Ruby模块可定义为类。但无法使用模块来创建对象或子类。也没有继承的模块层次结构。
module是Ruby中模块的关键字。module的使用仅与类有一点相似。可以使用模块名访问其自身的属性与静态方法。但不可以通过模块名访问一般方法。

模块基本上主要有两个目的：
- 它们作为命名空间，防止对象名字冲突。
- 它们允许`mixin`工具在类之间共享功能。


调用模块方法:模块名.模块方法名
引用模块常量:模块名::模块常量
# 1 模块的定义

**语法**
```shell
module ModuleName  
   statement1  
   statement2  
   ...........  
end
```
模块名称应以大写字母开头。

```ruby
module BaseFunc
    Version = "0.0.1"

    def v
        return Version
    end

    def add(a, b)
        return a + b
    end

    def self.showVersion #静态方法
        return Version
    end

    #将v方法定义范围静态方法
    module_function :v
end
```

# 2 添加方法

与类中定义实例方法一样，在模块中创建方法只需在模块中定义一个方法即可：

```ruby
require 'digest'

module Encryption
  def encrypt(string)
    Digest::SHA2.hexdigest(string)
  end
end

```

现在模块拥有了一个名为encrypt的方法。这个方法是用于将传入字符串进行 SHA256 加密。
注意事项：因为和类不同，模块没有new方法，不能实例化成为一个对象，所以不能模块一般通过被类进行引用来执行相应的方法。

```ruby
Encryption.new

# ---- 输出结果 ----
undefined method `new' for Encryption:Module (NoMethodError)
```

不过，模块可以通过模块名.方法名()的形式调用模块方法。
```ruby
require 'digest'

module Encryption
  def self.encrypt(string) # 注意这里多了一个self
    Digest::SHA2.hexdigest(string)
  end
end

puts Encryption.encrypt('super password')

# ---- 输出结果 ----
'02f10a4b97a846ae06d64073bb56469d8516bbe19bd1487e9d80ae7e9ec0ac1b'

```


**通过模块名访问**：
```ruby
puts BaseFunc::Version
#self两种访问方法
puts BaseFunc.showVersion
puts BaseFunc::showVersion
puts BaseFunc.v
#puts BaseFunc.add(10, 30) #会报错，使用模块名无法访问一般方法。
```

# 3 引用模块


**解释：** 在这里我们使用了`include`关键字来引用模块（引入模块一共有三种方式：_include_、_extend_、_prepend_，在之后的章节中我们会对这三种情况逐个分析）
https://www.jianshu.com/p/7ee3cf85b06c




## 3.1 区别


先创建一个`Person`类，并且定义一个`FooModule`模块

```ruby
class Person
  def introduce
    puts "hello, i am from Person"
  end
end

person = Person.new
person.introduce
#=> hello, i am from Person

module FooModule
  def introduce
    puts "hello, i am from Person FooModule"
  end

  def current_time
    puts "current time is #{Time.now.to_s}"
  end
end
```

extend
现在我们在`Person`类中使用`extend`关键字来调用模块，使用相关方法，并且打印出相关信息。
我们由`Person.introduce`和`Person.current_time`可以观察出来`extend`关键字的作用是为`Person`添加了2个类方法。
```ruby
class Person
  extend FooModule
end

Person.introduce
# => hello, i am from FooModule
person.introduce
# => hello, i am from Person
Person.current_time
# => current time is 2019-11-01 10:53:04 +0800
person.current_time
# =>  undefined method `current_time' for #<Person:0x000000028395d0> (NoMethodError)
```


include
上面两个类调用的方法都报错了可以看出，`include`和`extend`相反，`include`是为类中添加的实例方法，而不是类方法。
```ruby
class Person
  include FooModule
end

Person.introduce
# =>  undefined method `introduce' for Person:Class (NoMethodError)
person.introduce
# => hello, i am from Person
Person.current_time
# =>  undefined method `current_time' for Person:Class (NoMethodError)
person.current_time
# => current time is 2019-11-01 10:59:28 +0800
```

prepend
`prepend`和`include`同样都是为类中添加实例方法，但不同之处在于，引入模块后的方法链会有一点区别

我们先看看 `include`的方法链
```ruby
class Person
  # extend FooModule
  include FooModule
  # prepend FooModule
end

puts Person.ancestors

# =>Person
# =>FooModule
# =>Object
# =>JSON::Ext::Generator::GeneratorMethods::Object
# =>PP::ObjectMixin
# =>Kernel
# =>BasicObject
```

再看看`prepend`的
```ruby

class Person
  # extend FooModule
  # include FooModule
  prepend FooModule
end

puts Person.ancestors

# =>FooModule
# =>Person
# =>Object
# =>JSON::Ext::Generator::GeneratorMethods::Object
# =>PP::ObjectMixin
# =>Kernel
# =>BasicObject
```


可以看出`FooModule`和`Person`的顺序有什么不同，那么具体不同体现在哪，接下来我们调用方法就可以看出区别了。
可以看到，如果使用`prepend`的话我们会优先去查找`FooModule`中的`introduce`方法（如果`FooModule`没有`introduce`方法话，会依次去祖先链的上一级去查找`introduce`方法），而`include`是优先在`Person`中去查找。

include 
```ruby
class Person
  # extend FooModule
  include FooModule
  # prepend FooModule
end
person.introduce
# => hello, i am from Person
```


prepend
```ruby
class Person
  # extend FooModule
  # include FooModule
  prepend FooModule
end
person.introduce
# => hello, i am from FooModule
```

## 3.2 Ruby 的 include 和 included (添加实例方法)

Ruby 提供了混入（Mixin）的方式使用模块（Module）,它可以让被混入的模块中的代码被其他类或者模块使用。
本章节我们学习了`include`，当类`include`一个模块的时候，会将模块中定义的方法填充进类的实例方法中，另外，当一个模块被`include`的时候，会触发`included`回调，并返回操作的类名

### 3.2.1 include

`include`将会把模块里的方法包含进被执行的类的**实例方法**中。

**解释**：当类`Member`包含了模块`Person`之后，类`Member`增加了一个名为`name`的实例方法（类实例后可调用的方法）。

```ruby
module Person
  def name
    "My name is Andrew"
  end
end

class Member
  include Person
end

puts Member.new.name

# ---- 输出结果 ----
My name is Andrew

```




引用的方法都会变成`Person`类的实例方法。重构后，我们调用`encrypted_password`时加密时使用的`encrypt`方法来自`Encryption`模块内，这样避免了在很多类中做同一种加密，每修改一次加密形式就要修改每一个类代码的问题。

上述这种情况假设我们还有其它需要加密内容的类，我们还希望将加密的方法保留在一个地方，这么做有 4 个好处：
- 当我们想切换到另一种加密方式的时候，只需要更改这个模块的加密代码即可；

- 我们不希望相同的加密逻辑代码在某些需要的位置重复被使用；
- 可以把这种代码视为一个杂物，隐藏在另一个文件中，我们只需要关心类的工作，不需要关心加密事务的具体逻辑；
- 使用模块来封装代码也会使可读性更高。


```ruby
class BaseClass include BaseFunc
end

puts BaseClass::Version #可以访问
#self等私有方法属性是该模块自身的不会被include。
# puts BaseClass.showVersion
# puts BaseClass.v
myCls = BaseClass.new
#模块中的一般方法可以被include该模块的类的object继承。
puts myCls.add(10,20)
```

让我们用Person类举例：
```ruby
require 'digest'

class Person
  
  def initialize(name)
    @name = name
  end

  def name
    @name
  end
  
  def password=password
    @password = password
  end

  def encrypted_password
    Digest::SHA2.hexdigest(@password)
  end
end

person = Person.new("Andrew")
person.password = "super password"
p person.encrypted_password


# ---- 输出结果 ----
'02f10a4b97a846ae06d64073bb56469d8516bbe19bd1487e9d80ae7e9ec0ac1b'




```

Person类拥有一个对密码加密的方法，让我们对这个方法进行重构。
我们要重构这个获取对密码进行加密之后的结果的方法encrypted_password。在这时我们选择引用Encryption模块来添加对字符串加密的encrypt方法。


```ruby
require 'digest'

module Encryption
  def encrypt(string)
    Digest::SHA2.hexdigest(string)
  end
end

class Person
  include Encryption
  
  def initialize(name)
    @name = name
  end

  def name
    @name
  end
  
  def password=password
    @password = password
  end

  def encrypted_password
    encrypt(@password)
  end
end

person = Person.new("Andrew")
person.password = "super password"
p person.encrypted_password


# ---- 输出结果 ----
'02f10a4b97a846ae06d64073bb56469d8516bbe19bd1487e9d80ae7e9ec0ac1b'

```


### 3.2.2 included

当我们include了一个模块后，模块被包含后会调用一个钩子方法，这个方法名为included。

让我们为模块Person增加一个钩子方法。
**解释**：当模块`Person`被包含后，触发了`included`钩子方法，打印了“Member included Person”。

```ruby
module Person
  def self.included(klass)
    puts "#{klass} included #{self}"
  end
  
  def name
    "My name is Andrew"
  end
end

class Member
  include Person
end

puts Member.new.name

# ---- 输出结果 ----
Member included Person
My name is Andrew

```


## 3.3 extend 和 extended (扩展的是类方法)

本章节我们学习了`extend`，当类或模块`extend`一个模块的时候，会将模块中定义的方法填充进类或模块的类方法中，另外，当一个模块被`extend`的时候，会触发`extended`回调，并返回操作的类名。

### 3.3.1 extend

`extend`可以扩展类或模块里面的方法，与`include`不同，`extend`扩展的是类方法（_Class Method_）。

**解释**：如上面输出结果展示的那样，扩展后，模块`Person`为类`Member`增加了一个名叫`name`的类方法（类方法是以类名直接调用的方法）。
除此之外对象也可以对模块进行`extend`，模块中的方法变成了实例方法（_Instance Method_），不过扩展的方法只针对一个对象有效。

```ruby
module Person
  def name
    "My name is Andrew"
  end
end

class Member
  extend Person
end

puts Member.name

# ---- 输出结果 ----
My name is Andrew

```

### 3.3.2 extended

当我们`extend`一个模块的时候，就会触发模块的`extended`回调函数。

让我们修改一下上面的实例。
**解释**：当模块`Person`被扩展后，会触发`extended`钩子方法，率先打印了“Member extended Person”。

```ruby
module Person
  def self.extended(klass)
    puts "#{klass} extended #{self}"
  end
  
  def name
    "My name is Andrew"
  end
end

class Member
  extend Person
end

puts Member.name

# ---- 输出结果 ----
Member extended Person
My name is Andrew

```



## 3.4 prepend 和 prepened (添加的类的实例方法)

本章节我们学习了`prepend`，它和`include`很像，都可以向类追加实例方法，不同的是，`prepend`追加的方法的名称和类本身拥有的相同时，`prepend`会覆盖原有的方法，`include`则不会覆盖。另外，当一个模块被`prepend`的时候，会触发`prepended`回调，并返回操作的类名。


### 3.4.1 prepend

prepend是在 Ruby2.0 以后引入进来的，它的使用方式和include非常类似，将模块中定义的方法添加进被添加的类的实例方法中。

```ruby
module Person
  def name
    "My name is Andrew"
  end
end

class Member
  prepend Person
end

puts Member.new.name

# ---- 输出结果 ----
My name is Andrew

```


### 3.4.2 prepend 和 include 的区别

使用`include`或者`extend`引入的方法如果和被引入的模块/类中的**方法重名**，将不会覆盖被引入模块的方法，但是`prepend`**会覆盖被引入模块的方法**。

而 prepend 则是这个样子。
```ruby
module Person
  def name
    "My name is Person"
  end
end

class Member
  include Person
  
  def name
    "My name is Andrew"
  end
end

puts Member.new.name

# ---- 输出结果 ----
My name is Andrew

```

我们会发现，原本的 name 实例方法被覆盖了。
```ruby
module Person
  def name
    "My name is Person"
  end
end

class Member
  prepend Person
  
  def name
    "My name is Andrew"
  end
end

puts Member.new.name

# ---- 输出结果 ----
My name is Person

```

### 3.4.3 prepended
当类追加模块后，会触发prepended钩子方法。
**解释**：当模块`Person`被追加后，触发了`prepended`钩子方法，打印了“Person prepended to Member”。
```ruby
module Person
  def self.prepended(klass)
    puts "#{self} prepended to #{klass}"
  end

  def name
    "My name is Andrew"
  end
end

class Member
  prepend Person
end

puts Member.new.name

# ---- 输出结果 ----
Person prepended to Member
My name is Andrew

```
# 4 模块命名空间

在编写较大的文件时，需要生成大量可重用的代码。 这些代码被组织成类，可以插入到一个文件中。

例如，如果两个人在不同的文件中具有相同的方法名称。 并且这两个文件都需要包含在第三个文件中。 那么它可能会产生问题，因为这两个包含的文件中的方法名称是相同的。

这里，模块机制发挥作用。 模块定义一个命名空间，您可以在命名空间中定义方法和常量，而不用管其他方法和常量执行。

**示例：**

假设在`modules-file1.rb`中，定义了不同类型的图书馆书籍，如小说，恐怖等。
在`modules-file2.rb`中，定义了阅读的小说数量，包括小说小说。
在`modules-file3.rb`中，需要加载文件`modules-file1.rb`和`modules-file2.rb`。这里我们将使用模块机制。

创建文件：_modules-file1.rb_ ，其代码如下所示 -
```ruby
#!/usr/bin/ruby   

# Module defined in file1.rb file   

module Library   
   num_of_books = 300   
   def Library.fiction(120)   
   # ..   
   end   
   def Library.horror(180)   
   # ..   
   end   
end
```

创建文件：_modules-file2.rb_ ，其代码如下所示 -
```ruby
#!/usr/bin/ruby   

# Module defined in file2.rb file   

module Novel   
   total = 123   
   read = 25   
   def Novel.fiction(left)   
   # ...   
   end   
end
```

创建文件：_modules-file3.rb_ ，其代码如下所示 -
```ruby
require "Library"   
require "Novel"   

x = Library.fiction(Library::num_of_books)   
y = Novel.fiction(Novel::total)
```

通过在模块名称后带点(`.`)符号来调用模块方法，并使用模块名称和两个冒号引用常量。

# 5 模块混合

Ruby不支持多重继承。 模块消除了在Ruby中使用mixin的多重继承的需要。模块没有实例，因为它不是一个类。 但是，一个模块可以包含在一个类中。

当您在类中包含模块时，该类将可以访问模块的方法。

**示例：**

```ruby
module Name   
   def bella   
   end   
   def ana   
   end   
end   
module Job   
   def editor   
   end   
   def writer   
   end   
end   

class Combo   
include Name   
include Job   
   def f   
   end   
end   

final=Combo.new   
final.bella   
final.ana   
final.editor   
final.writer   
final.f
```

这里，模块`Name`由方法`bella`和`ana`组成。 模块`Job`由方法`editor`和`writer`组成。`Combo`类包括两个模块，由于`Combo`可以访问所有四种方法。 因此，`Combo`类作为`mixin`混合类型使用。

# 6 模块 和 类的关系 

Class类的超类是Module（模块）
每个类都是一个模块

类就是带有三个方法(new,allocate,superclass)的增强模块

代码被包含到别的代码中----->使用模块
某段代码被实例化或被继承----->使用类

在 Ruby 中，模块在某种程度上类似于类：它们可以持有方法，就像类一样。但是，和类不同的是无法实例化模块，即模块不可以创建对象。因此，与类不同，模块没有new方法。
那么哪里需要使用模块呢？
使用模块，您可以在类之间共享方法：模块可以包含在类中，这使得它们的方法可以在多个类中使用，就像我们将这些方法复制并粘贴到类定义上一样。这种使用方式我们也称为Mixin。




类可通过引用访问
```
class Foo
end
foo = Foo.new
myFoo = Foo
foo2= myFoo.new
```
myFoo是变量，Foo是常量 类是对象，类名是常量

## 6.1 常量
任何以大写字母开头的引用都是常量（包括类名和模块名）

常量的值可以修改，常量和变量的作用域不同

```
MyConstant = "root level"
module MyModule
	MyConstant = "outer"
	class MyFoo
		MyConstant = "inner"
	end
end
#这里的常量像文件系统一样组织成树形结构
```

## 6.2 常量的路径
通过路径标识，用双冒号进行分隔

```
puts MyModule::MyConstant
puts MyModule::MyFoo::MyConstant
puts ::MyConstant
```

绝对路径--->双冒号开头

Module类有一个实例方法和类方法，方法名都是constants

```
puts Module.constants#返回当前程序中所有顶层的常量 包括类名
puts MyModule.constants#返回当前范围内的所有常量
```

获取当前代码所在的路径，使用Module.nesting方法。
防止类名冲突，使用模块充当常量容器的模块----->命名空间
