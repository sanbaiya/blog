---
title: 解决butterfly主题部署全局吸底alayer播放器不显示的问题
abbrlink: 2f1db370
data: 2024-12-15 15:33:33
description: '排查播放器不显示问题'
---
## 问题阐述
您是否遇到过在安装完aplayer插件后全局吸底播放器不显示的问题,在浏览器控制台看到这样的报错
![F12控制台报错页面](https://s2.loli.net/2024/12/15/ghqvAQjFMc4Jxyp.jpg)
## 排查问题
解决这种报错很简单
### 关闭asset_inject
打开博客配置文件查看是否添加了
```YAML
aplayer:
  meting: true
  asset_inject: false
```
### 开启主题aplayerinject
在主题的配置文件中，enable设为true和per_page设为true
```YAML
# Inject the css and script (aplayer/meting)
aplayerInject:
  enable: true
  per_page: true
```
### 插入Aplayer html
```MarkDown
# aplayer音乐
    - <div class="aplayer no-destroy" data-id="12368843344" data-server="netease" data-type="playlist"   data-order="list" data-fixed="true" data-preload="auto" data-autoplay="false" data-mutex="true" ></div>
```
data-id,data-server和data-type是必配置项，其他的可选
把aplayer代码插入到主题配置文件的inject.bottom中去
```YAML
inject:
  head:
  bottom:
    # aplayer音乐
    - <div class="aplayer no-destroy" data-id="12368843344" data-server="netease" data-type="playlist"   data-order="list" data-fixed="true" data-preload="auto" data-autoplay="false" data-mutex="true" ></div>
```
### 插入CDN
如果按照以上步骤都还是无法显示的情况下那就是缺少CDN
在主题配置文件中的CDN.option中找到
```YAML
# aplayer_css:
# aplayer_js:
# meting_js:
```
并替换成
```YAML
aplayer_css: https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css
aplayer_js: https://cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js
meting_js: https://cdn.jsdelivr.net/gh/metowolf/MetingJS@1.2/dist/Meting.min.js
```
hexo cl和hexo g和hexo s后看看左下角是否出现了播放器