# 创建和运行

第一篇指南的目的是为了让你尽快地创建并运行一个Phoenix应用。

在开始之前，请花一点时间阅读[阅读指南](http://www.phoenixframework.org/docs/installation)。记得提前安装好任何需要的依赖，这样我们就能够让我们的应用顺利创建并运行。

从这点出发，我们应该提前安装好Elixir，Erlang，Hex，和Phoenix镜像。同时，我们也应该提前安装好PostgreSQL和node.js来满足默认配置。

一切就绪后，我们就可以开始啦！

可以从Phoenix应用的根目录执行命令`mix phoenix.new`来创建新应用。Phoenix接收当前目录的相对路径或者绝对路径。假设我们将要创建的应用的名字为`hello_phoenix`，下面的命令都是有效的。
```
$ mix phoenix.new /Users/me/work/elixir-stuff/hello_phoenix
```
```
$ mix phoenix.new hello_phoenix
```
>在开始之前有一个关于[Brunch.io](http://brunch.io/)的注意事项：Phoenix默认使用Brunch.io作为静态文件管理器。Brunch.io的依赖是通过node.js的包管理器来安装的，而不是mix。因此Phoenix将在`mix phoenix.new`命令完成后提示我们来安装它。如果选“否”并且也不用`npm install`安装相应依赖的话，应用在运行时会抛出错误，而且静态文件可能无法被正确加载。如果不想使用Brunch.io，只要在执行`mix phoenix.new`命令时设置参数`--no-brunch`即可。

