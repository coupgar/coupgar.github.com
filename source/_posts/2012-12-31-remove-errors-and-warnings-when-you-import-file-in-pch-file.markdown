---
layout: post
title: "remove errors and warnings when you import file in .pch file"
date: 2012-12-31 15:02
comments: true
categories: iPhone, Cocoa
---
在xcode中频繁使用的类库的头文件可以放入pch文件中，这样gcc会在预编译的过程中将头文件导入到每个.h 文件中。虽然import比include更加智能，但是似乎xcode中还是会遇到预编译错误提示，也一直困扰我很久。
<!-- more -->
现在只需要在pch文件中导入的每个头文件中定义一个宏，然后在pch文件中ifndef判断一下就ok

例如在pch中有如下导入：
{% codeblock lang:objc %}
#imprort "MagicLibrary.h"
{% endcodeblock %}

现在在MagicLibrary.h中添加如下def
{% codeblock lang:objc %}
#define MAGIC_LIBRARY_DEF
{% endcodeblock %}

在pch中做如下修改：
{% codeblock lang:objc %}
#ifndef MAGIC_LIBRARY_DEF
    #imprort "MagicLibrary.h"
#endif
{% endcodeblock %}