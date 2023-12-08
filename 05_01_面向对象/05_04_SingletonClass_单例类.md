
Ruby 对象模型有一个很特殊的存在，他就是**单例类**（_Singleton Class_），本章节中我们会对单例类做一个简单的介绍。

本章节我们学习了 Ruby 中单例类的知识，学习到了：
1. 定义对象的方法的时候（包括类），所有的方法都是定义到了单例类中。
2. 每一个对象都有单例类（包括类）。
3. 实例对象的单例类的超类是实例对象的类。
4. 类的单例类的超类等于类的超类的单例类。
5. 单例类同样也是`Class`类的对象。
6. 我们可以通过`class <<`来打开单例类并定义方法。

# 1 什么是单例类

让我们从一个简单的例子开始：

**实例**

```ruby
class Person
  def a_name
    'Andrew'
  end
end

person = Person.new
puts person.a_name
puts person.class.instance_methods.grep /name/

# ---- 输出结果 ----
Andrew
a_name

```

这个是一个简单的例子，在类的本质中我们学习到了，如果要查找一个对象中的方法，要从他的类中查找，因为方法都是定义在类中的。

**类其实也是对象**，那么当我们定义类方法的时候，同理，方法应该存在于`Class`类中的，但是答案却是否定的。

**实例：**
```ruby
class Person
  def self.list
    [ 'Andrew', 'Tom', 'Alice' ]
  end
 
end

p Person.list
p Person.class.instance_methods.grep /list/

# ---- 输出结果 ----
["Andrew", "Tom", "Alice"]
[]

```


那么这个类方法到底放在了哪里呢，答案就是存在于单例类中。

我们可以使用`singleton_class`来获取一个对象的单例类。

**实例：**
```ruby
class Person
  def self.list
    [ 'Andrew', 'Tom', 'Alice' ]
  end
 
end

p Person.list
p Person.singleton_class.instance_methods.grep /list/

# ---- 输出结果 ----
["Andrew", "Tom", "Alice"]
[:list]

```

**解释：**当我们定义`self.list`的时候，实际上是告诉Ruby打开`self`的单例类，并在其定义`list`方法。

同理可知，所有的对象他们的方法实际上都存在于单例类之中。

**实例：**
```ruby
class Person
  def a_name
    'Andrew'
  end
end

person = Person.new
puts person.a_name
puts person.singleton_class.instance_methods.grep /name/

# ---- 输出结果 ----
Andrew
a_name

```
# 2 单例类与类、对象之间的关系

现在让我们定义一个`Student`类和`Person`类。

**实例：**

```ruby
class Person
  def a_name
    'Andrew'
  end 
end

class Student < Person
end

student = Student.new
p student.a_name

# ---- 输出结果 ----
'Andrew'

```

让我们输出它的祖先链：

```ruby
p Student.ancestor

# ---- 输出结果 ----
[Student, Person, Object, Kernel, BasicObject]

```

让我们从头输出一下它祖先链类的部分：

```ruby
p student.class
p student.class.superclass
p student.class.superclass.superclass
p student.class.superclass.superclass.superclass

# ---- 输出结果 ----
Student
Person
Object
BasicObject

```

现在让我们在中间输出它的每一个对象的单例类：

```ruby
p student.singleton_class
p student.class
p student.class.singleton_class
p student.class.superclass
p student.class.superclass.singleton_class
p student.class.superclass.superclass
p student.class.superclass.superclass.singleton_class
p student.class.superclass.superclass.superclass
p student.class.superclass.superclass.superclass.singleton_class

# ---- 输出结果 ----
#<Class:#<Student:0x007f82f4170060>>
Student
#<Class:Student>
Person
#<Class:Person>
Object
#<Class:Object>
BasicObject
#<Class:BasicObject>

```

> **Tips**：单例类输出前面会有一个`#`，比如：通常我们说`Person`的单例类是`#Person`。

实例对象的单例类的超类是实例对象的类。

```ruby 
p student.class
p student.singleton_class.superclass

# ---- 输出结果 ----
Student
Student

```

类的单例类的超类等于类的超类的单例类。

```ruby
p student.class.singleton_class.superclass
p student.class.singleton_class.superclass.superclass
p student.class.singleton_class.superclass.superclass.superclass

p student.class.superclass.singleton_class
p student.class.superclass.superclass.singleton_class
p student.class.superclass.superclass.superclass.singleton_class

# ---- 输出结果 ----
#<Class:Person>
#<Class:Object>
#<Class:BasicObject>
#<Class:Person>
#<Class:Object>
#<Class:BasicObject>

```

单例类同样也是`Class`类的对象。

```ruby 
p student.singleton_class.class
p student.class.singleton_class.class

# ---- 输出结果 ----
Class
Class

```

# 3 打开单例类

我们可以通过`class <<`来打开单例类并且在其中定义方法。

**实例：**

普通实例对象：

```ruby
class Person
end

person = Person.new

class << person
  def name
    'Andrew'
  end
end

puts person.name

# ---- 输出结果 ----
Andrew

```

对于类您同样可以这么使用：
```ruby
class Person
end

class << Person
  def list
    ['Andrew', 'Alice']
  end
end

p Person.list

# ---- 输出结果 ----
["Andrew", "Alice"]

```


或者在类的内部打开单例类：
```ruby
class Person
  class << self
    def list
      ['Andrew', 'Alice']
    end
  end
end

p Person.list

# ---- 输出结果 ----
["Andrew", "Alice"]

```


