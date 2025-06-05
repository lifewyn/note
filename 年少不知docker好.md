---
time: 2025-05-27
link: https://www.dianyuan.com/people/805449?page=6#article
---


最近因工作需要，接触了docker，发现这个东西真的非常适合开发人员，值得玩一玩。

我们知道，做嵌入式软件的，经常需要使用交叉编译环境编译单片机或者linux程序，在window时，我们可能只要安装一个IDE就够了，但linux环境不同，需要安装各种软件，有的时候可能还需要自己编译交叉工具链，非常不方便。

安装软件的时候，可能对操作系统的版本也有要求，比如某个软件在18.04可以轻松安装，到了20.04你可能死活安装不了，即使安装了这一个，可能还会有其他软件需要同步安装，一堆坑等着你踩，比如gcc版本的坑，Python版本的坑。

但是用了docker就不一样了，只要有一个dockerfile文件（可能需要其它资源依赖），随时随地可以生成当时需要的开发环境，极大的节省了开发环境搭建的时间。如果别人已经生成了镜像，那更好，只要导入了docker镜像就能用，方便的很。

领导再也不担心我搭不好开发环境了。

![](https://eestar-public.oss-cn-shenzhen.aliyuncs.com/article/image/20240821/2024082110063166c54ba71871a.jpg?x-oss-process=image/watermark,g_center,image_YXJ0aWNsZS9wdWJsaWMvd2F0ZXJtYXJrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSxQXzQwCg==,t_20)

来源于网图

而使用docker的时间成本也比较低，只要在当前的虚拟机（不管你的虚拟机是哪个版本）中安装好docker就行了（最糟糕的情况也不过可能需要自己编译），常用的命令也就那么几条，配合 vscode使用非常丝滑。

而docker的工作原理是利用了宿主机的资源，而不是全部模拟，docker里面的进程也是以虚拟机里面的进程形式运行的，因此效率更高。

其它资源占用也和宿主机类似，比如cpu、ram等，不像虚拟机需要占用非常多资源。

一个虚拟机里面可以运行非常多的docker容器，比如Ubuntu16.04，Ubuntu18.04，Ubuntu20.04都可以，互不影响，资源消耗却少的可怜。

而docker仓库也有大量别人上传好的镜像可以直接用，比如nginx、eclipse、go、甚至还有win10镜像，并且这些镜像的功能还可以组合使用，相当方便。

还等什么？赶紧上！