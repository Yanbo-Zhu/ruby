
Ruby I/O是与系统交互的一种方式。 数据以字节/字符的形式发送。 `IO`类是Ruby中所有输入和输出的基础。它可以是双工的，因此可使用多个本机操作系统流。

`IO`类有一个叫作`File`类的子类，允许在Ruby中读取和写入文件。 这两个类是密切相关的。 `IO`对象表示对键盘和屏幕的可读/可写交互。

# 1 I/O 对象

在 Ruby 中，I/O 对象包装输入/输出流。常量`STDIN`，`STDOUT`和`STDERR`指向包装标准流的I/O对象。默认情况下，全局变量`$stdin`，`$stdout`和`$stderr`指向它们各自的常量。尽管常量应始终指向默认流，但是可以覆盖全局变量以指向另一个 I/O 流（例如文件）。


# 2 输入和输出


输出 puts, print p 
需要注意一点，puts 和 print 的区别是 puts 在结尾有一个换行符，而 print 没有。
p 相当于将puts打印的内容进行了to_s操作。

```ruby 
puts [1, 2, 3].to_s
p [1, 2, 3]

#---- 输出结果 ----
[1, 2, 3]
[1, 2, 3]

```


输入  gets
我们使用 $stdin.gets 来让用户进行输入操作。
当然我们也可以省略 $stdin，只使用 gets。

```
a = $stdin.gets 
p a
             # 键盘输入1

#---- 输出结果 ----
1
"1\n"

```

# 3 I/O端口中的常用模式

- “**r**”：只读模式，是从文件开始的默认模式。
- “**r+**”：读写模式，从文件开头开始。
- “**w**”：只写模式，创建新文件或截断现有文件进行写入。
- “**w+**”：读写模式，创建一个新文件或截断一个现有文件进行读写。
- “**a**”：只写模式，如果文件存在，它会将文件附加到一个新的文件将被创建为仅写入。
- “**a+**”：读写模式，如果文件存在，它将附加文件，一个新文件将被创建用于写入和读取。

# 4 IO控制台

**IO**控制台提供了与控制台交互的不同方法。`IO`类提供以下基本方法：

- `IO::console`
- `IO#raw#raw!`
- `IO#cooked`
- `IO#cooked!`
- `IO#getch`
- `IO#echo=`
- `IO#echo?`
- `IO#noecho`
- `IO#winsize`
- `IO#winsize=`
- `IO#iflush`
- `IO#ioflush`
- `IO#oflush`

# 5 Ruby打开文件

可以使用不同的读取，写入或读写方法创建Ruby文件。

在Ruby中打开文件有两种方法：

- **File.new**方法 - 使用这个方法，可以创建一个新的文件用于读取，写入或读写。
- **File.open**方法 - 使用这个方法创建一个新的文件对象。该文件对象被分配给一个文件。

两种方法之间的区别是：`File.open`方法可以与一个块相关联，而`File.new`方法不能。

**语法**

```ruby
f = File.new("fileName.rb")
```

或者 -

```ruby
File.open("fileName.rb", "mode") do |f|
```

**创建文件的示例**

使用`File.open`方法在Ruby中创建一个文件，以便从文件读取或写入数据。

**步骤1)** 在文件`file-create.rb`中，编写代码以创建新文件，如下所示。

```ruby
#!/usr/bin/ruby   
# file : file-create.rb
puts 'Start to create file ...'
File.open('create-first-file.txt', 'w') do |f|   
    f.puts "This is Yiibai Website"   
    f.write "You are reading Ruby tutorial.\n"   
    f << "Please visit our website.\n"   
end   
puts 'End create file.'
```

**步骤2)**在控制台中键入以下命令以创建的文件。

```ruby
F:\worksp\ruby>ruby file-create.rb
Start to create file ...
End create file.

F:\worksp\ruby>
```

创建新文件：_create-first-file.txt_可在代码目录下找到。

# 6 Ruby读取文件

读取文件有三种不同的方法。要返回一行，使用以下语法。

**语法：**

```ruby
f.gets  
 code...
```

要在当前位置之后返回整个文件，使用以下语法。

**语法**

```ruby
f.read  
 code...
```

要以文件行的形式返回文件，使用以下语法。

**语法**

```ruby
f.readlines  
 [code...]
```


**读取文件的示例**

使用`File.open`方法在Ruby中创建一个文件，以便从文件读取或写入数据。

**步骤1)**在文件 _file-read.rb_ 中，编写代码以读取已存在的文件，如下所示。

```ruby
#!/usr/bin/ruby   
# file ： file-read.rb
while line = gets   
    puts line   
end
```

**步骤2)**在控制台中输入以下命令以读取文件。

```shell
F:\worksp\ruby>ruby file-read.rb create-first-file.txt
This is Yiibai Website
You are reading Ruby tutorial.
Please visit our website.

F:\worksp\ruby>
```

关于文件：_create-first-file.txt_ 的内容显示在控制台中。

**sysread 方法**

`sysread`方法也用于读取文件的内容。使用此方法可以以任何模式打开文件。

示例：

在文件：_file-sysread.rb_中，编写代码以读取已存在的文件，如下所示。

```ruby
#!/usr/bin/ruby   

aFile = File.new("create-first-file.txt", "r")   
if aFile   
   content = aFile.sysread(30)   
   puts content   
else   
   puts "Unable to open file!"   
end
```

执行上面代码，输出结果如下 -

```shell
F:\worksp\ruby>ruby file-sysread.rb
This is Yiibai Website
You ar

F:\worksp\ruby>
```

参数`30`表示将从文件打印`30`个字符。

# 7 Ruby写入文件

借助于`syswrite`方法，可以将内容写入文件。 需要在此方法的写入模式下打开文件。

新内容将覆盖已经存在的文件中的旧内容。

**示例**

```ruby
#!/usr/bin/ruby   
# file : file-syswrite.rb

aFile = File.new("about.txt", "r+")   
if aFile   
aFile.syswrite("New content is written in this file.\n Yes, do...while write \n我乱写一片.")   
end  
puts 'write to file: about.txt success. '
```

执行上面代码，得到以下输出结果 -

```ruby
F:\worksp\ruby>ruby file-syswrite.rb
write to file: about.txt success.

F:\worksp\ruby>
```

# 8 Ruby重命名和删除文件

使用`rename`方法重命名Ruby文件，并使用`delete`方法进行删除。

要重命名文件，使用以下语法。


```ruby
File.rename("olderName", "newName.txt")  
```


```ruby
#!/usr/bin/ruby   
# file ： file-rename.rb

File.rename("about.txt", "about.new.txt")
puts 'rename file => about.txt to about.new.txt'
```

执行上面代码，得到以下输出结果 -

```ruby
F:\worksp\ruby>ruby file-rename.rb
rename file => about.txt to about.new.txt

F:\worksp\ruby>
```

在上面的输出中，_about.txt_文件不再存在，因为它的名称已被更改为_about.new.txt_文件。

要删除文件，使用以下语法。

```ruby
File.delete("filename.txt")
```

**示例**

```ruby
#!/usr/bin/ruby   

File.delete("new.txt")
```

在上述输出中，`new.txt`文件不再存在，因为它已被删除。


