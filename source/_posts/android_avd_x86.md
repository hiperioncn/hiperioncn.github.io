title: Android x86模拟器安装apk时报错 INSTALL_FAILED_NO_MATCHING_ABIS
comments: true
permalink: hiperioncn.github.io
date: 2016-05-20 10:18:29
tags: 模拟器
categories: Android
---

AndroidStudio中的x86模拟器速度确实提高不少，但使用中也遇到一些问题。
目前开发的这个App使用了一个只兼容arm的so库，无法正常安装到模拟器上。

安装时会出现 INSTALL_FAILED_NO_MATCHING_ABIS 的错误，这是由于使用了native libraries 。该native libraries 不支持当前的cpu的体系结构。我当前这个问题就是native libraries 不支持x86架构的cpu。
目前安卓模拟器的CPU/ABI一般有三种类型，INTEL X86,ARM,MIPS

解决方法：
可以再build.gradle中的android{}中加入如下内容，

```
splits {
        abi {
            enable true
            reset()
            include 'x86', 'armeabi-v7a'
            universalApk false
        }
    }

```

这样就可以正常安装到x86架构的模拟器上。
只限于调试界面在不兼容的模拟器上，不建议调试native libraries部分的代码。
