title: Android 秒级编译框架配置过程
comments: true
permalink: hiperioncn.github.io
date: 2016-11-17 18:48:29
tags: freeline
categories: Android
---

Freeline是蚂蚁金服旗下一站式理财平台蚂蚁聚宝团队在Android平台上的量身定做的一个基于动态替换的编译方案，稳定性方面：完善的基线对齐，进程级别异常隔离机制。性能方面：内部采用了类似Facebook的开源工具buck的多工程多任务并发思想, 并对代码及资源编译流程做了深入的性能优化。
具体原理请查看：https://yq.aliyun.com/articles/59122?spm=5176.8091938.0.0.1Bw3mU

github: https://github.com/alibaba/freeline

配置步骤：

###### 1.配置project-level的build.gradle，加入freeline-gradle的依赖：

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.antfortune.freeline:gradle:0.8.2'
    }
}
```
###### 2. 在你的主module的build.gradle中，应用freeline插件的依赖：

```
apply plugin: 'com.antfortune.freeline'

android {
    ...
}
```
###### 3. 在命令行执行以下命令来下载 freeline 的 python 和二进制依赖。

```
Windows[CMD]: gradlew initFreeline
Linux/Mac: ./gradlew initFreeline
```
对于国内的用户来说，如果你的下载的时候速度很慢，你也可以加上参数，执行gradlew initFreeline -Pmirror，这样就会从国内镜像地址来下载。

你也可以使用参数-PfreelineVersion={your-specific-version}来下载特定版本的 freeline 依赖。

如果你的工程结构较为复杂，在第一次使用freeline编译的时候报错了的话，你可以添加一些freeline提供的配置项，来适配你的工程。具体可以看Freeline DSL References。https://github.com/alibaba/freeline/wiki/Freeline-DSL-References

###### 4.安装Freeline插件，在Android Studio中，通过以下路径Preferences → Plugins → Browse repositories，搜索“freeline”，并安装. 安装好后会在工具栏出现如下图所示按钮
![](https://ooo.0o0.ooo/2016/11/17/582db94eaf31c.jpeg
)

###### 5.在工程根目录下执行一次全量编译 
```
python freeline.py -f
```
！！！！！！！！！这步很重要，如果不执行，否则当你点击Freeline按钮时会报错,错误内容如下：

```
Traceback (most recent call last):
  File "freeline.py", line 49, in <module>
    main()
  File "freeline.py", line 45, in main
    freeline.call(args=args)
  File "freeline.py", line 20, in call
    self.dispatcher.call_command(args)
  File "/Users/hiperion/AndroidStudioProjects/Bloomsky/freeline/freeline_core/dispatcher.py", line 57, in call_command
    self._exec_command(self._command)
  File "/Users/hiperion/AndroidStudioProjects/Bloomsky/freeline/freeline_core/dispatcher.py", line 100, in _exec_command
    '[WARNING] some important file missed, a clean build will be automatically executed.')
  File "/Users/hiperion/AndroidStudioProjects/Bloomsky/freeline/freeline_core/dispatcher.py", line 125, in _retry_clean_build
    self._setup_clean_build_command(is_build_all_projects=False)
TypeError: _setup_clean_build_command() takes exactly 3 arguments (2 given)
```
我已经给官方提了issue，很快得到了响应，正在修复中
[issue详细请点击这里](https://github.com/alibaba/freeline/issues/308)
###### 6.现在就可以点击工具栏安装好的插件进行秒级增量编译了。

Enjoy!
