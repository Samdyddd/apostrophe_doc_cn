# 全局文档： 跨页面共享内容
-----------------------------

静态版权声明并不能很好解决这个问题。你的用户希望编辑自己的全局页脚。

像以下那样做：
```
{% block main %}
  <div class="main-content">
    {{
      apos.singleton(data.global, 'footer', 'aposotrophe-rich', {
        toolbar: ['Bold', 'Italic', 'Styles', 'Link', 'Unlink']
      })
    }}
  </div>

{% endblock %}
```

就像`data.page`,`data.global`可以包含apostrophe区域和单字符。
不像`data.page`，它总是引用相同的共享文档。因此，它是理想的页脚横幅或其他全站共享的内容元素。