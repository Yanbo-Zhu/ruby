# 1 基本

套接字是网络通信通道的端点，客户端和服务器之间相互通信。它们可以在同一台机器上或在不同的机器上进行通信。

套接字(Socket)的类型有以下几种：

- TCP套接字
- UDP套接字
- UNIX套接字

有两个级别的套接字 - 高级和低级。 低级访问允许您处理系统支持的套接字。 它允许实现无连接和面向连接的协议。 高级访问允许您处理像HTTP和FTP这样的网络协议。

**示例-1**

创建一个服务器端 - **server1.rb**，代码如下 -

```ruby
#!/usr/bin/ruby   
# file : server1.rb

require 'socket'   

server = TCPServer.open(8088)   
loop {   
    client = server.accept   
    client.puts "Hello. This is socket programming"   
    client.close   
}
```

在上述代码中，需要包括预先安装的`socket`模块。 在系统上使用`8088`端口。当然您可根据需要可以使用任何其它的端口。

启动循环，接受所有连接到`8088`端口，并通过套接字网络将数据发送给客户端。

最后，关闭套接字。

创建一个客户端代码 - _client1.rb_

```ruby
#!/usr/bin/ruby   
# file : client1.rb
require 'socket'   

hostname = 'localhost'   
port = 8088   

s = TCPSocket.open(hostname, port)   

while line = s.gets   
    puts line.chomp   
end   
s.close
```

在上述代码中，需要包括预先安装的`socket`模块。 创建一个套接字并将其连接到端口`8088`。

创建一个`while`循环来获取通过套接字发送的所有信息。

最后，关闭套接字。

**执行输出：**

打开终端，转到保存上述两个文件的目录，在本示例中，所有代码均保存在：`F:\worksp\ruby` 目录下。

现在打开两个终端。在第一个终端先执行服务器脚本(`ruby server1.rb`)，服务器脚本启动完成后，在第二个终端执行以下命令执行客户端脚本，得到以下结果 -

```ruby
F:\worksp\ruby>ruby client1.rb
Hello. This is socket programming

F:\worksp\ruby>
```

# 2 多个客户端套接字编程

对于多个客户端进行套接字编程，将需要一个循环和一些线程来接受和响应多个客户端连接和交互。

**示例-2**

创建一个服务器端 - **server2.rb**，代码如下 -

```ruby
#!/usr/bin/env ruby -w
# file : server2.rb

require "socket"   
class Server   
  def initialize( port, ip )   
    @server = TCPServer.open( ip, port )   
    @connections = Hash.new   
    @rooms = Hash.new   
    @clients = Hash.new   
    @connections[:server] = @server   
    @connections[:rooms] = @rooms   
    @connections[:clients] = @clients   
    run   
  end   

  def run   
    loop {   
      Thread.start(@server.accept) do | client |   
        nick_name = client.gets.chomp.to_sym   
        @connections[:clients].each do |other_name, other_client|   
          if nick_name == other_name || client == other_client   
            client.puts "This username already exist"   
            Thread.kill self   
          end   
        end   
        puts "#{nick_name} #{client}"   
        @connections[:clients][nick_name] = client   
        client.puts "Connection established..."   
        listen_user_messages( nick_name, client )   
      end   
    }.join   
  end   

  def listen_user_messages( username, client )   
    loop {   
      msg = client.gets.chomp   
      @connections[:clients].each do |other_name, other_client|   
        unless other_name == username   
          other_client.puts "#{username.to_s}: #{msg}"   
        end   
      end   
    }   
  end   
end   

Server.new( 8088, "localhost" )
```

在上面的代码中，服务器将与客户端在指定的端口(`8088`)来建立连接。 在这里，每个连接的用户需要一个线程来处理用户连接信息交互。

`run`方法验证输入的名称是否唯一。 如果用户名已经存在，连接将被断开，否则将建立连接。

`listen_user_messages`方法用于侦听用户消息并将其发送给所有用户。

创建一个客户端代码 - _client2.rb_

```ruby
#!/usr/bin/env ruby -w
# file : client2.rb

require "socket"   
class Client   
  def initialize( server )   
    @server = server   
    @request = nil   
    @response = nil   
    listen   
    send   
    @request.join   
    @response.join   
  end   

  def listen   
    @response = Thread.new do   
      loop {   
        msg = @server.gets.chomp   
        puts "#{msg}"   
      }   
    end   
  end   

  def send   
    puts "Enter your name:"   
    @request = Thread.new do   
      loop {   
        msg = $stdin.gets.chomp   
        @server.puts( msg )   
      }   
    end   
  end   
end   

server = TCPSocket.open( "localhost", 8088 )   
Client.new( server )
```

在上面的代码中，创建了`Client`类用来处理用户。

在 `send` 和 `listen` 方法中创建两个线程，以便可以同时读取/写入消息。

**执行输出：**

下面显示两个客户端之间的聊天消息，首先执行(ruby server2.rb)，然后再运行两个客户端，在两个客户端之间发送信息聊天。

![](http://www.yiibai.com/uploads/images/201705/1005/441110521_32318.png)

服务器(server2.rb)记录每个客户端连接信息 -

```shell
F:\worksp\ruby>ruby server2.rb
maxsu #<TCPSocket:0x00000002c3dc30>
minsu #<TCPSocket:0x00000002c3d730>
......
```

//更多请阅读：https://www.yiibai.com/ruby/socket-programming.html




