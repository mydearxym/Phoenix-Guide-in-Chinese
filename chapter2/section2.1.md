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

一切就绪，让我们输入`phoenix.new`和你项目的相对路径。
```
$ mix phoenix.new hello_phoenix
* creating hello_phoenix/README.md
. . .
```

Phoenix会为我们生成目录结构和所有应用程序中需要的文件。当它完成它的任务时，它将会询问我们是否想让它为我们安装依赖。在这里我们选是
```
Fetch and install dependencies? [Yn] y
* running npm install && node node_modules/brunch/bin/brunch build
* running mix deps.get
```

一旦依赖安装完成，终端就会提示我们进入项目目录对项目初始化。
```
We are all set! Run your Phoenix application:

    $ cd hello_phoenix
    $ mix ecto.create
    $ mix phoenix.server

You can also run your app inside IEx (Interactive Elixir) as:

    $ iex -S mix phoenix.server
```
Phoenix假设我们的PostgreSQL数据库有一个拥有基本权限的用户名为`postgres`，密码为“postgres”。如果事实不是这样，请查阅[ecto.create](http://www.phoenixframework.org/docs/mix-tasks#section--ecto-create-)部分的指南。

搞定后，执行下面的命令：
```
$ cd hello_phoenix
$ mix ecto.create
$ mix phoenix.server
```
>注意：如果你第一次执行这个命令，Phoenix可能会要求你安装Rebar，一个用来构建Erlangbao的工具。请安装它。

如果创建新项目时选择不让Phoenix来安装依赖，`phoenix.new`终端会在我们想安装以来的时候告诉我们必要的步骤。
```
Fetch and install dependencies? [Yn] n

Phoenix uses an optional assets build tool called brunch.io
that requires node.js and npm. Installation instructions for
node.js, which includes npm, can be found at http://nodejs.org.

After npm is installed, install your brunch dependencies by
running inside your app:

    $ npm install

If you don't want brunch.io, you can re-run this generator
with the --no-brunch option.


We are all set! Run your Phoenix application:

    $ cd hello_phoenix
    $ mix deps.get
    $ mix phoenix.server

You can also run it inside IEx (Interactive Elixir) as:

    $ iex -S mix phoenix.server
```

Phoenix的默认监听端口为4000.可以在浏览器打开[http://localhost:4000/](http://localhost:4000/)，不出意外的话我们会看到下面的欢迎页面。

![Phoenix Welcome Page](https://www.filepicker.io/api/file/0t1GuSinQ1yeRhw3ZgLK)

如果你看到上面的画面，恭喜！你的应用已经在运行中。

我们的应用是在iex终端里面运行。要停止的话只需要按Ctrl+c即可。

接下来我们将定制我们的程序，让我们对一个Phoenix应用是如何搭建起来的有所了解。