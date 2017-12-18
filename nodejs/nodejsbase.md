## node
### REPL(交互式解释器)

```text
  表示一个电脑的环境， 输入node进入PEPL
  你可以使用下划线(_)获取表达式的运算结果.
  REPL命令：
    ctrl+c ==> 退出当前终端。
    ctrl+c 按下两次=>退出 Node REPL。
    ctrl+d ==> 退出 Node REPL.
    向上/向下 ==> 查看输入的历史命令
    tab 键 ==> 列出当前命令
    .help ==> 列出使用命令
    .break ==> 退出多行表达式
    .clear ==> 退出多行表达式
    .save filename - 保存当前的Node REPL会话到指定文件
    .load filename - 载入当前Node REPL会话的文件内容。
```
### 模块和包(Module,Package)

### http

```javascript
  var http = require('http');
  http.createServer(function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('hello world');
  }).listening(8080);
```
### fs

### 事件events

```text
  1.EventEmetter类：
      var EventEmitter = require('events');
      当新的监听器被添加时，所有的EventEmitter会触发'newListener'事件；
      当移除已存在的监听器时，则触发'removeListener'。
      defaultMaxListeners(n) ==> 修改所有实例的监听器数量
  2.EventEmetter类实例：
      emitter = new events.EventEmitter();
      也可 emitter = new events();
      方法：
        on(eventName, listener) ==>返回一个EventEmitter引用，可以链式调用.
           事件监听器会按照添加的顺序依次调用
        once(eventName, listener)
        prependListener(eventName, listener) ==> 添加listener函数到名为eventName
                                                 的事件的监听器数组的开头
                                                 返回一个EventEmitter引用,可以链式调用
        prependOnceListener(eventName, listener)
        removeAllListeners(eventName) ==> 移除全部或指定eventName的监听器
                                          返回一个 EventEmitter 引用，可以链式调用。
        removeListeners(eventName)
        setmaxlisteners(n/Infinity) ==> 改指定的 EventEmitter 实例的限制
        emit(eventName, args) ==>按监听器的同步地调用每个注册到名为eventName事件的监听器
                    如果事件有监听器，则返回 true ，否则返回 false
                    箭头函数作为监听器，这样 this 关键词就不再指向 EventEmitter 实例
        addlistener(...) ==> emitter.on(eventName, listener)的别名
        eventNames() ==> 返回一个列出触发器已注册监听器的事件的数组。
                         数组中的值为字符串或符号
        getMaxListeners() ==> 返回EventEmitter当前的最大监听器限制值
        listenerCount(eventName) ==> 返回正在监听名为eventName的事件的监听器的数量
        listeners(eventName) ==> 返回名为 eventName 的事件的监听器数组的副本
    3.事件：
      error
      newLitener
      removeListener

```

### [global](http://nodejs.cn/api/globals.html)

