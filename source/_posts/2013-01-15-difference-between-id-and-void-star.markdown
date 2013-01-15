---
layout: post
title: "Difference between id and void *"
date: 2013-01-15 15:35
comments: true
categories: Objective-C
---

This post is part of answer from [StatckOverflow](http://stackoverflow.com/questions/1304176/objective-c-difference-between-id-and-void)

**void *** means "a reference to some random chunk o' memory with untyped/unknown contents"

**id** means "a reference to some random Objective-C object of unknown class"

<!-- more -->

There are additional semantic differences:

* Under GC Only or GC Supported modes, the compiler will emit write barriers for references of type id, but not for type void *. When declaring structures, this can be a critical difference. Declaring iVars like void *_superPrivateDoNotTouch; will cause premature reaping of objects if _superPrivateDoNotTouch is actually an object. Don't do that.

* attempting to invoke a method on a reference of void * type will barf up a compiler warning.

* attempting to invoke a method on an id type will only warn if the method being called has not been declared in any of the @interface declarations seen by the compiler.

Thus, one should never refer to an object as a void *. Similarly, one should avoid using an id typed variable to refer to an object. Use the most specific class typed reference you can. Even NSObject * is better than id because the compiler can, at the least, provide better validation of method invocations against that reference.

The one common and valid use of void * is as an opaque data reference that is passed through some other API.

Consider the sortedArrayUsingFunction: context: method of NSArray:

	- (NSArray *)sortedArrayUsingFunction:(NSInteger (*)(id, id, void *))comparator context:(void *)context;
The sorting function would be declared as:

	NSInteger mySortFunc(id left, id right, void *context) { ...; }
In this case, the NSArray merely passes whatever you pass in as the context argument to the method through as the context argument. It is an opaque hunk of pointer sized data, as far as NSArray is concerned, and you are free to use it for whatever purpose you want.

Without a closure type feature in the language, this is the only way to carry along a hunk of data with a function. Example; if you wanted mySortFunc() to conditionally sort as case sensitive or case insensitive, while also still being thread-safe, you would pass the is-case-sensitive indicator in the context, likely casting on the way in and way out.

Fragile and error prone, but the only way.

Blocks solve this -- Blocks are closures for C. They are available in Clang -- http://llvm.org/ and are pervasive in [Snow Leopard](http://developer.apple.com/library/ios/documentation/Performance/Reference/GCD_libdispatch_Ref/GCD_libdispatch_Ref.pdf).