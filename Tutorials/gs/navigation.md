# 建立导航栏

apostrophe 页面都是'tree'中的一部分，其中除主要外，每个页面都是另一个页面的子页面。在构建导航链接时变得简单。


## 链接到home 页面

```
<h2><a href="{{data.home._url}}">{{data.home.title}}</a></h2>

```
> 为什么是`_url`？url不是驻留在数据库中的page对象的永久部分。任何动态设置且不是数据库一部分的都以`_`开头。

## 链接到 home页面的子页面（'tab'导航）

许多网站在顶部都有一排“tabs”，显示正在那个页面上：

```
<ul class="tabs">
  {% for tab in data.home._children %}
    <li><a href="{{tab._url}}">{{tab.title}}</a></li>
  {% endfor %}
</ul>
```

添加一个css类表示当前页：

```
<ul class="tabs">
  {% for tab in data.home._children %}
    <li class="
      {% if data.page and
        (apos.page.isAncestorOf(tab, data.page) or tab._id== data.page._id) 
       %}
       current
       {% endif %}
    "
    ><a href="{{tab._url}}">{{tab.title}}</a></li>
  {% endfor %}
</ul>
```

## 下拉菜单

显示下拉菜单，每个菜单表示主页的一个子菜单，每个菜单上的每个项目表示该页面的一个子菜单

首先，在app.js中，在获取当前页面的祖先时，配置apostrophe-pages来检索两层子页面。

```
modules: {
  // ...
  'apostrophe-pages': {
    ancestors: {
      children: {
        depth: 2
      }
    },
    children: true
  }
}
```

我们很容易输出所有的标记，我们需要的下拉菜单：

```
<ul>
  {% for tab in data.home._children %}
    <li><a href="{{tab._url}}">{{tab.title}}</a>
    {% if tab._children.length %}
      <ul>
        {% for child in tab._children %}
        <li><a href="{{child._url}}">{{child.title}}</a></li>
        {% endfor %}
      </ul>
    {% endif %}
    
    </li>

  {% endfor %}
</ul>

```

## breadcrumb trails（面包屑）

当前页面是`data.page`,默认情况下`data.page._ancestors`是可用的。

```
{% if data.page %}
  <ul>
    {% for ancestor in data.apge._ancestors %}
      <li><a href="{{ancestor._url}}">{{ancestor.title}}</a></li>
    {% endfor %}
  </ul>
{% endif %}
```

> 在布局模板中使用data.page时，一定要检查它是否存在，改模版可以通过login.html、notfound.html和其他没有CMS 'page'的地方进行扩展。


## 手风琴导航

列出当前页面的祖先及其子页：

```
{%  if data.page %}
  <ul class="accordion">
    {% for ancestor in data.page._ancestors %}
      <li><a href="{{ancestor._url}}">{{ancestor.title}}</a>
        {% if ancestor._children.length %}
        <ul>
          {% for child in ancestor._children %}
            <li><a href="{{child._url}}">{{child.title}}</a></li>
          {% endfor %}
        </ul>
        {% endif %}
      </li>
    {% endfor %}
  </ul>
{% endif %}
```

## 当前页面子元素

```
{% if data.page %}
  <ul class="children">
    {% for child in data.page._children %}
      <li><a href="{{child._url}}">{{child.title}}</a></li>
    {% endfor %}
  </ul>

{% endif %}

```



