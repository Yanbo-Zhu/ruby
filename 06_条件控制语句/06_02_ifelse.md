
Ruby `if else`语句用于测试条件。 Ruby中有各种各样的`if`语句。

- `if`语句
- `if-else`语句
- `if-else-if`(`elsif`)语句
- 三元(缩写if语句)语句

## 0.1 Ruby if语句

Ruby `if`语句测试条件。 如果`condition`为`true`，则执行`if`语句。

语法：

```ruby
if (condition)  
//code to be executed  
end
```

流程示意图如下所示 -

![](http://www.yiibai.com/uploads/images/201705/0705/208090522_43717.png)

代码示例：

```ruby
a = gets.chomp.to_i   
if a >= 18   
  puts "You are eligible to vote."   
end
```

将上面代码保存到文件：_if-statement.rb_中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby if-statement.rb
90
You are eligible to vote.

F:\worksp\ruby>
```

# 1 Ruby if else语句

Ruby `if else`语句测试条件。 如果`condition`为`true`，则执行`if`语句，否则执行`block`语句。

**语法：**

```ruby
if(condition)  
    //code if condition is true  
else  
//code if condition is false  
end
```

流程示意图如下所示 -

![](http://www.yiibai.com/uploads/images/201705/0705/208090529_93418.png)

代码示例：

```ruby
a = gets.chomp.to_i   
if a >= 18   
  puts "You are eligible to vote."   
else   
  puts "You are not eligible to vote."   
end
```

将上面代码保存到文件：_if-else-statement.rb_中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby if-else-statement.rb
80
You are eligible to vote.

F:\worksp\ruby>ruby if-else-statement.rb
15
You are not eligible to vote.

F:\worksp\ruby>
```

# 2 Ruby if else if(elsif)

Ruby `if else if` 语句测试条件。 如果`condition`为`true`则执行`if`语句，否则执行`block`语句。

语法：

```ruby
if(condition1)  
//code to be executed if condition1is true  
elsif (condition2)  
//code to be executed if condition2 is true  
else (condition3)  
//code to be executed if condition3 is true  
end
```

流程示意图如下所示 -

![](http://www.yiibai.com/uploads/images/201705/0705/764090547_73150.png)

示例代码 -

```ruby
a = gets.chomp.to_i   
if a <50   
  puts "Student is fail"   
elsif a >= 50 && a <= 60   
  puts "Student gets D grade"   
elsif a >= 70 && a <= 80   
  puts "Student gets B grade"   
elsif a >= 80 && a <= 90   
  puts "Student gets A grade"    
elsif a >= 90 && a <= 100   
  puts "Student gets A+ grade"    
end
```

将上面代码保存到文件：_if-else-if-statement.rb_中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby if-else-if-statement.rb
25
Student is fail

F:\worksp\ruby>ruby if-else-if-statement.rb
80
Student gets B grade

F:\worksp\ruby>
```

# 3 Ruby三元语句

在Ruby三元语句中，`if`语句缩短。首先，它计算一个表达式的`true`或`false`值，然后执行一个语句。

语法：

```ruby
test-expression ? if-true-expression : if-false-expression
```

示例代码

```ruby
var = gets.chomp.to_i;   
a = (var > 3 ? true : false);    
puts a
```

将上面代码保存到文件：_ternary-statement.rb_中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby ternary-operator.rb
Ternary operator
5
2

F:\worksp\ruby>
```



