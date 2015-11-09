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