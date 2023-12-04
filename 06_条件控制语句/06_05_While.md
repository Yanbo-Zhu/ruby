
# 1 While 

Ruby `while`循环用于多次迭代程序。 如果程序的迭代次数不固定，则使用`while`循环。Ruby while循环在条件为真时执行条件。当条件变为`false`时，`while`循环停止循环执行。

语法：

```ruby
while conditional [do]  
   code  
end
```

while循环流程示意图如下 -

![](http://www.yiibai.com/uploads/images/201705/0705/964100519_27288.png)

代码示例：

```ruby
#!/usr/bin/ruby   

puts "Enter a value:" 
x = gets.chomp.to_i   
while x >= 0    
  puts x   
  x -=1   
end
```

将上面代码保存到文件： _while-loop.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby while-loop.rb
Enter a value:
8
8
7
6
5
4
3
2
1
0

F:\worksp\ruby>
```

# 2 do…while循环

Ruby `do...while`循环遍历程序的一部分几次。 它与while循环语法非常相似，唯一的区别是`do...while`循环将至少执行一次。 这是因为在while循环中，条件写在代码的末尾。

语法：

```ruby
loop do   
  #code to be executed  
  break if booleanExpression  
end
```

代码示例：

```ruby
#!/usr/bin/ruby   

loop do   
  puts "Checking for answer: "   
  answer = gets.chomp   
  if answer == '5'   
    puts "yes, I quit!"
    break   
  end   
end
```

将上面代码保存到文件： _do-while-loop.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby do-while-loop.rb
Checking for answer:
1
Checking for answer:
2
Checking for answer:
3
Checking for answer:
4
Checking for answer:
5
yes, I quit!

F:\worksp\ruby>
```

//更多请阅读：https://www.yiibai.com/ruby/while-and-do-while-loop.html




