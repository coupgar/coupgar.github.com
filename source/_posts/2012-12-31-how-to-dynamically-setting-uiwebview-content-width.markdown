---
layout: post
title: "how to dynamically setting uiwebview content width with javascript"
date: 2012-12-31 15:31
comments: true
categories: iPhone, Cocoa, UIWebView, JavaScript, Objective-C
---
在加载uiwebview时，uiwebview会根据网页中设定的width来layout内容，如果网页中的width设定不正确，就不能正确layout网页。例如渣浪微博的登录页面是设置的device width，iPhone上一般没有问题，但在ipad中的view就显示不正确。

<!-- more -->

解决这个问题很简单，只需要在uiwebview加载的html中修改width就OK了。按照这个思路，找到uiwebview的delegate回调：
{% codeblock lang:objc %}
- (void)webViewDidFinishLoad:(UIWebView \*)webView
{
 	NSString* js =[NSString stringWithFormat:
                   @"var meta = document.createElement('meta'); " \
                   "meta.setAttribute( 'name', 'viewport' ); " \
                   "meta.setAttribute( 'content', 'width = %f, initial-scale = 5.0, user-scalable = yes' ); " \
                   "document.getElementsByTagName('head')[0].appendChild(meta)",
                   self.loginWebView.bounds.size.width];
    [webView stringByEvaluatingJavaScriptFromString:js];
    
    //web view should looks great now.
}
{% endcodeblock %} 
  
  PS: JS和webview是可以互调的，详情可以参考这个[库](https://github.com/marcuswestin/WebViewJavascriptBridge)
  
  
  
