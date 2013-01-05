---
layout: post
title: "how to replace window.close method func by javascript in UIWebview"
date: 2013-01-05 13:33
comments: true
categories: javascript, UIWebView,iPhone,iOS,Cocoa
---

iOS中的UIWebView不能响应window.close方法，如果碰到web页面中调用了这个方法，如何去截获这个方法呢？
<!-- more -->

首先想到的方法是修改按钮的方法，但是这个方法不具备普遍性，万一web页面升级，那么这个方法就失效了。

那么能不能重写window.close方法呢？答案是可以的。具体如下：

{% gist 4459930 gistfile1.m %}

可以看到，给window.close重写了一个方法，并在其中访问了一个特殊的url。接下来在webview的回调中判断访问的url。

{% gist 4459930 gistfile2.m %}

至此，我们就完成了window.close方法的重写。

