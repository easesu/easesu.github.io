---
title: 解决Microsoft Onenote剪辑微信文章时图片挂掉的问题
---

# 解决Microsoft Onenote剪辑微信文章时图片挂掉的问题

## 问题描述

当使用Onenote浏览器插件剪辑微信文章之后，再使用Onenote软件打开剪辑后的内容，经常会发现图片裂了，无法正常打开，再怎么重复剪辑都一样，即使单独复制图片进去也是无法正常查看。

## 原因

竟然是因为Onenote不支持webp格式的图片！

    WebP是Google推出的影像技术，它可让网页图档有效进行压缩，同时又不影响图片格式兼容与实际清晰度，进而让整体网页下载速度加快。

现在越来越多的网站会根据用户使用的浏览器自动适配图片格式，以提升网站打开速度，比如微信的公众号文章。

只可惜Onenote剪辑不支持webp图片。

## 解决办法

微信文章里面的图片会优先使用webp格式，但也会向下兼容png格式，只需要将图片地址中的tp参数值从webp改为png即可。

复制以下代码，保存为浏览器书签，在微信文章点击保存好的书签，即可将webp替换png图片。

```javascript
javascript:document.querySelectorAll('img').forEach(img => {if(img.src.indexOf('tp=webp') !== -1){img.src=img.src.replace('tp=webp','tp=png')}});
```
