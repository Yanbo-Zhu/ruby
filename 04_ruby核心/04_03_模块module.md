
Ruby模块是方法和常量的集合。 模块方法可以是实例方法或模块方法。实例方法是包含模块的类中的方法。
可以在不创建封装对象的情况下调用模块方法，但是实例方法不能这么直接调用。
Ruby模块类似于类，因为它们包含方法，类定义，常量和其他模块的集合。Ruby模块可定义为类。但无法使用模块来创建对象或子类。也没有继承的模块层次结构。
module是Ruby中模块的关键字。module的使用仅与类有一点相似。可以使用模块名访问其自身的属性与静态方法。但不可以通过模块名访问一般方法。

模块基本上主要有两个目的：
- 它们作为命名空间，防止对象名字冲突。
- 它们允许`mixin`工具在类之间共享功能。

**语法**

```shell
module ModuleName  
   statement1  
   statement2  
   ...........  
end
```

模块名称应以大写字母开头。


**模块的定义**：

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

**通过模块名访问**：

```ruby
puts BaseFunc::Version
#self两种访问方法
puts BaseFunc.showVersion
puts BaseFunc::showVersion
puts BaseFunc.v
#puts BaseFunc.add(10, 30) #会报错，使用模块名无法访问一般方法。
```

**类include模块**：

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

# 1 模块命名空间

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

# 2 模块混合

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



