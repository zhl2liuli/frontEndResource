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

### util(工具)

```text
  til 模块主要用于支持 Node.js 内部API的需求, 大部分实用工具也可用于应用程序与模块开发者
  引入：
    const util = require('util');
  方法：
    callback(origin) ==> 返回一个异步函数或者promise，返回的函数接受error/rejected做参数一
        resolved做第二个参数
    debuglog(section) ==> 创建一个函数，基于NODE_DEBUG环境变量的存在与否有条件地写入调试信息到stderr
    deprecate(fn, str) 方法会包装给定的 function 或类，并标记为废弃的
    format(format[, ...args]) ==> 一个格式化后的字符串，使用第一个参数作为一个类似printf的格式
        参数一: 
            %S
            %d
            %i ==> Integer
            %f
            %J
            %o
            %O
            %% ==>  单个百分号（'%'）。不消耗参数
    inspect(object, options) ==> 返回object的字符串表示
        showHidden:boolean 如果为 true，则object的不可枚举的符号与属性也会被包括在格式化后的结果中。
            默认为false。
        depth:number 指定格式化object时递归的次数。这对查看大型复杂对象很有用。默认为 2。
            若要无限地递归则传入null。
        colors:boolean 如果为true，则输出样式使用ANSI颜色代码。 默认为false。
            颜色可自定义，详见自定义 util.inspect 颜色。
        customInspect:boolean 如果为false，则object上自定义的inspect(depth, opts)函数不会被调用。
            默认为true。
        showProxy:boolean 如果为true，则Proxy对象的对象和函数会展示它们的target和handler对象。 
            默认为 false。
        maxArrayLength:number 指定格式化时数组和TypedArray元素能包含的最大数量。默认为 100。 
            设为null则显式全部数组元素。 设为0或负数则不显式数组元素。
        breakLength:number 一个对象的键被拆分成多行的长度。 设为Infinity则格式化一个对象为单行。
            默认为 60。
    inspect.defaultOptions ==> 对被util.inspect使用的默认选项进行自定义
    inspect.styles ==> 关联一个样式名到一个util.inspect.colors颜色 
    inspect.colors ==> 颜色设置
    [util.inspect.custom](depth,opts)==> 可被用于声明自定义的查看函数

    promisify(original) ==> 
```

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
### Stream(流)

```text
  四种流类型：
    Readable
    Writable
    Duplex
    Transform ==> 操作被写入数据，然后读出结果
    声明类：
      const {Readable/Writable/Duplex/Transform} = require('stream');
    Writable和 Readable流都会将数据存储到内部的缓存（buffer）中。这些缓存可以通过相应的 
      writable._writableState.getBuffer() 或 readable._readableState.buffer来获取

  事件：
  close ==> 事件将在流或其底层资源（比如一个文件）关闭后触发。
  data ==> 当有数据可读时触发, 当流转换到 flowing 模式时会触发该事件
          如果调用readable.setEncoding()方法明确为流指定了默认编码，
          回调函数将接收到一个字符串，否则接收到的数据将是一个Buffer实例
  drain ==> 如果调用stream.write(chunk)方法返回 false，
            流将在适当的时机触发'drain' 事件，这时才可以继续向流中写入数据 
  end ==> 没有更多的数据可读时触发
  error ==> 事件在写入数据出错或者使用管道出错时触发,
            注意: 'error' 事件发生时，流并不会关闭。
  finish ==> 在调用了stream.end()方法，且缓冲区数据都已经传给底层系统underlying system
             之后，finish事件将被触发。
  pip ==> 在可读流（readable stream）上调用stream.pipe()方法，并在目标流向(destinations)
          中添加当前可写流(writable)时，将会在可写流上触发pipe事件。回调函数接受src参数
          src为输出到目标可写流（writable）的源流（source stream）
  unpip ==> 移除
  readable ==> 事件将在流中有数据可供读取时触发

  Readable类实例方法：
    isPaused() ==> 返回可读流的当前操作状态
    pause() ==> 方法将会使flowing模式的流停止触发data事件， 进而切出flowing模式
    pip(destination, options) ==> 绑定一个Writable到readable上，将可写流自动切换
                到flowing 模式并将所有数据传给绑定的Writable。数据流将被自动管理
    unpip(destinnation) ==> 如果destination没有传入, 则所有绑定的流都会被分离.
    push(chunk，encoding)
    unshift(chunk，encoding)
    read(size)
    resume() ==> 重新触发data事件, 将暂停模式切换到流动模式
    setEncoding(encoding) ==> 会为从可读流读入的数据设置字符编码
    wrap(stream) ==> 主要是用来向前兼容的，如果你使用了旧的node库，还使用data事件触发和pause()，
                     则可以使用wrap来创建一个readable stream，这个stream是用旧的的数据源

  Writable类实例方法：
    write(chunk,encoding,callback) ==> 如果流需要等待drain事件触发才能继续写入数据，这里将返回false；
                                        否则返回 true
    cork() ==> 将强制所有写入数据都存放到内存中的缓冲区里
    uncork() ==> 将输出在stream.cork()方法被调用之后缓冲在内存中的所有数据。
        如果一个流多次调用了cork()方法，那么也必须调用同样次数的uncork()方法以输出缓冲区数据。
    end(chunk,encoding,callback) ==> 方法表明接下来没有数据要被写入Writable
    setDefaultEncoding(encoding) ==> 用于为Writable设置 encoding
```

