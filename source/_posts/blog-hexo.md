---
title: Hexo搭建博客常见问题
date: 2018-05-09
categories:
  - IT
toc: true
copyright: true
abbrlink: 5b526bb
---

# 基本命令

备注下：
hexo new "postName" #新建文章
hexo new draft "new draft" #新建草稿
(config.xml render_drafts 参数设为 true 预览草稿，或者hexo server --drafts)
hexo new page "pageName" #新建页面
hexo clean
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

hexo publish [layout] <filename> #草稿变成文章

# 流程图插件

[hexo-filter-flowchart](https://github.com/bubkoo/hexo-filter-flowchart)
[hexo-filter-sequence](https://github.com/bubkoo/hexo-filter-sequence)


# 404页面

可以在主题对应source文件夹下建立404.html

# 版权信息配置

`layout\_partial\post\ `目录下新建 `copyright.ejs` 内如如下：

``` html
<div class="article-footer-copyright">
  <div>本文作者：IT姑凉</div>
  <div>本文链接：<a href="<%- url_for(post.permalink) %>"><%= post.permalink %></a></div>
  <div>本文首发于 <a href="https://itguliang.github.io">IT姑凉博客</a> ，如需转载请注明出处，谢谢！</div>
</div>
```

`layout\_partial\article.ejs `文件下合适位置添加如下代码：

``` javascript
<% if (!index && post.copyright==true){ %>
  <%- partial('post/copyright') %>
<% } %>
```

样式自己对应修改

最后，只要在需要添加版权信息的文章中，添加 `copyright: true` 即可显示版权信息。


# 反垃圾链接

安装插件 `hexo-autonofollow`：

``` bash
npm install hexo-autonofollow --save
```

`_config.xml`中添加如下字段：

``` xml
nofollow:
    enable: true
    exclude:
    - 友链地址  
```

# 永久链接

``` bash
npm install hexo-abbrlink --save
```

配置：

``` javascript
permalink: post/:abbrlink.html
abbrlink:
  alg: crc32  // 算法：crc16(default) and crc32
  rep: hex    // 进制：dec(default) and hex
```

样例：
crc16 & hex
posts/66c8.html
crc16 & dec
posts/65535.html
crc32 & hex
posts/8ddf18fb.html
crc32 & dec
posts/1690090958.html


# 添加友情链接

在根目录`_config.yml`文件，添加链接内容如下：

``` yml
links:
   IT姑凉's Blog: https://itguliang.github.io
```

添加主题布局文件

将友情链接放到右侧的sidebar中，于是需要在主题目录下的`layout/_widget`中添加文件`links.ejs`，内容如下：

``` javascript
<% if (theme.links){ %>
  <div class="widget-wrap">
    <h3 class="widget-title"><%= __('友链') %></h3>
    <div class="widget">
      <ul class="entry">
        <% for (var i in theme.links){ %>
          <li class='link'>
            <a target="_blank" href='<%- theme.links[i] %>'><%= i %></a>
          </li>
        <% } %>
      </ul>
    </div>
  </div>
<% } %>
```

在对应的主题目录下的`_config.yml`文件中widgets下添加上links

``` xml
widgets:
- links
```
# 图床

七牛云+[hexo-qiniu-sync](https://github.com/gyk001/hexo-qiniu-sync)

# 其他功能

## 打赏

自己添加

## 评论

Valine 是一款基于Leancloud的快速、简洁且高效的无后端评论系统。

地址：https://valine.js.org/

## 统计

网上查了一下，用不蒜子，不过暂时好像不开放注册，先备注下，后期加上
http://busuanzi.ibruce.info/
暂时先用leancloud

## 返回顶部

界面：
```html
<div class="back-to-top" id="back-to-top">
  <i class="fa fa-arrow-up"></i>
</div>
```
js:
```javascript
$(function () {
  var backButton=$('#back-to-top');
  function backToTop() {
    $('html,body').animate({
      scrollTop: 0
    }, 800);
  }
  backButton.on('click', backToTop);
        
  $(window).on('scroll', function () {
    if($(window).scrollTop() > $(window).height())
      backButton.fadeIn();
    else
      backButton.fadeOut();
  });
});
```

## 只需改变源码repo自动deploy(未完成)

https://formulahendry.github.io/2016/12/04/hexo-ci/#

参考：
https://segmentfault.com/a/1190000006057336
http://www.ituring.com.cn/article/199295
https://hexo.io/zh-cn/docs/helpers.html
http://linbinghe.com/2017/982b12f0.html

