

Ruby `until`循环运行直到给定的条件求值为`true`。 当条件成立时，它退出循环。 它正好与while循环相反，`while`循环运行直到给定的条件评估求值为`false`。

`until`循环允许您编写更可读和逻辑的代码。

语法：

```ruby
until conditional  
   code  
end
```

代码示例：

```ruby
#!/usr/bin/ruby   

i = 1   
until i == 10   
    print i*10, "\n"   
    i += 1   
end
```

将上面代码保存到文件： _until-loop.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby until-loop.rb
10
20
30
40
50
60
70
80
90

F:\worksp\ruby>
```

//更多请阅读：https://www.yiibai.com/ruby/until-loop.html

