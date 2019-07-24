# 编辑页面模板

这个教程将会介绍`apostrophe-pages`和`apostrophe-templates`两个模块。它将涵盖了在Nunjucks编辑页面模板的基础和向你展示如何在Home 页面添加一个hero部分。

#### 关于app.js

app.js是apostrophe的主要配置文件。

#### `lib/modules`: Apostrophe的模块

Apostrophe是一个模块化的内容管理系统。每个有意义的组件被分解成自己的模块，然后可以与系统中的其他模块交互或子类化（扩展）。在底层，模块是由moog和moog-require提供支持。

项目中的`lib/modules`文件夹是为你自己项目创建的模块所在位置。也是可以"隐式子类化"（即配置或改进）apostrophe自己模块的地方，无论是apostrophe npm模块核心的一部分还是打包在单独的npm模块中。

在test-project中的`lib/modules`中扩展了两个模块:
`apostrophe-assets`/`apostrophe-pages`。
`apostrophe-assets`获取一些自定义的less css文件，`apostrophe-pages`则包含页面模板。


*apostrophe模块和npm模块不是一样的*。一个npm模块可以打包几个apostrophe模块，这些模块作为包（bundle）来维护。当你安装`apostrophe-blog`包时你会看出区别。


#### 添加一个新的页面模板

添加一个`default`模板到一个新页面上。

第一步是在app.js添加页面模块配置：

```
// 配置我们默认的页面模板
'apostrophe-page': {
  types: [
    {
      name: 'default',
      label: 'Default'
    },
    {
      name: 'home',
      label: 'Home'
    }
  ]
}
```

> 如果你不使用nodemon,那么在进行大多数代码更改时，需要重启node程序。在终端中，按'ctrl+c'，然后启动：node app.js

这是因为每个节点应用程序本身是一个web服务器。与php程序不同，节点应用程序无限期地"存活(stay alive)", 异步处理许多页面请求以获得更好的性能。在生产中，我们将使用更健壮的web服务器作为"前端代理(frantend proxy)"来帮助处理负载。

由`apostrophe-cli`生成的项目提供了一个合理的nodemon配置。


#### 创建和扩展页面模板

我们已经为站点配置了所需的两种页面类型。但是，如果你现在尝试通过"页面菜单(page Menu)"(左下角)添加一个新页面，并将其设置为"默认(default)"页面类型，将会得到一个错误。

apostrophe查找模板文件`lib/modules/apostrophe-pages/views/pages/default.html`,因此打开编辑器并创建该文件。

如何向页面添加内容。

`default.html`文件默认为空，可以通过扩展布吉模板免费获取更多。

Nunjucks允许扩展另一个模板，并具有覆盖`block`去更新或更改模板的选项。比如：

```
{% extends "layout.html" %}
```

从apostrophe样板文件项目中使用cli创建的吸纳灌木在顶级`view/`文件夹中提供了一个简单的`layout.html`文件，如果模板不在特定模块中，则可以在该文件中找到模板。

```
{% block beforeMain %}
  {#
    We recommend you use the beforeMain block for global page components
    like headers or navigation.
  #}
{% endblock %}

{% block main %}
  {#
    通常，apostrophe-pages模块中的页面模板将覆盖
    这一块。可以安全地假设这是页面特定内容所在的位置
    应该去。
  #}
{% endblock %}

{% block afterMain %}
  {#
    这将是放置全局页脚的好地方
  #}
{% endblock %}

```

可以使用`block`关键字在页面模板中覆盖其中定义的。

在页面上使用这些blocks意味着页面模板输出的内容都将插入到布局的适当位置。

还有一个title block ，你可以扩展它来设置页面标题。

> 甚至layout.html也扩展了另一个文件。对于典型的页面加载，它扩展outerLayout.html,该文件位于lib/modules/apostrophe-templates/views文件夹中。该文件扩展带apostrophe 的outerLayoutBase.html文件。大多数你不需要看那里，但是它确实包含了你可以覆盖的附加块，特别是extraHead,它非常适合向head元素添加链接元素，等等。


#### 使用blocks覆盖布局的部分

现在我们已经在默认情况下扩展了layout.html。我们可以覆盖`beforeMain`模块部分。

```
{% block beforeMain %}
  <div class="block hero">
    <div class="inner">
      <h3>welcome</h3>
    </div>
  </div>
{% endblock %}
```
因为我们扩展了布局，所以可以避免重复很多标记。


#### 包括从许多模块导入和扩展文件

有时，你需要编写numjucks宏、包含文件或可扩展的布局，这些布局由许多不同的apostrophe模块使用。为方便起见，你可以将这些全局模板放在项目的顶级views/文件夹中。在当前模块中未找到任何模板都将作为回退定位在哪里。在较旧的项目中，除非启用该模板，否则无法工作、请参考apostrophe样板项目中的app.js。



















