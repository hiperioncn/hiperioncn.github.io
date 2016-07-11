title: Android 使用Glide时如何进行log打印
comments: true
permalink: hiperioncn.github.io
date: 2016-06-03 13:48:29
tags: Glide
categories: Android
---

Glide图片加载框架是Google官方推荐使用的。
默认情况下Glide是不打印log的。今天在调试一个demo时Glide无法正确载入图片，也不报错。
后来添加了Glide的log日志打印后发现自己犯了一个低级错误，由于是新创建的一个demo工程，未加入internet权限。

Glide添加日志打印的方法：

先实现一个RequestListener
```
public class LoggingListener<T, R> implements RequestListener<T, R> {
    @Override public boolean onException(Exception e, Object model, Target target, boolean isFirstResource) {
        android.util.Log.d("GLIDE", String.format(Locale.ROOT,
                "onException(%s, %s, %s, %s)", e, model, target, isFirstResource), e);
        return false;
    }
    @Override public boolean onResourceReady(Object resource, Object model, Target target, boolean isFromMemoryCache, boolean isFirstResource) {
        android.util.Log.d("GLIDE", String.format(Locale.ROOT,
                "onResourceReady(%s, %s, %s, %s, %s)", resource, model, target, isFromMemoryCache, isFirstResource));
        return false;
    }
}
```

然后在Glide加载图片时加入该listener
```
Glide.with(this)
                .load("http://inthecheesefactory.com/uploads/source/glidepicasso/cover.jpg")
                .listener(new LoggingListener<String, GlideDrawable>())
                .into(imageView);
```
