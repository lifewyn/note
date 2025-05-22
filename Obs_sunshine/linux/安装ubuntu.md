---
time: 2025-03-24
tags:
  - C
---
[VirtualBox](https://www.debugpoint.com/tag/virtualbox) 是 Oracle 的一款流行的虚拟化软件，可用于 Linux、mac 和 Windows 系统。它是灵活的，并提供了许多功能来实现虚拟化。这是在 Windows 中体验 Ubuntu 而不安装它的最佳且简单的方法。然而，我强烈建议将 Ubuntu 以双引导的方式安装在物理机上，从而更好地体验 Ubuntu。

下面列出的步骤假设你是第一次在 Windows 中安装 Ubuntu。因此，这些步骤有点描述性，也有点冗长。此外，以下步骤适用于 Windows 10 和 Windows 11 作为宿主机。

### 你需要什么

- 可上网的 PC
    
- 用于安装的 Ubuntu Linux ISO 镜像文件
    
- 安装了 VirtualBox 的 Windows 系统
    

### 使用 VirtualBox 在 Windows 上安装 Ubuntu

#### 下载并安装必要的东西

从以下链接下载 Ubuntu Linux 桌面版 ISO 镜像文件。

> **[下载 Ubuntu 桌面版](https://ubuntu.com/download/desktop)**

此外，请从下面的官方网站下载 Oracle VirtualBox 安装程序。

> **[下载 VirtualBox](https://www.virtualbox.org/wiki/Downloads)**

![https://img.linux.net.cn/data/attachment/album/202301/23/230544n8twuigir6zf65g8.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230544n8twuigir6zf65g8.jpg)

_VirtualBox for Windows 的下载位置_

#### 如何安装和配置 VirtualBox

Windows 中的 VirtualBox 需要 “Microsoft Visual C++ 2019 Redistrobutiable package”。你必须先安装它。从以下链接下载软件包（X64 架构）：

> **[下载 MSVC](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)**

![https://img.linux.net.cn/data/attachment/album/202301/23/230553kwvbtt3vyza1ayk3.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230553kwvbtt3vyza1ayk3.jpg)

_下载 VirtualBox 的依赖项_

![https://img.linux.net.cn/data/attachment/album/202301/23/230602s9xnn75cd52v17oz.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230602s9xnn75cd52v17oz.jpg)

_安装 VirtualBox 的依赖项_

完成以上安装后，从以下链接下载最新的 Python 包。Python 绑定也是 Windows 端 VirtualBox 安装所需的依赖项。

> **[下载 Python for Windows](https://www.python.org/downloads/windows/)**

然后，启动 VirtualBox 安装程序并按照屏幕上的说明进行安装。

安装后，重新启动 Windows 系统。

#### 为 Ubuntu 设置虚拟机

从开始菜单启动 VirtualBox。

![https://img.linux.net.cn/data/attachment/album/202301/23/230613sfnsrjqgunuzbofy.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230613sfnsrjqgunuzbofy.jpg)

_从开始菜单中选择 VirtualBox_

在 VirtualBox 窗口工具栏上，单击 “新建New”。

![https://img.linux.net.cn/data/attachment/album/202301/23/230625sd4m9pplq0ycqaal.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230625sd4m9pplq0ycqaal.jpg)

_单击新建_

- 在创建虚拟机窗口中，输入虚拟机的名称。它可以是标识此版本 Ubuntu 的任何名称。
    
- 保持 “文件夹Folder” 不变。这是创建虚拟机文件的路径。
    
- 在 “ISO 镜像文件ISO Image” 一栏，浏览你下载的 Ubuntu ISO 文件。
    
- 然后选择 “跳过无人值守安装Skip Unattended installation”。如果不选择此选项，将在虚拟机中创建一个 [默认用户 id（vboxuser）和密码](https://www.debugpoint.com/virtualbox-id-password/)。让我们暂时不要管它。
    

![https://img.linux.net.cn/data/attachment/album/202301/23/230645pf7zv8t07l5c5t5h.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230645pf7zv8t07l5c5t5h.jpg)

_选择 ISO 文件_

- 单击 “硬件Hardware” 部分，并调整虚拟机所需的内存。一般的经验是，虚拟机的内存大小应该小于主机系统中的物理内存。我建议对于 8 GB 内存系统的虚拟机使用 2 GB 到 4 GB。要选择 4 GB 内存，拖动滑块（或键入）使其为 4096 MB（即 4×1024）。
    
- 选择 2 或 4 核处理器。
    

![https://img.linux.net.cn/data/attachment/album/202301/23/230654sk00u2jd7d7x7v7z.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230654sk00u2jd7d7x7v7z.jpg)

_选择硬件_

- 单击 “硬盘Hard Disk” 部分，并保持文件位置不变。
    
- 为 Ubuntu 安装提供至少 20 GB 到 25 GB 的容量。
    
- 硬盘文件类型值保持为 VDI（VirtualBox 磁盘镜像）
    
- 不要选择 “预分配完整大小Pre-allocate Full Size”。
    
- 最后，单击 “完成Finish”。
    

![https://img.linux.net.cn/data/attachment/album/202301/23/230703go8ot5pip2pccc1z.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230703go8ot5pip2pccc1z.jpg)

_选择硬盘_

你应该在 VirtualBox 的左侧面板上看到一个新条目，其中包含一个 Ubuntu 22.04 条目（你之前设置的名称）。

选择条目并单击 “开始Start” 以引导到虚拟机：

![https://img.linux.net.cn/data/attachment/album/202301/23/230713l3ae8385azn3n55f.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230713l3ae8385azn3n55f.jpg)

_在 VirtualBox 中启动 Ubuntu_

#### 使用 VirtualBox 安装 Ubuntu

成功引导后，你应该看到以下屏幕，其中显示了安装 Ubuntu 的各种选项。选择 “尝试 UbuntuTry Ubuntu” 或 “安装 UbuntuInstall Ubuntu”。

在欢迎屏幕中，单击 “尝试 UbuntuTry Ubuntu”。过了一会儿，你会看到下面的 Ubuntu 临场Live桌面。如果要更改分辨率，请右键单击桌面并选择显示设置。并将分辨率更改为 1400×900。

![https://img.linux.net.cn/data/attachment/album/202301/23/230822oaq6gqlxxglxegae.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230822oaq6gqlxxglxegae.jpg)

_选择尝试 Ubuntu_

在桌面上，双击 “安装 UbuntuInstall Ubuntu”。

![https://img.linux.net.cn/data/attachment/album/202301/23/230829azltkvkg5iph66nh.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230829azltkvkg5iph66nh.jpg)

_Ubuntu LIVE 桌面_

在下一组屏幕中，根据需要选择 “语言Language” 和 “键盘布局Keyboard Layout”。

![https://img.linux.net.cn/data/attachment/album/202301/23/230836ni44c9wni2aq6w9k.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230836ni44c9wni2aq6w9k.jpg)

_选择语言_

![https://img.linux.net.cn/data/attachment/album/202301/23/230859p6sytmreh3j1t56v.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230859p6sytmreh3j1t56v.jpg)

_选择键盘布局_

安装屏幕为你提供所需的安装类型。选择 “正常安装Normal Installation”，然后在 “其他选项Other options” 下选择两个选项。

![https://img.linux.net.cn/data/attachment/album/202301/23/230909j66l6leqq2jg3sn3.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230909j66l6leqq2jg3sn3.jpg)

_选择安装选项_

由于你是在虚拟磁盘空间中安装的，即它只是一个文件，因此你可以安全地选择 “擦除磁盘并安装 UbuntuErase disk and install Ubuntu” 选项。

![https://img.linux.net.cn/data/attachment/album/202301/23/230918z77kin6memzc28xy.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230918z77kin6memzc28xy.jpg)

_安装类型_

点击 “立即安装Install Now” 并 “继续Continue”。

![https://img.linux.net.cn/data/attachment/album/202301/23/230926kzcjoj1yom2yzamm.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230926kzcjoj1yom2yzamm.jpg)

_将更改写入磁盘_

然后选择 “地区region”，添加“你的名字Your name”、“计算机名称Your computer's name”、“用户名Username” 和 “密码Password”。这将是安装后登录 Ubuntu 的用户 id 和密码。

单击 “继续Continue” 开始安装。等到它完成。

![https://img.linux.net.cn/data/attachment/album/202301/23/230934ow2afgswss6gwd0w.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230934ow2afgswss6gwd0w.jpg)

_创建用户帐户_

安装完成后，单击 “立即重新启动Restart Now”。等待几秒钟，你将看到一个登录屏幕。使用用户 id 和密码登录。你应该看到 Ubuntu 桌面在 Windows 端 VirtualBox 中作为 VM 运行。

![https://img.linux.net.cn/data/attachment/album/202301/23/230942bsclslqssse2stee.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/230942bsclslqssse2stee.jpg)

_Ubuntu 安装完成_

![https://img.linux.net.cn/data/attachment/album/202301/23/231113xhadp4n9u2a82qs4.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231113xhadp4n9u2a82qs4.jpg)

_登录 Ubuntu_

![https://img.linux.net.cn/data/attachment/album/202301/23/231121jkfjacqjn0pupufv.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231121jkfjacqjn0pupufv.jpg)

_使用 Virtualbox 在 Windows 中运行的 Ubuntu_

### 安装后配置和提示（可选）

#### 安装客体机增强项

成功安装后，应为 Windows 宿主机和 Ubuntu 客体机安装 “VirtualBox 客体机增强项VirtualBox guest additions”。客体机增强项是一组需要安装在客体虚拟机（即 Ubuntu）内的软件包，以启用 **共享文件夹、双向复制 / 粘贴、自动更改分辨率** 和许多类似功能。

要安装它，请引导到 Ubuntu。从 VirtualBox 菜单中，选择“设备Devices > 插入客体机增强 CD 镜像Insert Guest Additions CD Image”。必要的软件包将安装在 Ubuntu 中。

![https://img.linux.net.cn/data/attachment/album/202301/23/231130l9kp6k05g9g0545y.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231130l9kp6k05g9g0545y.jpg)

_从菜单中选择客体机增强_

打开文件管理器并打开装入的文件夹，如下所示。然后右键单击 > 选择 “在终端中打开open in terminal”。

![https://img.linux.net.cn/data/attachment/album/202301/23/231139c5b6bkvbtv9vcb7f.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231139c5b6bkvbtv9vcb7f.jpg)

_打开已挂载的光盘并选择带有终端的选项_

然后运行以下命令：

`Plain Text sudo ./VBoxLinuxAdditions.run`

![https://img.linux.net.cn/data/attachment/album/202301/23/231149ud0pd0v6ze6ddvez.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231149ud0pd0v6ze6ddvez.jpg)

_VirtualBox 为 Windows 主机添加客体机增强项_

完成上述命令后，重新启动 Ubuntu VM。

#### 启用 Windows 和 Ubuntu 之间的复制和粘贴

要在 Windows 和 Ubuntu 系统之间启用复制和粘贴，请从菜单中选择 “设备Devices > 共享剪贴板Shared Clipboard > 双向Bi-directional”。

![https://img.linux.net.cn/data/attachment/album/202301/23/231159efc6tuctew3t11t2.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231159efc6tuctew3t11t2.jpg)

_启用共享剪贴板_

#### 关闭 Ubuntu VM

理想情况下，你应该从自己的关机菜单中关闭 VM。但是，你也可以从 VirtualBox 主窗口关闭。右键单击虚拟机名称并选择 “关闭Close > 关机Poweroff”。

![https://img.linux.net.cn/data/attachment/album/202301/23/231207pobrnb9d396r9bj2.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231207pobrnb9d396r9bj2.jpg)

_关闭虚拟机_

#### 如何删除 Ubuntu 并删除所有数据

如果要完全删除虚拟机（例如 Ubuntu）及其数据，请选择 “删除Remove” 和 “删除所有文件Delete All Files”。

![https://img.linux.net.cn/data/attachment/album/202301/23/231219bnwlwssnqullu64u.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231219bnwlwssnqullu64u.jpg)

_选择删除以移除虚拟机_

![https://img.linux.net.cn/data/attachment/album/202301/23/231236qd4c2kc44ndcnukh.jpg](https://img.linux.net.cn/data/attachment/album/202301/23/231236qd4c2kc44ndcnukh.jpg)

_选择删除选项_

### 结语