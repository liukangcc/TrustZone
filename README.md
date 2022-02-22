# ARM Trustzone For Cortex-M

## 简介

ARM TrustZone® 技术是系统范围的安全方法，针对高性能计算平台上的大量应用，包括安全支付、数字版权管理数字版权管理、企业服务和基于 Web 的服务。

适用于 Arm Cortex-M 处理器的 TrustZone 技术可在所有成本点为物联网设备提供强大的保护水平。该技术通过将关键的安全固件、资产和私人信息与应用程序的其余部分隔离开来，降低了攻击的可能性。它为基于平台安全架构 (PSA) 指南建立设备信任根提供了完美的起点。本篇文章以 STM32U5 为例，探究一下 TrustZone 技术在安全方面的应用。

STM32U5 是一颗主打低功耗和安全应用的 MCU，它的安全特性，从各个方面都比以往的 STM32 系列有了进一步提高。

- 它支持安全启动，这是信任链的可靠锚点：bootlock 可以保证启动的唯一入口，HDP 可以将用户闪存的一部分隐藏起来，通常是复位后运行的安全启动代码，使得其对后面的用户应用程序不可见。
- 为了支持 CM33 内核里的 trustzone 安全扩展，L5 上的外设模块、存储模块都做了升级来配合 trustzone 机制
- 为了实现对软件和数据的保护，L5 新增了配合 trustzone 的安全调试功能，可以限制用户仅能调试非安全世界代码，而无法调试安全世界的代码。另外，L5 还有一个 OTFDEC 模块，可以对存储在芯片外 Ocot-SPI flash 上的密文数据或者密文代码，on the fly 地解密执行和解密读取，从而解放了片上闪存空间对用户代码的限制，同时还能保证用户代码的机密性。

![image-20220222161810814](C:\Users\LiuKang\AppData\Roaming\Typora\typora-user-images\image-20220222161810814.png)

TrustZone的基本原则，就是把应用中的关键操作代码，和其他应用程序隔离开来，避免恶意应用程序的攻击，或者由于应用程序本身的设计漏洞，对敏感操作代码的攻击。

![image-20220222161948734](C:\Users\LiuKang\AppData\Roaming\Typora\typora-user-images\image-20220222161948734.png)

## 特点和优势

- 灵活的基础

  TrustZone 为系统范围的安全性和创建受信任的平台奠定了基础。系统的任何部分都可以设计为安全世界的一部分，包括调试、外设、中断和存储器。

- 简化的安全设计

  TrustZone 允许 SoC 设计人员从一系列组件中进行选择，这些组件在安全环境中实现特定功能。TrustZone 由 Corstone 参考包支持，可帮助公司更快地开发系统。

- 友好型用户设计

  开发人员可以使用熟悉的语言创建 TrustZone 系统，同时保持现有程序员的模型。此外，TrustZone 还由 RTOS、编译器、调试和跟踪解决方案组成的综合生态系统提供支持。

## 名词解释

### CMSE

The Cortex-M Security Extensions(CMSE) 是对安全扩展（架构内在函数和选项）的编译器支持，并且是 Arm C 语言 (ACLE) 规范的一部分。

开发在安全状态下运行的软件时需要 CMSE 功能。这提供了定义安全入口点的机制，并使工具链能够在程序映像中生成正确的指令或支持功能。

使用各种属性和内在函数访问 CMSE 功能。附加的宏也被定义为 CMSE 的一部分。

### GTZC

Global TrustZone Controller (GTZC)：全局 TZ 控制器，由三个子模块组成：

![image-20220222151331943](C:\Users\LiuKang\AppData\Roaming\Typora\typora-user-images\image-20220222151331943.png)

- TZSC: TrustZone security controller

  此子模块定义了从外设的安全 / 特权状态。 它也控制水印内存外设控制器的子区域大小和属性  (MPCWM)。 TZSC 通知一些外设 (如 RCC、GPIO) 关于每个安全外设的安全状态，通过与 RCC 和 I/O 逻辑共享。

- MPCBB: memory protection controller - block based

  此子模块配置信任区系统产品中的内部 RAM，该产品具有分段 SRAM（512 字节页），具有可编程安全性和特权属性。

- TZIC: TrustZone illegal access controller

  此子模块收集系统中的所有非法访问事件，并生成安全中断到 NVIC 。

## 架构设计

## 软件设计

## 参考例程

### 裸机版

### RT-Thread 版

![image-20220222165204893](C:\Users\LiuKang\AppData\Roaming\Typora\typora-user-images\image-20220222165204893.png)

## 参考链接

- [TrustZone for Cortex-M – Arm®](https://www.arm.com/technologies/trustzone-for-cortex-m)
