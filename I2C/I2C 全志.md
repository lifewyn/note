---
time: 2025-03-20
tags:
  - C
link: https://tina.100ask.net/SdkModule/Linux_TWI_DevelopmentGuide-02/
---
###1 源码模块结构

I2C 总线驱动的源代码位于内核在 drivers/i2c/busses 目录下：

![[Pasted image 20250320103855.png]]

### 2 驱动框架介绍

![](https://photos.100ask.net/Tina-Sdk/LinuxTWIDevelopmentGuide_006.png)

​ 图 2-6: TWI 模块结构框图

#### 2.1 Linux 中 I2C 体系结构上图所示，图中用分割线分成了三个层次：
1. 用户空间，包括所有使用I2C 设备的应用程序；
2. 内核，也就是驱动部分；
3. 硬件，指实际物理设备，包括了 I2C 控制器和 I2C 外设。

##### 2.2 Linux 内核中的 I2C 驱动程序从逻辑上又可以分为 6 个部分：
1. I2C framework 提供一种 “访问 I2C slave devices” 的方法。由于这些 slave devices 由I2C controller 控制，因而主要由 I2C controller 驱动实现这一目标。
2. 经过 I2C framework 的抽象，用户可以不用关心 I2C 总线的技术细节，只需要调用系统的接口，就可以与外部设备进行通信。正常情况下，外部设备是位于内核态的其它 driver（如触摸屏，摄像头等等）。I2C framework 也通过字符设备向用户空间提供类似的接口，用户空间程序可以通过该接口访问从设备信息。
3. 在 I2C framework 内部，有 I2C core、I2C busses、I2C algos 和 I2C muxes 四个模块。
4. I2C core 使用 I2C adapter 和 I2C algorithm 两个子模块抽象 I2C controller 的功能，使用 I2C client 和 I2C driver 抽象 I2C slave device 的功能（对应设备模型中的 device 和 device driver）。另外，基于 I2C 协议，通过 smbus 模块实现 SMBus（System Management Bus，系统管理总线）的功能。
5. I2C busses 是各个 I2C controller drivers 的集合，位于 drivers/i2c/busses/目录下，i2c-sunxi-test.c、i2c-sunxi.c、i2c-sunxi.h。
6. I2C algos 包含了一些通用的 I2C algorithm，所谓的 algorithm，是指 I2C 协议的通信方法，用于实现 I2C 的 read/write 指令，一般情况下，都是由硬件实现，不需要特别关注该目录。