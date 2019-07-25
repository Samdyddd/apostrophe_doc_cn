# 推送资源到浏览器
------------------------

## 配置样式表

`lib/modules/apostrophe-assets/index.js`配置静态文件，包括`site.less`和`site.js`

```
// 以下配置是apostrophe-assets模块推送'site.less'

module.exports = {
  stylesheets: [
    {
      name: 'site'
    }
  ],
  scripts: [
    {
      name: 'site'
    }
  ]
};

```

我们本可以将此配置防止在`app.js`中，但这会导致app.js文件混乱。apostrophe会自动在`lib/modules/MODULE-N AME-HERE/index.js`文件，如果存在则加载。

```
// lib/modules/apostrophe-assets/public/css/site.less

@import "utils/reset.less";
body{
  .karla;
  background-color: @apos-light
}

.main-content{
  margin: 200px auto;
  color: @apos-white;
  .apos-drop-shadow;
}
```

> 

## 在不减少编译的情况下推送css样式表

当你正在使用自己的基于web包设置构建自己的css文件。
有时你的css可能包含一些还不能接受的内容。
可以按以下使用导入标记来解决这个问题：

```
// lib/modules/apostrophe-assets/index.js

module.exports = {
  stylesheets: [
    {
      name: 'site',
      import: {
        inline: true
      }
    }
  ]
}
```
当你使用`inlne`时，less编译器会按照原样导入文件，而不是将它当less来解析。


## 为浏览器配置javascript

将js文件推送到浏览器。

```
scripts: [
  {
    name: 'site'
  }
]
```
将`lib/modules/apostrophe-assets/public/js/site.js`推送到浏览器上。

> 使用`gulp`,`browserify`,`grunt`


### 包括web字体、图像和其他资源

* 主题模块中的资源 *

在进行包括css、javascript、web字体和图片资源

你的`my-theme`模块可能如下：
```
my-theme
  -public/
    -css/
    -fonts/
    -img/
    -js/
  -index.js
```

在`my-theme`模块中，在`lib/modules/my-theme/public/**`包含了所有资源。然后在`modules/my-theme/**`

例子，`karla.woff`文件在`lib/modules/my-theme/public/fonts`中、
```
@font-face{
  font-family: 'Karla';
  src: url('/modules/my-theme/fonts/karla.woff') format('woff');
}

```

## apostrophe-assets 中的资源

在使用apostrophe-assets和使用自定义模块中的资源相比是有差异的。













