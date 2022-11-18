# Linux命令

+ 切换到管理员权限，初次用`sudo passwd root`设置root的密码，然后之后就可以用`su`命令输入root的密码直接切换。由root切换到普通用户可以直接`exit`或者用`su - teswin`。

+ ```
  gcc hello.c -o hello
  ```

  `gcc`的`-o`选项指定了输出文件的名称, 如果将`-o hello`改为`-o hi`, 将会生成名为`hi`的可执行文件. 如果不使用`-o`选项, 则会默认生成名为`a.out`的文件, 它的含义是[assembler output](http://en.wikipedia.org/wiki/A.out). 在命令行输入

  ```
  ./hello
  ```

  就能够运行改程序. 命令中的`./`是不能少的, 点代表了当前目录, 而`./hello`则表示当前目录下的`hello`文件. 与Windows不同, Linux系统默认情况下并不查找当前目录, 这是因为Linux下有大量的标准工具(如`test`等), 很容易与用户自己编写的程序重名, 不搜索当前目录消除了命令访问的歧义.

+ `ls -lh xxx`列出当前目录下的详细信息，一般看权限之类的，然后h是用KB、MB、GB为单位输出显示的。