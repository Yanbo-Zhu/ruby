

# 1 redo 

Ruby `redo`语句用于重复循环的当前迭代。 `redo`语句在没有访问循环的条件的情况下进行。

`redo`语句在一个循环中使用。

语法：

```ruby
redo
```

代码示例：

```ruby
#!/usr/bin/ruby   

array=[1,2,3,4,5]
array.each do |i|
    puts i
    i+=1
    redo if i==3
end

#输出：1 2 3 3 4 5
```

将上面代码保存到文件： _redo-statment.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby redo-statment.rb
1
2
3
3
4
5

F:\worksp\ruby>
```

## 1.1 Ruby retry语句

Ruby `retry`语句用于从一开始重复整个循环迭代。

`retry`语句在循环中使用。

语法：

```ruby
retry
```

示例代码：

```ruby
#!/usr/bin/ruby   

n = 0
begin
  puts 'Trying to do something?'
  raise 'oops'
rescue => ex
  puts ex
  n += 1
  retry if n < 3
end
puts "Ok, I give up"
```

将上面代码保存到文件： _retry-statment.rb_ 中，执行上面代码，得到以下结果 -

```shell
F:\worksp\ruby>ruby retry-statment.rb
Trying to do something?
oops
Trying to do something?
oops
Trying to do something?
oops
Ok, I give up

F:\worksp\ruby>
```

//更多请阅读：https://www.yiibai.com/ruby/redo-and-retry-statement.html

