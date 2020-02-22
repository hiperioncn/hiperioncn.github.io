本文是基于已经完成了Freeline的相关配置基础上的相关使用介绍：
需要配置的同学请看这里：[配置过程](http://blog.csdn.net/ocean20/article/details/53215304)


----------

开始使用Freeline
------------

点击工具栏的freeline按钮即可将apk编译到手机上，
![freeline 按钮](https://ooo.0o0.ooo/2016/11/17/582db94eaf31c.jpeg)
初次使用Freeline时会进行全量编译，时间稍长。完成编译后在androidstuido中将会出现freeline窗口
![这里写图片描述](https://ooo.0o0.ooo/2016/11/18/582eca7c17e09.jpeg)

窗口左边共有5个按钮，从上到下按个说一下。
1. 每次编译时就点这个按钮就可以增量编译了，对应命令是：python freeline.py
2. 停止freeline编译
3. 进行调试编译 对应命令是： python freeline.py -d 
4. 全量编译 对应命令是： python freeline.py -f
5. 清屏按钮

现在再说下python命令对应可选参数的说明：
python freeline.py -d  
可选参数:
-h, --帮助 显示帮助信息并退出
-v, --版本 显示版本信息
-f, --cleanBuild 强制执行一次 clean build
-w, --等待 让应用程序等待 debugger
-a, --全部  在所有工程上强制执行clean build 并执行-f全量编译
-c, --清空 清空缓存目录和工作空间
-d, --调试 打开debug模式
-i, --初始化 对工程进行进行freeline初始化配置