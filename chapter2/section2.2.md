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
对于`use HelloPhoenix.Web, :controller`的讨论我们留到[控制器指南](http://www.phoenixframework.org/docs/controllers)。现在我们主要讨论行为函数`index/2`。

一个控制器行为需要两个参数。一个是连接参数`conn`，用来传递与请求有关的数据包。第二个是`params`，用来传递请求中的参数。在这里，我们将不会用到`params`，并且给`params`加上`_`前缀来避免编译时的警告。

这个动作的核心`render conn, "index.html"`。这行代码告诉Phoenix寻找一个叫做`index.html.eex`的模板并渲染它。Phoenix会在以控制器名字命名的目录下寻找模板，也就是`web/templates/hello`。

注意：在这里使用一个原子（atom）作为模板的名字也是可以得。`render conn,:index`，但这样会让模板不清楚选择的是`index.html`还是`index.json`等等。

响应渲染函数的模块是视图，接下来我们会创建一个新视图。

##<strong><em>一个新视图</em></strong>

Phoenix的视图负责几个重要的工作。它们渲染模板。它们作为数据的表现层，为模板渲染数据做准备。相关的函数应该被放置在视图模块里面。

举个例子，假设我们有这样一个数据结构，它表示一个用户，有两个字段一个是`first_name`另一个是`last_name`。在模板里面我们要显示用户的全名。我们可以通过在模板里面嵌入代码来完成名字的结合，但更好的选择是在视图里面写一个函数，然后在模板里面调用它。这样模板更清晰也更合理。

为了`HelloController`能够调用函数渲染任意模板，我们需要一个`HelloView`。在这里名字很重要——视图和控制器的首部分必须一致。在这里先创建一个空视图，接着再进行详细描述。创建一个文件`web/views/hello_view.ex`，它的内容如下：
```
defmodule HelloPhoenix.HelloView do
  use HelloPhoenix.Web, :view
end
```
##<strong><em>一个新模板</em></strong>

Phoenix模板能够渲染数据。Phoenix的标准模板引擎是eex，它以[Embedded Elixir](http://elixir-lang.org/docs/stable/eex/http://elixir-lang.org/docs/stable/eex/)为基础。我们所有的模板文件都会有`.eex`的扩展名。

模板在视图里面，视图又在控制器里面。事实上，这意味着我们在目录`web/templates`下创建了一个以控制器名字命名的目录。在这个应用下，它意味着我们需要在`web/templates`目录下创建一个目录`hello`，并在里面创建一个`index.html.eex`文件。

现在就创建一个文件`web/templates/hello/index.html.eex`，代码如下：
```
<div class="jumbotron">
  <h2>Hello World, from Phoenix!</h2>
</div>
```
现在，我们已经搞定了路由，控制器，视图和模板，我们应该能在浏览器的[http://localhost:4000/hello](http://localhost:4000/hello)中看到Hello World, from Phoenix!（如果你关闭了服务器，重新开启的命令是`$ mix phoenix.server`）。

![phoenix welcome pages](https://www.filepicker.io/api/file/TLBRJ5LBR9mK9Fkcqwea)

我们刚才做的事情有几个比较值得注意。当我们改变代码时，并不需要重新启动服务器就可以在浏览器中看到变化。是的，Phoenix支持热更新！同时，尽管`index.html.eex`只由一个简单div标签构成，但最后我们会得到一个完整的html页面。我们的首页会被渲染进应用输出页面`web/templates/layout/app.html.eex`。（译者注：也就是模板继承）。在那个页面里你如果看到`<%= @inner %>`，在生成html页面传给浏览器前它会被`index.html.eex`的内容替换掉。

# 额外的新页面

让我们添加一个稍微复杂点的新页面。我们将添加一个新页面，它较之于前面的页面URL后有额外部分“messenger”，并将其作为参数传给控制器再传递给模板，以便于我们的messenger能向我们打招呼。就像我们前面做的一样，我们先创建一个路由。


##<strong><em>一个新路由</em></strong>

对于这个练习，我们将复用刚刚创建的`HelloController`，并添加新的`show`行为函数。我们在最后一个路由下添加一行代码，如下：
```
scope "/", HelloPhoenix do
  pipe_through :browser # Use the default browser stack.

  get "/", PageController, :index
  get "/hello", HelloController, :index
  get "/hello/:messenger", HelloController, :show
end
```
注意我们把原子`:messenger`放在路径里。Phoenix将会把URL里面这个位置的值与key`messenger`组成一个[字典（Dict）](http://elixir-lang.org/docs/stable/elixir/Dict.html)传给控制器。

举个例子，如果我们在浏览器中打开[http://localhost:4000/hello/Frank](http://localhost:4000/hello/Frank)，那么`:messenger`的值就是`Frank`。

##<strong><em>一个新动作</em></strong>

对于发送到我们定义的路由的请求，它们会被`HelloPhoenix.HelloController`的`show`行为函数处理。我们已经有了控制器，也就是`web/controllers/hello_controller.ex`，因此我们需要把`show`函数加进去。这次，我们需要封装一个map传给行为函数，从而我们能把messenger传给模板。为此，我们在控制器里面定义show函数。
```
def show(conn, %{"messenger" => messenger}) do
  render conn, "show.html", messenger: messenger
end
```
这里有很多事情要注意。我们对传给show函数的参数进行了模式匹配，从而变量`messenger`能够与URL里面`:messenger`的值绑定起来。举个例子，如果我们的URL是[http://localhost:4000/hello/Frank](http://localhost:4000/hello/Frank)，对应的变量会与`Frank`绑定起来。

在`show`函数里面，我们传给render函数第三个参数，一个键值对，其中`:messenger`为键，变量messenger就是要传递的值。

注意：如果函数体需要整个map的权限来进行变量绑定，我们可以这样定义`show/2`函数：
```
def show(conn, %{"messenger" => messenger} = params) do
  ...
end
```
最好记住[字典（Dict）](http://elixir-lang.org/docs/stable/elixir/Dict.html)的键总是字符串，还有`=`不意味着赋值，而是[模式匹配](http://elixir-lang.org/getting-started/pattern-matching.html)。

##<strong><em>一个新模板</em></strong>

对于问题的最后一部分，我们需要一个新的模板。由于这个模板的建立是为了`HelloController`的`show`函数，因此它将会被放在目录`web/templates/hello`下，命名为`show.html.eex`。它看起来就像前面的`index.html.eex`，除了显示的内容有所不同。

因此，我们将会用到eex引擎的标签——<%= %>，用来执行Elixir代码。注意开始标签有一个`=`就像`<%=`。那意味着标签里面的任何Elixir代码都会被执行，并且执行结果会替换掉标签。如果开始标签没有`=`，代码依旧会被执行，但是结果不会显示在页面上。

这个模板的代码应该跟下面一样。
```
<div class="jumbotron">
  <h2>Hello World, from <%= @messenger %>!</h2>
</div>
```
我们的messenger就是`@messenger`的值。在这种情况下，它不是模块属性。它只是用元编程实现的一个语法，它意味着`Dict.get(assigns, :messenger)`。它看起来更美观也更好用。

搞定啦。如果你在浏览器中打开[http://localhost:4000/hello/Frank](http://localhost:4000/hello/Frank)。你应该能看到下面的页面。

![new page](https://www.filepicker.io/api/file/jTxIw39Sma27wHr6z14g)

可以多试几次。你放在`/hello/will`后面的东西都会作为`messenger`显示在页面上。