```text
  1.process ==> Nodejs进程状态的对象
    属性：
      execPath ==>node的二进制文件绝对路径
      argv ==>
        返回命令行参数数组：参数1为process.execPath，参数二为脚本文件路径，参数三以后为运行的参数
      argv0 ==> 保存Node.js启动时传入的argv[0]参数值的一份只读副本。
      execArgv ==> 数组 Node可执行文件与脚本文件之间的命令行参数 
      stdout ==> 标准输出流 console.log === process.stdout.write()
      stdin ==> 标准输入流
      stderr ==> 标准错误流
      env ==> 对象 当前shell的环境变量
      exitCode ==> 进程退出时的代码
      version
      versions ==> 对象 版本和依赖
      config ==> 
        包含用来编译当前node执行文件的javascript配置选项的对象，这与执行
        ./configure脚本生成的config.gypi文件结果是一样的
      pid ==> 当前进程号
      title ==> 进程名
      arch ==> 当前cpu构架
      platform ==> 运行程序所在的平台
      mainModule ==> require.main的备选方案

    事件&描述
      exit ==> 进程退出时触发
      beforeExit ==>
        当Node.js的事件循环数组已经为空，并且没有额外的工作被添加进来,事件'beforeExit'会被触发
      [uncaughtException](https://github.com/zhl2liuli/node-api-cn/blob/master/process/event_uncaughtexception.md)  当一个异常冒泡回到事件循环，触发这个事件。
        如果给异常添加了监视器，默认的操作（打印堆栈跟踪信息并退出）就不会发生。
      Signal ==> 当进程接收到信号时就触发
        SIGTERM 和 SIGINT 在非windows平台绑定了默认的监听器，这样进程以代码128 + signal number结束之前，可以重置终端模式。  如果这两个事件任意一个绑定了新的监听器，原有默认的行为会被移除(Node.js不会结束)
      unhandledRejection ==>
        在事件循环的一次轮询中，一个Promise被rejected，并且此Promise没有绑定错误处理器
        事件就会被触发。
      warning

      方法：
        abort() ==> node 触发 abort 事件。会让 node 退出并生成一个核心文件.
        chdir(directory) ==> 变更Node.js进程的当前工作目录，如果变更目录失败会抛出异常.
        cwd() ==> 返回当前进程的工作目录
        exit([code]) ==>使用指定的code结束进程。如果忽略，将会使用code0
        getgid() ==> 获取进程的群组标识,这个函数仅在 POSIX 平台上可用
                     在Windows或Android平台无效.
        setgid(id)
        getuid()/setuid(id) ==> 获取/设置进程的用户标识,POSIX平台上可用
        getgroups()/setgroups(groups) ==> 返回/设置进程的群组 iD 数组，设置需要root权限
                                          POSIX平台上可用
        initgroups(user, extra_group) ==> 读取 /etc/group ，并初始化群组访问列表，
                                         使用成员所在的所有群组,POSIX平台上可用
        kill(pid[, signal]) ==> 发送信号给进程. pid是进程id，
                            并且signal是发送的信号的字符串描述,信号名是字符串.
        memoryUsage() ==> 返回一个对象，描述了 Node 进程所用的内存状况，单位为字节
                {
                  rss: 4935680,   
                  heapTotal: 1826816,  ==>heapTotal和heapUsed代表V8的内存使用情况。
                  heapUsed: 650472,
                  external: 49879   ==>代表V8管理的，绑定到Javascript的C++对象的内存使用情况
                }
        nextTick(callback) ==> 一旦当前事件循环结束，调用回到函数
            事件轮询随后的ticks 调用，会在任何I/O事件（包括定时器）之前运行.
        umask([mask]) ==> 设置或读取进程文件的掩码,设置时返回值为之前的掩码。
        uptime() ==> 返回 Node 已经运行的秒数。
        hrtime() ==> 返回当前进程的高分辨时间,返回当前时间以[seconds, nanoseconds]
                      该值与日期无关，因此不受时钟漂移的影响。主要用途是可以通过精确的时间间隔，来衡量程序的性能.可接受上一次调用的返回值做参数。
  2.console
    方法：
      log([data][,...]) ==> 向标准输出流打印字符并以换行符结束
      error([data][,...]) ==> 输出错误消息的
      warn([data][,...]) ==> 输出警告消息
      dir(obj[,options]) ==> 用来对一个对象进行检查（inspect）
          options ==> showHidden=>boolean true=>对象中的不可枚举属性和symbol属性也会显示
                      depth=>number 
                      colors=>boolean
      time(str) ==> 输出时间，表示计时开始
      timeEnd(str) ==> 结束时间
      trace(message[,...]) ==> 当前执行的代码在堆栈中的调用路径,测试函数运行很有帮助
      assert(value[,message][,...]) ==> 用于判断某个表达式或变量是否为真，接收两个参数，
                                        第一个参数是表达式，第二个参数是字符串
      count([str]) ==> 维护一个指定label的内部计数器并且输出到stdout指定label调用的次数.
      countReset([str])
      info() ==> console.log()的一个别名
  3.Buffer
    该类用来创建一个专门存放二进制数据的缓存区
    Buffer属性：
      poolSize ==> 这是用于决定预分配的、内部Buffer实例池的大小的字节数

    Buffer方法：
      alloc(alloc(size[,fill[,encoding]]))
      allicUnsafe(size) ==> 不会分配内存
      allicUnsafeSlow(size) ==> 应当仅仅作为开发者已经在他们的应用程序中
                                观察到过度的内存保留之后的终极手段使用
      byteLength(string,encoding) ==> 返回字节长度
      compare(buf1, buf2) ==> 比较buf1和buf2 ，通常用于Buffer实例数组的排序。
                              相当于调用buf1.compare(buf2)
      concat(list, totalLength) ==> 返回一个合并了list中所有Buffer实例的新建的Buffer
          list: buf组成的array 
          totalLength: 实际总长度，最好提供这样可以提高速度
      from(arrayBuffer[,byteOffset[.length]])
      from(buffer) ==> 将传入的buffer数据拷贝到一个新建的Buffer实例
      from(obj) ==> obj调用Symbol.toPrimitive or valueOf()方法转化成String传入from
      from(string[,encoding])
      isbuffer(obj)
      isencoding(encoding) ==> 是否支持此编码

    buffer属性：
      INSPECT_MAX_BYTES: 当调用buf.inspect()时返回的最大字节数
      kMaxLength: 分配给单个Buffer实例的最大内存
    buffer方法：
       SLowBuffer()

    参数解释：
      encoding:
        ascii ==> 仅支持7位ASCII数据。如果设置去掉高位的话，这种编码是非常快的。
        utf8 ==> 多字节编码的Unicode字符。许多网页和其他文档格式都使用UTF-8 。
        utf16le ==> 2或4个字节，小字节序编码的Unicode字符,支持代理对（U+10000至U+10FFFF）。
        ucs2 ==> utf16le的别名。
        base64 ==> Base64编码。
        latin1 ==> 一种把Buffer编码成一字节编码的字符串的方式。
        binary ==> latin1的别名。
        hex ==> 将每个字节编码为两个十六进制字符。
      offset ==> 开始写入string前要跳过的字节数,默认:0
      length ==> 要写入的字节数。默认: buf.length - offset
      encoding ==> string 的字符编码。默认: 'utf8'
    创建Buffer类的实例：
      alloc(size[,fill[,encoding]]) ==> 返回一个指定大小的Buffer实例,
                                        如果没有设置 fill，则默认填满0
      allocUnsafe(size) ==> 返回一个指定大小的 Buffer 实例，但是它不会被初始化，
                            所以它可能包含敏感的数据.
      allocUnsafeSlow(size) 
      from(array) ==> 返回一个被 array 的值初始化的新的 Buffer 实例
      from(arrayBuffer[,byteOffset[.length]]) ==> 
          返回一个新建的与给定的ArrayBuffer共享同一内存的Buffer
      from(buffer) ==> 复制传入的Buffer实例的数据，并返回一个新的Buffer实例
      from(string[,encoding]) ==> 返回一个被string的值初始化的新的Buffer实例
    实例属性：
      buffer ==> 向创建该Buffer的底层的ArrayBuffer对象
      length ==> 返回这个buffer的bytes数,length 是buffer对象所分配的内存数，
                它不会随着这个buffer对象内容的改变而改变
      [index] ==> 获取或设置指定的字节。返回值代表一个字节
      values ==> 创建并返回一个包含buf的值（字节）的Iterator.
    实例方法：

      write(string[,offset[,length]][,encoding]) ==>
          返回number类型，表示写入了多少8位字节流
      toString([encoding[, start[, end]]]) ==> 解码成encoding指定的string类型
      toJSON() ==> 转换成JSON对象
      equals(otherBuffer) ==> 比较两个缓冲区是否相等
      compare(otherBuffer) ==> 比较两个Buffer对象，返回一个0,1,-1，
                               表示buf在otherBuffer相同，之前，之后
      copy(targetBuffer[,targetStart[,sourceStart[,sourceEnd]]]) ==>
          buffer 拷贝，源和目标可以相同
      slice(start,[,end])
      fill(value[,offset][,end]) ==> 使用指定的value来填充这个buffer
      values() ==> 创建并返回一个包含buf的值（字节）的Iterator.
      key() ==> 键名Interator
      entries() ==> 从buf的内容中，创建并返回一个[index, byte]形式的Iterator.
      includes(value[,byteoffset][,encoding]) ==> value是否在buf中存在，
            value：str或ASCII值buffer，
      indexOf(value[,offset][,encoding]) ==> 返回首次出现索引，没有返回-1
            value：
                  str:则 value 根据 encoding 的字符编码进行解析。
                  Buffer或Uint8Array: 则value会被作为一个整体使用。如果要比较部分 Buffer,可使用buf.slice()。
                  数值: 则value会解析为一个0至255之间的无符号八位整数值。
      lastIndexOf(value[,offset][,encoding]) ==> 从后向前
      swap16() ==> 将buf解析为一个无符号16位的整数数组，并且以字节顺序原地进行交换。
                   如果buf.length不是2的倍数，则抛出RangeError错误
      swap32() ==> 4的倍数
      swap64() ==> 8的倍数

      writeUIntBE(value, offset, bytelength[,noAssert]) ==>将value写入到buffer里，
          它由offset和byteLength决定，最高支48位无符号整数,大端对齐.noAssert值为true 时，不再验证value和offset的有效性
      writeUIntLE(value, offset, byteLength[,noAssert]) ==> 小端对齐
      writeIntLE(value, offset, byteLength[,noAssert]) ==> 有符号整数，小端对齐
      writeIntBE(value, offset, byteLength[,noAssert]) ==> 有符号，大端对齐

      readUIntLE(offset, byteLength[, noAssert]) ==> 支持读取48位以下的无符号数字,小端对齐
      readUIntBE(offset, byteLength[, noAssert]) ==> 支持读取48位以下的无符号数字,大端对齐
      readIntLE(offset, byteLength[, noAssert])
      readIntBE(offset, byteLength[, noAssert])

      readUInt8(offset[, noAssert]) ==> 根据指定的偏移量，读取一个无符号8位整数
      readUInt16(offset[, noAssert])
      readUInt16LE(offset[, noAssert]) ==> 使用特殊的endian字节序格式读取一个无符号16位整数
      readUInt16BE(offset[, noAssert])
      readUInt32LE(offset[, noAssert])
      readUInt32BE(offset[, noAssert])
      readInt8(offset[, noAssert]) ==> 读取一个有符号8位整数
      readInt16(offset[, noAssert])
      readInt16LE(offset[, noAssert])
      readInt16BE(offset[, noAssert])
      readInt32LE(offset[, noAssert])
      readInt32BE(offset[, noAssert])
      readFloatLE(offset[, noAssert]) ==> 指定的endian字节序格式读取一个32位双浮点数
      readFloatBE(offset[, noAssert])
      readDoubleLE(offset[, noAssert]) ==> endian字节序格式读取一个64位双精度数
      readDoubleBE(offset[, noAssert])

      writeUInt8(offset[, noAssert]) ==> 根据指定的偏移量，写入一个无符号8位整数
      writeUInt16(offset[, noAssert])
      writeUInt16LE(offset[, noAssert]) ==> 使用特殊的endian字节序格式写入一个无符号16位整数
      writeUInt16BE(offset[, noAssert])
      writeUInt32LE(offset[, noAssert])
      writeUInt32BE(offset[, noAssert])
      writeInt8(offset[, noAssert]) ==> 写入一个有符号8位整数
      writeInt16(offset[, noAssert])
      writeInt16LE(offset[, noAssert])
      writeInt16BE(offset[, noAssert])
      writeInt32LE(offset[, noAssert])
      writeInt32BE(offset[, noAssert])
      writeFloatLE(offset[, noAssert]) ==> 指定的endian字节序格式写入一个32位双浮点数
      writeFloatBE(offset[, noAssert])
      writeDoubleLE(offset[, noAssert]) ==> endian字节序格式写入一个64位双精度数
      writeDoubleBE(offset[, noAssert])
  4.__filename 
    表示当前正在执行的脚本的文件名,它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径
  5.__dirname
    表示当前执行脚本所在的目录
  6.setTimepot(callback,ms)
  7.clearTimeout(t) ==> 参数 t 是通过 setTimeout() 函数创建的定时器。
  8.setInterval(callback,ms)
  9.clearInterval(t)
  10.Module
    module属性：
      filename ==> 当前模块的文件夹名称 等同于__filename 和 path.dirname(__filename)
      exports ==>  由模块系统创建的对象
      parent ==> 最先引用该模块的模块
      children ==> 被该模块引用的模块对象
      filename ==> 模块的完全解析后的文件名
      id ==> 模块的标识符,通常是完全解析后的文件名。
      loaded ==> 模块是否已经加载完成，或正在加载中
      paths ==> 模块的搜索路径
    module方法：
      require(id) ==> module.require方法提供了一种类似require() 
                      从原始模块被调用的加载模块的方式。
  11.require
    require属性：
      cache ==> 被引入的模块将被缓存在这个对象中,从此对象中删除键值对
                        将会导致下一次 require 重新加载被删除的模块
      main ==>  Node.js直接运行一个文件时，require.main会被设为它的module
                require.main === module 来判断一个文件是否被直接运行
    require方法：
      resolve(request[,options]) ==> 此操作只返回解析后的文件名，不会加载该模块
```