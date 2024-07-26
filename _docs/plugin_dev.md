---
title: 插件开发教程（Plugin Development Tutorial）
permalink: /docs/plugin_dev/
redirect_from: /docs/index.html
---

这个简易教程用来帮助用户快速了解如何基于易点开发自己的算法插件。在完成教程以后，你将能够基于易点的可视化框架快速简易的集成自己的算法插件。如果有问题和意见建议，请联系easypoint3d@126.com。

## 1	教程概述
以下是教程中包括的课程概述 (Below is an overview of this tutorial)。

|功能|描述|
|:-------------:|:-------------:|
| 系统介绍 | 介绍开发环境及文件系统 |
| 创建插件 | 如何创建一个新的插件 |
| 编译插件 |如何对插件进行开发|
|加载插件| 如何将插件加载进易点软件 |
|||

## 2	系统介绍

### 2.1 开发环境

**QT版本：5.13.1**

**编译器版本：msvc2017_64**

### 2.2 文件系统

![图片2](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721872586.png)

## 2	创建插件

外部插件包括两种类型：标准插件和读写插件，前者以按钮的形式出现在工 具条和菜单栏里，后者以文件过滤器的形式出现在打开文件和保存文件之后。创建插件是一个完全自动化的过程：首先打开Build/Launcher.exe，点击工具条的“🔧”，填写插件的基本信息及可选信息，如下图所示，点击确定。

![image-20240725100344342](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873609.png)

![image-20240725100406852](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873606.png)

## 3	编译插件

插件的QT 工程会自动生成在Plugins 文件夹下，用QT 打开对应插件文件夹下的pro 文件。

**标准插件以CSF 为例，需要实现CSF_PLUGIN.cpp 文件里7 个函数的功能：**

### 1）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873597.png" alt="image-20240725100736875" style="zoom: 80%;" />

获取插件详细信息，包括输入、参数、输出，例如：

![image-20240725100824582](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873599.png)

> 在该函数下通过一段字符串来描述插件需要的输入数据、所需参数及输出数据。方便后续使用者在通过CMD、Python等进行外部调用时了解插件所需信息。

### 2）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873530.png" alt="image-20240725101210446" style="zoom: 80%;" />

获取插件参数名称，按参数顺序输入，例如：

![image-20240725101410955](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873651.png)

### 3）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873686.png" alt="image-20240725101446779" style="zoom: 80%;" />

获取插件输入数据名称，按输入顺序输入，例如：

![image-20240725101541211](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873741.png)

### 4）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873756.png" alt="image-20240725101556107" style="zoom:80%;" />

获取插件输出数据名称，按输出顺序输入，例如：

![image-20240725101653307](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721873813.png)

### 5）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874044.png" alt="image-20240725102043983" style="zoom: 67%;" />

通过显示UI界面设置输入和参数。

![image-20240725102339146](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874219.png)

### 6）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874091.png" alt="image-20240725102131600" style="zoom: 80%;" />

应用算法处理输入和参数， 并返回结果。

![image-20240725102411340](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874251.png)

> 通过inputs获取数据数据；通过params获取输入参数；将输出结果添加到results中。

### 7）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874164.png" alt="image-20240725102243955" style="zoom:80%;" />

点击按钮，调用UI和执行功能。

![image-20240725102629889](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874389.png)

**读写插件以BIFIO为例， 需要实现 *_FILTER.cpp文件里9个函数的功能：**

### 1）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874565.png" alt="image-20240725102924963" style="zoom:80%;" />

获取插件详细信息，包括读取文件和保存文件 的参数，例如：

![image-20240725102955045](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874595.png)

### 2）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874629.png" alt="image-20240725103029560" style="zoom: 80%;" />

获取读取文件参数名称，按参数顺序输入，例如：

![image-20240725103218834](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874738.png)

### 3）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874753.png" alt="image-20240725103233712" style="zoom:80%;" />

获取保存文件参数名称，按参数顺序输入，例如：

![image-20240725103307514](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874787.png)

### 4）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874812.png" alt="image-20240725103332134" style="zoom:80%;" />

获取默认的读取文件参数 ，名称要与 2）保持一致，例如：

![image-20240725103358909](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874838.png)

### 5）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874871.png" alt="image-20240725103431351" style="zoom:80%;" />

获取默认的保存文件参数 ，名称要与 3）保持一致，例如：

![image-20240725103500085](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721874900.png)

### 6）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875037.png" alt="image-20240725103717279" style="zoom:80%;" />

通过显示UI界面设置读取文件参数。

![image-20240725103954290](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875194.png)

### 7）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875076.png" alt="image-20240725103756106" style="zoom:80%;" />

通过显示UI界面设置保存文件参数。

![image-20240725104015987](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875216.png)

### 8）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875108.png" alt="image-20240725103828059" style="zoom:80%;" />

从文件中按照设置的参数读取数据。

![image-20240725104029652](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875229.png)

### 9）<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875153.png" alt="image-20240725103912927" style="zoom: 80%;" />

将数据按照设置的参数写入到文件。

![image-20240725104043914](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875244.png)

**补充完代码后，直接右键工程进行构建，生成的插件dll和lib会出现在Build文件夹内。**

## 4	加载插件

**插件测试：**

一种方式是重新打开Build/Launcher.exe，会自动加载新生成的插件。如果不重启程序，可以点击“**工具**”  -“**管理插件**” ，点击“**加载其他插件**”按钮 ，从 Build文件夹中添加新生成的插件。 标准插件会出现在“工具条”和“插件” 菜单下，<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875485.png" alt="image-20240725104444930" style="zoom:50%;" />点击按钮可以测试插件的功能是否正确；

**读取测试：**

点击<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875508.png" alt="image-20240725104508753" style="zoom:50%;" />，弹出打开文件对话框，查看是否添加了新打开文件对话框，查看是否添加了新的读写的读写插件插件筛选器，<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875584.png" alt="image-20240725104624414" style="zoom:50%;" />，并测试读取功能。

**保存测试：**

右键添加的数据，点击保存文件，<img src="https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875637.png" alt="image-20240725104717431" style="zoom:50%;" />，弹出保存文件对话框，查看是否添加了新的读写 插件 筛选器， 并测试保存功能。

**CMD测试**：

在Build文件夹下 打开 系统 cmd 首先输入 EP_CMD –help 查看 控制台程序的帮助 图 2 。 输入 EP_CMD –plugin 插件名称 -info 查看插件的详细信息是否正确，更进阶地可以使用 5和 6两条命令测试插件的 批处理 功能是否正确。

![image-20240725105020388](https://raw.githubusercontent.com/ApolloCBT/Image_upgit/master/2024/07/upgit_20240725_1721875820.png)
