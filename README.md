## funscript 包含个人日常开发脚本

___

[awesome-scripts](https://github.com/superhj1987/awesome-scripts) 项目提供了很多有用的脚本工具。

## 网络脚本的使用

### 下载并查看

```bash
curl -fsSL https://github.com/farfarfun/funscript/raw/master/funscript/develop/env.sh | cat
```

### 下载并执行

```bash
# curl -s 表示 silence 即不输出进度和错误信息
curl -s "https://github.com/farfarfun/funscript/raw/master/funscript/develop/env.sh" | bash -s
```


```bash
# 执行
curl "http://xxx.com/xx/xx.sh" | bash
# 脚本传参
curl http://example.com/script.sh | bash -s -- arg1 arg2 
```

curl 命令其它常见用法 

```bash
# -o 将文件保存到执行文件名, -O 保存成文件，并将 URL 的最后部分当作文件名。
curl  https://www.baidu.com -o 2.txt # 保存为 2.txt
curl -O https://www.example.com/foo/bar.html # 保存为 bar.html
# -d 发送 post 参数，-X 参数指定 HTTP 请求的方法
curl -d 'login=emma＆password=123' -X POST https://google.com/login
# -u参数用来设置服务器认证的用户名和密码。
curl -u 'bob:12345' https://google.com/login
```


## 关于 shell 脚本中的特殊变量 `$0、$?、$!、$$、$*、$#、$@`

-   **`$1～$n：`**添加到 Shell 的各参数值。`$1是第1参数、$2是第2参数…`
-   **`$0：`**Shell 本身的文件名
-   **`$#：`**添加到 Shell 的参数个数
-   **`$@：`**所有参数列表。如`"$@"用「"」括起来的情况、以"$1" "$2" … "$n"`的形式输出所有参数。
-   **`$\*：`**所有参数列表。如`"$*"用「"」括起来的情况、以"$1 $2 … $n"`的形式输出所有参数。
-   **`$!：`**Shell最后运行的后台 Process 的 PID
-   **`$$：`**Shell 本身的 PID（ProcessID）

```
# shell 测试脚本
#!/bin/bash

echo "\$# = params number: $#"
echo "\$0 = script file name: $0"
echo "\$1 = first param: $1"
echo "\$2 = second param: $2"
echo "\$@ = all params: $@"
echo "\$* = all params: $*"
echo "\$$ = shell pid: $$"
echo "\$! = last bg pid: $!"

# 运行 sh test.sh a b c d 可以看到输出
$# = params number: 4
$0 = script file name: test.sh
$1 = first param: a
$2 = second param: b
$@ = all params: a b c d
$* = all params: a b c d
$$ = shell pid: 58271
$! = last bg pid:
```

## Shell 重定向

Linux 中一切皆文件，包括标准输入设备（键盘）和标准输出设备（显示器）在内的所有计算机硬件都是文件。Linux 程序在执行任何形式的 I/O 操作时，都是在读取或者写入一个文件描述符。一个文件描述符只是一个和打开的文件相关联的整数，它的背后可能是一个硬盘上的普通文件、FIFO、管道、终端、键盘、显示器，甚至是一个网络连接。stdin、stdout、stderr 默认都是打开的，在重定向的过程中，0、1、2 这三个文件描述符可以直接使用。

| 文件描述符 | 文件名 | 类型 | 硬件 |
| --- | --- | --- | --- |
| 0 | stdin | 标准输入文件 | 键盘 |
| 1 | stdout | 标准输出文件 | 显示器 |
| 2 | stderr | 标准错误输出文件 | 显示器 |

Linux Shell 重定向分为两种，一种输入重定向，一种是输出重定向；从字面上理解，输入输出重定向就是「改变输入与输出的方向」的意思。

### 输出重定向

输出重定向是指命令的结果不再输出到显示器上，而是输出到其它地方，一般是文件中。这样做的最大好处就是把命令的结果保存起来，当我们需要的时候可以随时查询。

| 类 型 | 符 号 | 作 用 |
| --- | --- | --- |
| 标准输出重定向 | command >file | 以**覆盖**的方式，把 command 的正确输出结果输出到 file 文件中。 |
| command >>file | 以**追加**的方式，把 command 的正确输出结果输出到 file 文件中。 |  |
| 标准错误输出重定向 | command 2>file | 以覆盖的方式，把 command 的错误信息输出到 file 文件中。 |
| command 2>>file | 以追加的方式，把 command 的错误信息输出到 file 文件中。 |  |
| 正确输出和错误信息同时保存 | command >file 2>&1 | 以覆盖的方式，把正确输出和错误信息**同时保存到同一个文件**（file）中。 |
| command >>file 2>&1 | 以追加的方式，把正确输出和错误信息同时保存到同一个文件（file）中。 |  |
| command >file1 2>file2 | 以覆盖的方式，把正确的输出结果输出到 file1 文件中，把错误信息输出到 file2 文件中。 |  |
| command >>file1 2>>file2 | 以追加的方式，把正确的输出结果输出到 file1 文件中，把错误信息输出到 file2 文件中。 |  |
| command >file 2>file | 【**不推荐**】这两种写法会导致 file 被打开两次，引起资源竞争，所以 stdout 和 stderr 会互相覆盖，我们将在《[结合Linux文件描述符谈重定向，彻底理解重定向的本质](http://c.biancheng.net/view/vip_3241.html)》一节中深入剖析。 |

注意：输出重定向的完整写法其实是fd>file或者fd>>file，其中 fd 表示文件描述符，如果不写，默认为 1，也就是标准输出文件。另外 `>` 两边不要加空格，否则会解析失败

最常见的指令莫过于

```
ls -l &gt;out.log 2&gt;&amp;1 # 将命令 ls -l 的正确结果和错误结果一起保存在 out.log 文件
ls -l &gt;out.log 2&gt;err.log # 将命令 ls -l 的正确结果保存在 out.log 文件, 错误结果保存在 err.log
ls java &amp;&gt;/dev/null # 如果你既不想把命令的输出结果保存到文件，也不想把命令的输出结果显示到屏幕上，干扰命令的执行，那么可以把命令的所有结果重定向到 /dev/null 文件中。注意没有 &amp; 的话, 错误结果还是会输出
```

### 输入重定向

输入重定向就是改变输入的方向，不再使用键盘作为命令输入的来源，而是使用文件作为命令的输入。

| 符号 | 说明 |
| --- | --- |
| command <file | 将 file 文件中的内容作为 command 的输入。 |
| command <<END | 从标准输入（键盘）中读取数据，直到遇见分界符 END 才停止（分界符可以是任意的字符串，用户自己定义）。 |
| command file2 | 将 file1 作为 command 的输入，并将 command 的处理结果输出到 file2。 |

举个栗子，Linux wc 命令可以用来对文本进行统计，包括单词个数、行数、字节数，它的用法如下，其中，-c选项统计字节数，-w选项统计单词数，-l选项统计行数。

```
$ cat readme.txt  #预览一下文件内容
C语言中文网
http://c.biancheng.net/
成立7年了
日IP数万
$ wc -l &lt;readme.txt  #输入重定向
4
```

## 问号表达式

下面的命令组使用了在 Bash 里称为 **list constructs** 的工具。它允许你通过 &&（代表 **and**） 和 || (代表 **or**） 将命令串到一起。上面的命令将会执行 `ls java` 命令，如果退出码是 0 命令 echo "lala" 将被执行。但如果 `ls java` 的退出码为 1 ，圆括号里的命令将在之后被执行。圆括号里的命令也通过 && 、 || 被串到一起。

```
$ ls # 当前目录下有三个文件夹
arthas  project temp
$ ls java &amp;&amp; echo "lala" || (ls temp &amp;&amp; echo "blabla" || echo "fail") # java 文件夹不存在，执行会异常，temp 文件夹存在可以正常输出里面的文件
ls: java: No such file or directory
err.log                log.text               self-installer-test.sh self-installer.sh      test.sh
blabla
```

## exit code

在 Unix 和 Linux 系统中，程序可以在执行终止后传递值给其父进程。这个值被称为退出码（exit code）或退出状态（exit status）。在 POSIX 系统中，惯例做法是当程序成功执行时传递 0 ，当程序执行失败时传递 1 或比 1 大的值。

如果你不定义状态码，它仍然存在于你的脚本中。如果你不定义恰当的退出码，执行失败的脚本可能会返回成功的状态，这样就会导致问题出现！

举个栗子，下面的脚本既会执行 touch 命令也会执行 echo 命令。当我们以非 root 用户执行这个脚本时 touch 命令将会执行失败，可以看到，当执行 ./tmp.sh 后退出码是 0 ，而 0 代表脚本执行成功，虽然 touch 命令执行失败了。因为我们没有指定退出码所以脚本以最后一个命令执行后的状态码退出。在这个例子中，最后运行的是 echo 命令，这个命令确实执行成功了。

```
# tmp.sh 内容
#!/bin/bash
touch /root/test
echo created file
# 执行上面脚本看下
$ ./tmp.sh 
touch: cannot touch '/root/test': Permission denied
created file
$ echo $?
0
```

因此我们最好在脚本中自定义退出码，类似下面这样

```
#!/bin/bash

touch /root/test 2&gt; /dev/null

if [$? -eq 0 ]
then
  echo "Successfully created file"
  exit 0
else
  echo "Could not create file" &gt;&amp;2
  exit 1
fi
```