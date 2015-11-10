 # 添加页面
 
 在这部分指南里面我们的任务是给Phoenix应用添加两个新页面。一个是纯粹的静态页面，另外一个将从URL中读取一个参数并将它传递给模板，并在页面中显示出来。在整个过程中，我们将对Phoenix应用的组件有所了解，包括：路由（router），控制器（controller），视图（views）和模板（templates）。
 
 当一个Phoenix应用创建好时，它为建立如下目录结构：
 ```
├── _build
├── config
├── deps
├── lib
├── priv
├── test
├── web
```
这部分指南里我们会把大部分精力花在`web`目录，它的目录结构如下：
```
├── channels
├── controllers
│   └── page_controller.ex
├── models
├── router.ex
├── templates
│   ├── layout
│   │   └── app.html.eex
│   └── page
│       └── index.html.eex
└── views
|   ├── error_view.ex
|   ├── layout_view.ex
|   └── page_view.ex
└── web.ex
```

`controllers`、`templates`、`views`三个目录下的文件创建了我们在前面指南中所看到的“Welcome to Phoenix!”页面。我们将会在接下来看到我们是如何复用这些代码的。

我们应用的所有静态文件如各类css文件、图片和js文件都被放在`priv/static`目录里。我们把需要构建的静态文件以及用到`priv/static`目录文件编译的`app.js`和`app.css`文件等放在`web/static`目录里。这里我们不对它们做任何改变，但是最好知道它们在哪以便将来引用。
```
priv
└── static
    └── images
        └── phoenix.png
```
```
web
└── static
    ├── css
    |   └── app.css
    ├── js
    │   └── app.js
    └── vendor
        └── phoenix.js
```

`lib`目录里面也有一些我们需要了解的东西。我们的应用的主要部分就放在`lib/hello_phoenix/endpoint.ex`里面，而应用文件（用来启动应用和它的组件）就在`lib/hello_phoenix.ex`。
```
lib
├── hello_phoenix
|   ├── endpoint.ex
│   └── repo.ex
└── hello_phoenix.ex
```

了解得够多了，让我们编写第一个页面。

##<strong><em>一个新路由</em></strong>

路由会匹配一个HTTP请求并将其交由控制器，控制器会根据它拥有的动作来进行处理。Phoenix在`web/router.ex`文件里面为我们设置路由。这也是这部分指南里我们要写代码的地方。

创建与运行指南里面的“Welcome to Phoenix!”页面的路由如下：
```
get "/", PageController, :index
```
来分析一些这个路由告诉我们什么信息。访问[http://localhost:4000/](http://localhost:4000/)这个行为意味着对网站的根目录发送了一个get请求。所有这种请求都会被`web/controllers/page_controller.ex`文件里面的`HelloPhoenix.PageController `模块的`index`函数处理。

我们将要建立一个路由为[http://localhost:4000/hello](http://localhost:4000/hello)的页面，它只会简单地显示“Hello World, from Phoenix!”。

要创建一个新页面，第一件事就是为它定义一个路由。在你喜欢的文本编辑器里面打开`web/router.ex`。它的结构如下：
```
defmodule HelloPhoenix.Router do
  use HelloPhoenix.Web, :router

  pipeline :browser do
    plug :accepts, ["html"]
    plug :fetch_session
    plug :fetch_flash
    plug :protect_from_forgery
    plug :put_secure_browser_headers
  end
  
  pipeline :api do
    plug :accepts, ["json"]
  end

  scope "/", HelloPhoenix do
    pipe_through :browser # Use the default browser stack

    get "/", PageController, :index
  end

  # Other scopes may use custom stacks.
  # scope "/api", HelloPhoenix do
  #   pipe_through :api
  # end
end
```
现在我们讲不讨论管道（pipeline）和`scope`的用法，而是专注于路由。（如果你很好奇，可以查阅[路由部分的指南](http://www.phoenixframework.org/docs/routing)）

让我们添加一个路由来匹配`/hello`的`GET`请求，并把它交给`HelloPhoenix.HelloController`的动作函数`index`处理。
```
get "/hello", HelloController, :index
```
`router.ex`文件里面的`scope "/"`现在看起来应该跟下面一样：
```
scope "/", HelloPhoenix do
  pipe_through :browser # Use the default browser stack

  get "/", PageController, :index
  get "/hello", HelloController, :index
end
```
##<strong><em>一个新控制器</em></strong>

控制器是Elixir模块，控制器的行为就是在里面定义的Elixir函数。行为的目的就是搜集数据和完成和渲染有关的任务。我们的路由明确指出了我们需要一个名为`HelloPhoenix.HelloController`的模块，里面有一个`index/2`的行为函数。

为了达到路由的要求，让我们创建一个新文件`web/controllers/hello_controller.ex`，并写入下面的代码：
```
defmodule HelloPhoenix.HelloController do
  use HelloPhoenix.Web, :controller

  def index(conn, _params) do
    render conn, "index.html"
  end
end
```