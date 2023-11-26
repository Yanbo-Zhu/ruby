
# 1 break 

Ruby `break`语句用于终止循环。 它主要用于在while循环中，在条件为真时执行语句，`break`语句一般用于终止循环。

`break`语句一般是在循环中调用的。

语法：

```ruby
break
```

代码示例：

```ruby
#!/usr/bin/ruby   

i = 1   
while true   
    if i*5 >= 25   
        break   
    end   
    puts i*5   
    i += 1   
end
```

将上面代码保存到文件： _break-statment.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby break-statment.rb
5
10
15
20

F:\worksp\ruby>
```

# 2 next语句

Ruby `next`语句用于跳过循环的下一个迭代。 执行下一条语句后，不再执行进一步的迭代。  
Ruby中的下一条语句等同于其他语言的`continue`语句。

语法：

```ruby
next
```

示例代码：

```ruby
#!/usr/bin/ruby   
for i in 1...12   
   if (i % 3 ==0) then   
      puts "Skip over ... "
      next   
   end   
   puts i   
end
```

将上面代码保存到文件： _next-statment.rb_ 中，执行上面代码，当`i`除以`3`等于`0`时，就会跳过后面的打印语句，得到以下结果 -

```shell
F:\worksp\ruby>ruby next-statment.rb
1
2
Skip over ...
4
5
Skip over ...
7
8
Skip over ...
10
11

F:\worksp\ruby>
```