### assert(断言)

```text
  assert模块提供了断言测试的函数，用于测试不变式。
  方法：
    equal(actual, expected, message) ==> 使用（==）测试actual参数与expected参数是否相等
    notEqual(actual, expected, message)

    strctEqual(actual,expected,message) ==> 使用===测试actual参数与expected参数是否全等
    notStrictEqual(actual, expected, message) ==> 使用!==测试actual参数与expected参数是否不全等

    deepequal(actual, expected, message) ==> 测试actual与expected是否深度相等(==)
        只测试[可枚举的自身属性]，不测试对象的[原型]、连接符、或不可枚举的属性
    notDeepequal(actual, expected, message) 

    deepStrictEqual(actual,expected,message) ==> 使用===，对象的原型也使用全等运算符比较，
        对象的类型标签要求相同，
    notDeepStrictEqual(actual,expected,message)

    doesNotThrow(block,error,message) ==> 被调用时，它会立即调用block函数
    fail(actual, expected[,message][,operator][,stackStartFunction]) ==> 抛出 AssertionError
    ifError(value) ==> 如果value为真，则抛出value。 可用于测试回调函数的error参数
    ok(value,message) ==> 测试value是否为真值。相当于assert.equal(!!value, true, message)
    (value,message) ==> ok(value,message)的别名
    throw(block,error,message) ==> 断言block函数会抛出错误,
                                   error参数可以是构造函数、正则表达式、或自定义函数
    注意：对于+0和-0，NAN和NAN比较用Object.is()比较。

```
### querystring

```text
  querystring模块提供了一些实用函数，用于解析与格式化URL查询字符串:
  引入：
    const querystring = require('querystring')
  方法：
    escape(str) ==> 对给定的str进行URL编码
    unescape(str) ==> 对给定的str进行解码
    stringify(obj,sep,eq,options) ==> 该方法通过遍历给定的 obj 对象的自身属性，生成 URL 查询字符串
    parse(str,sep,eq,options) ==> 该方法会把一个URL查询字符串str解析成一个键值对的集合
      sep ==> 用于界定查询字符串中的键值对的子字符串。默认为 '&'
      eq ==> 用于界定查询字符串中的键与值的子字符串。默认为 '='
      options ==> decodeURIComponent 解码查询字符串的字符时使用的函数。默认为 querystring.unescape()
                  maxKeys 指定要解析的键的最大数量。默认为 1000。指定为 0 则不限制。
    unescape(str) ==> 对给定的str进行解码
```
### url

