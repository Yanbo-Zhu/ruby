
Ruby `for`循环遍历特定的数字范围。 因此，如果程序具有固定次数的迭代，则使用`for`循环。  
Ruby `for`循环将在表达式中的每个元素执行一次。

语法：

```ruby
for variable [, variable ...] in expression [do]  
   code  
end
```

# 1 Ruby使用for循环遍历范围

代码示例：

```ruby
puts "输入一个数字："
a = gets.chomp.to_i   
for i in 1..a do   
  puts i   
end
```

将上面代码保存到文件： _for-loop-range.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby for-loop-range.rb
输入一个数字：
8
1
2
3
4
5
6
7
8

F:\worksp\ruby>
```

# 2 Ruby使用for循环遍历数组

代码示例：

```ruby
x = ["Blue", "Red", "Green", "Yellow", "White", '五颜六色']   
for i in x do   
  puts i   
end
```

将上面代码保存到文件：_for-loop-arrays.rb_中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby for-loop-arrays.rb
Blue
Red
Green
Yellow
White
五颜六色

F:\worksp\ruby>
```

//更多请阅读：https://www.yiibai.com/ruby/for-loop.html

