<strong>安装</strong>

---

在[总览](./Overview.md)中，我们大致浏览了Phoenix生态系统和它的组件之间是如何相互关联的。现在是时候来安装，我们跳进[启动和运行指南]()前可能需要的任何软件。

请看看下面的这份名单，并确保安装所有您的系统需要的软件。在任何令人沮丧的问题发生前安装好相应的依赖。

##<strong><em>Elixir</em></strong>

Phoenix是用Elixir来编写的，因此我们的应用也将用Elixir来编写。没有它我们将难以深入Phoenix的应用！Elixir官网有一个不错的[安装页面](http://elixir-lang.org/install.html)来帮助你。

如果是第一次把Elixir安装在电脑上，那么之后还需要安装包管理器Hex。Hex是Phoenix应用运行（安装依赖）和安装其它Elixir依赖的必要条件。

安装Hex的命令如下（注：可能要翻墙）

```
$mix local.hex
```

##<strong><em>Erlang</em></strong>

Elixir代码编译成Erlang字节码到Erlang虚拟机上运行。如果没有Erlang，Elixir代码不能在虚拟机上运行，​​所以我们需要安装Erlang。

当我们从Elixir[安装页面](http://elixir-lang.org/install.html)通过安装指导安装Elixir时，我们通常也安装了Erlang。如果Erlang
没有随着Elixir一起安装，请按照Elixir安装指南中的[Erlang指南](http://elixir-lang.org/install.html#installing-erlang)部分来操作。

##<strong><em>Phoenix</em></strong>

一旦我们成功安装了Elixir和Erlang，我们就可以开始安装Phoenix的mix镜像。一个mix镜像就是一个zip压缩包，包含着一个应用程序和它的编译过的beam文件。这个镜像会绑定到一个特定的版本。这里所说的镜像就是我们用来创建一个新的，基础的Phoenix应用的框架。

安装Phoenix的镜像的命令如下：（注：可能需要翻墙）

```
$ mix archive.install https://github.com/phoenixframework/phoenix/releases/download/v1.0.3/phoenix_new-1.0.3.ez
```

>注意：如果Phoenix镜像不能通过这个命令正常安装，我们可以从上面链接对应的网址手动下载镜像到本地，然后执行下面的命令：
```
$mix archive.install /path/to/local/phoenix_new.ez
```

##<strong><em>Plug, Cowboy, and Ecto</em></strong>

这三个是Elixir或者Erlang关于Phoenix项目的默认组成部分。我们不需要做什么特别的事情来安装它们。如果创建一个新项目时我们使用mix命令来安装依赖，这三个东西会被自动处理。如果不使用mix，Phoenix会在项目创建后告诉我们怎么做。

##<strong><em>node.js (>= 0.12.0)</em></strong>

Node是一个可选的依赖。因为Phoenix默认使用[brunch.io](http://brunch.io/)来编译静态文件（javascript，css等等）。Brunch.io使用node包管理器（npm）来安装它的依赖，然而npm又需要node.js。

如果我们没有任何静态文件，或者我们想用其它构建工具，我们可以通过在创建新项目时设置`--no-brunch`参数，这样node就不会作为依赖。

我们可以从[这里](https://nodejs.org/en/download/)下载node.js。注意Phoenix要求的node版本>=0.12.0。

Mac OS X可以从[homebrew](http://brew.sh/)下载node.js。

注意：io.js，io.js 是一个衍生自 Node.js，并兼容 npm 的开发平台，目前仍不清楚适合Phoenix与否。

Debian/Ubuntu用户可能会看到下列错误：

```
sh: 1: node: not found
npm WARN This failure might be due to the use of legacy binary "node"
```

这是因为Debian与node在字节码上有矛盾：[可以看看stackoverflow上的讨论](http://stackoverflow.com/questions/21168141/can-not-install-packages-using-node-package-manager-in-ubuntu)

有两个选项来修复这个问题，比如：

+ 安装nodejs-legacy：
```
$ apt-get install nodejs-legacy
```
或者

+ 创建一个链接
```
$ ln -s /usr/bin/nodejs /usr/bin/node
```

##<strong><em>PostgreSQL</em></strong>

PostgreSQL是一个关系型数据库服务器。Phoenix配置应用程序时在默认情况下使用它，但是我们可以通过创建一个新的应用程序时设置`--database MySQL`参数来切换到MySQL。

当我们在阅读Ecto模型部分的指南时，我们将使用PostgreSQL和Postgrex驱动。为了更好地理解对应的例子，我们应该安装PostgreSQL。PostgreSQL维基百科针对不同系统都有相应的[安装指南](https://wiki.postgresql.org/wiki/Detailed_installation_guides)。

Postgrex是Phoenix的直接依赖，因此当我们开始运行应用时它会随着剩下的依赖自动被安装。

##<strong><em>inotify-tools (for linux users)</em></strong>

这是只有Linux用户才需要安装的，为了代码热更新。（Mac OS X或者Windows用户可以安全地避免它。）

Linux用户可以查阅[inotify-tools维基](https://github.com/rvoicilas/inotify-tools/wiki)来进行相关操作。