```text
  url模块提供了一些实用函数，用于 URL 处理与解析。 
  url对象的所有属性都是在类的原型上实现为getter和setter
  引入:
    const url = require('url');
  url类实例：
    const {URL} = require('url');
    const myURL = new URL(input[,base]) ==> 如果input为相对url，则要指定基本base 
        input主机名中的Unicode字符将被使用Punycode算法自动转换为ASCII。
    实例属性：
      hash ==> 获取及设置URL的分段(hash)部分
      host ==> 获取及设置URL的主机(host)部分。
      hostname ==> 获取及设置URL的主机名(hostname)部分
      href ==> 获取及设置序列化的URL。
      origin ==> 获取只读序列化的URL origin部分。
      username ==> 获取及设置URL的用户名(username)部分
      password ==> 获取及设置URL的密码(password)部分
      pathname ==> 获取及设置URL的路径(path)部分。
      port ==> 获取及设置URL的端口(port)部分。
      protocal ==> 获取及设置URL的协议(protocol)部分
      search ==> 获取及设置URL的序列化查询(query)部分部分
      URLSearchParams ==> 获取表示URL查询参数的URLSearchParams对象。该属性是只读的
    实例方法：
      toJSON() ==> 返回序列化的URL,值与url.href和url.toString()的相同
                   当URL对象使用JSON.stringify(url)序列化时将自动调用该方法
      toString() ==> 返回序列化的URL
      resolve(from,to) ==> 会以一种Web浏览器解析超链接的方式把一个目标URL解析成相对于一个基础URL
      domainToASCII(domain) ==> 返回ASCII序列化的domain.无效域名，将返回空字符串
      domainToUnicode(domain) ==> 返回Unicode序列化的domain
      format(url,options) ==>url: 一个WHATWG URL对象
        options:
          auth {boolean} 如果序列化的URL字符串应该包含用户名和密码为true，否则为false。默认为true。
          fragment {boolean} 如果序列化的URL字符串应该包含分段为true，否则为false。默认为true。
          search {boolean} 如果序列化的URL字符串应该包含搜索查询为true，否则为false。默认为true。
          unicode {boolean} true 如果出现在URL字符串主机元素里的Unicode字符应该被直接编码而不是使用Punycode编码为true，默认为false。
      format(string/urnObj) ==> 如果是一个字符串，则通过url.parse()转换为一个对象。
        [格式化过程](https://github.com/nodejscn/node-api-cn/blob/master/url/url_format_urlobject.md)
      parse(urlStr,parseQueryString,slashesDenoteHost) ==> 解析一个 URL 字符串并返回一个 URL 对象
        parseQueryString {boolean} 如果为 true，则query属性总会通过querystring模块的parse() 方法生成
          一个对象。 如果为 false，则返回的 URL 对象上的 query 属性会是一个未解析、未解码的字符串。 默认为 false。
        slashesDenoteHost:如果为 true，则//之后至下一个/之前的字符串会被解析作为host。例如//foo/bar
        会被解析为{host:'foo',pathname:'/bar'}而不是{pathname:'//foo/bar'}。默认为 false。
  URLSearchParams类的实例：
    const { URLSearchParams } = require('url');
    const params = new URLSearchParams(string/obj/Iterable);
      string ==> 解析成一个查询字符串, 并且使用它来实例化一个新的URLSearchParams对象. 
                 如果string以'?'打头,则'?'将会被忽略.
      iterable ==> 可以是一个数组或者任何迭代对象, 每个键值对必须有两个元素  
    方法：
      append(name, value) ==> 在查询字符串中附加一个新的键值对
      delete(name) ==> 删除所有键为name的键值对 
      key() ==> 在每一个键值对上返回一个键的ES6迭代器。 
      value() ==> 返回查询参数序列化后的字符串，必要时存在百分号编码字符
      entries() ==> 返回一个ES6迭代器
      forEach(fn(value,key,searchparams)) ==> 遍历每一个子项调用fn
      get(name) ==> 返回键是name的第一个键值对的值。如果没有对应的键值对，则返回null
      getAll(name) ==> 返回组成的数组
      has(name) ==> 如果存在至少一对键是name的键值对则返回 true。
      sort() ==> 按现有名称就地排列所有的名称-值对
      toString() ==> 返回查询参数序列化后的字符串，必要时存在百分号编码字符
      set(name,value) ==> name相对应的值设置为value,已经存在键为name的键值对，
                          将第一对的值设为value并且删除其他对
  urlObject
    遗留的urlObject(require('url').Url)由url.parse()函数创建并返回,为了兼容历史版本。
    属性：
     auth ==> URL的用户名与密码部分
     hash ==> 包含 URL 的碎片部分，包括开头的 ASCII 哈希字符（#）
     host ==> URL 的完整的小写的主机部分，包括 port（如果有）
     hostname ==> host 组成部分排除 port 之后的小写的主机名部分
     href ==> 解析后的完整的 URL 字符串，protocol 和 host 都会被转换为小写的
     path ==> 一个 pathname 与 search 组成部分的串接
     pathname ==> 包含 URL 的整个路径部分
     port ==> host 组成部分中的数值型的端口部分
     port ==> host 组成部分中的数值型的端口部分
     protocol ==> URL 的小写的协议体制
     query ==> 不含开头 ASCII 问号（?）的查询字符串
     search ==> URL 的整个查询字符串部分，包括开头的 ASCII 问号字符（?）
     slashes ==> boolean，如果protocol中的冒号后面跟着两个ASCII斜杠字符（/）则值为true。
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