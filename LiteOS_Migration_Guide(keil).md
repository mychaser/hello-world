## 1开源协议说明
**您可以自由地：**
**分享** — 在任何媒介以任何形式复制、发行本作品
**演绎** — 修改、转换或以本作品为基础进行创作
只要你遵守许可协议条款，许可人就无法收回你的这些权利。
**惟须遵守下列条件：**
**署名** — 您必须提供适当的证书，提供一个链接到许可证，并指示是否作出更改。您可
以以任何合理的方式这样做，但不是以任何方式表明，许可方赞同您或您的使用。
**非商业性使用** — 您不得将本作品用于商业目的。
**相同方式共享** — 如果您的修改、转换，或以本作品为基础进行创作，仅得依本素材的
授权条款来散布您的贡献作品。
**没有附加限制** — 您不能增设法律条款或科技措施，来限制别人依授权条款本已许可的
作为。
**声明：**
当您使用本素材中属于公众领域的元素，或当法律有例外或限制条款允许您的使用，
则您不需要遵守本授权条款。
未提供保证。本授权条款未必能完全提供您预期用途所需要的所有许可。例如：形象
权、隐私权、著作人格权等其他权利，可能限制您如何使用本素材。

**注意**
为了方便用户理解，这是协议的概述. 可以访问网址 https://creativecommons.org/licenses/by-sa/3.0/legalcode 了解完整协议内容.

## 2前言
### 目的
本文档介绍基于Huawei LiteOS如何移植到第三方开发板，并成功运行基础示例。
### 读者对象
本文档主要适用于Huawei LiteOS Kernel的开发者。
本文档主要适用于以下对象：
- 物联网端软件开发工程师
- 物联网架构设计师

### 符号约定
在本文中可能出现下列标志，它们所代表的含义如下。

<table>
    <tr>
        <td width="15%">符 号 </td>
		<td>说明</td>
    </tr>
    <tr>
        <td>![](http://rnd-isourceb.huawei.com/images/SZ/20161212/57c1e86b-748a-4c56-85f7-fab2e4a1c20b.png)</td>
		<td>用于警示紧急的危险情形，若不避免，将会导致人员死亡或严重的人身伤害</td>
    </tr>
    <tr>
        <td>![](http://rnd-isourceb.huawei.com/images/SZ/20161212/080b927e-f0d3-461e-aeba-2279dc244112.png)</td>
		<td> 用于警示潜在的危险情形，若不避免，可能会导致人员死亡或严重的人身伤害</td>
    </tr>
    <tr>
        <td>![](http://rnd-isourceb.huawei.com/images/SZ/20161212/5e7c9943-7563-4779-9859-3f31656765b6.png)</td>
		<td>用于警示潜在的危险情形，若不避免，可能会导致中度或轻微的人身伤害</td>
    </tr>
    <tr>
        <td>![](http://rnd-isourceb.huawei.com/images/SZ/20161212/68f7bad3-7251-4320-8165-8dd565c7dfd3.png)</td>
		<td>用于传递设备或环境安全警示信息，若不避免，可能会导致设备损坏、数据丢失、设备性能降低或其它不可预知的结果
		“注意”不涉及人身伤害</td>
    </tr>
    <tr>
        <td>说明</td>
		<td>“说明”不是安全警示信息，不涉及人身、设备及环境伤害信息</td>
    </tr>
</table>

### 修订记录
修改记录累积了每次文档更新的说明。最新版本的文档包含以前所有文档版本的更新
内容。

<table>
	<tr>
	<td>日期</td>
	<td>修订版本</td>
	<td>描述</td>
	</tr>
	<tr>
	<td>2016年12月13日</td>
	<td>1.0</td>
	<td>完成初稿</td>
	</tr>
</table>

## 3概述
一般来说，用户拿到Huawei LiteOS操作系统，想要基于Huawei LiteOS进行业务开发，
主要有两种情况：
1. 将Huawei LiteOS在一款新的芯片上运行起来，根据芯片硬件参数，修改HuaweiLiteOS，最终能简单的运行一个程序。
2. 拿到已适配好芯片的Huawei LiteOS代码包，在单板上运行起来，之后基于HuaweiLiteOS开发新的模块业务，以及增加新的单板外设。

目前在github上已开源的Huawei LiteOS内核源码已适配好STM32F411芯片，本手册将
以STM32F429ZI芯片为例，介绍基于Cortex M4核芯片的移植过程。

## 4环境准备
基于Huawei LiteOS Kernel开发前，我们首先需要准备好单板运行的环境，包括软件环
境和硬件环境。
硬件环境：
<table>
	<tr>
	<td>所需硬件</td>
	<td>描述</td>
	</tr>
	<tr>
	<td>STM32F4291-DISCO单板</td>
	<td>STM32开发板(芯片型号STM32F429ZIT6)</td>
	</tr>
	<tr>
	<td>PC机</td>
	<td>用于编译、加载并调试镜像</td>
	</tr>
	<tr>
	<td>电源（5v）</td>
	<td>开发板供电(使用Mini USB连接线)</td>
	</tr>
</table>


软件环境：
<table>
	<tr>
	<td>软件</td>
	<td>描述</td>
	</tr>
	<tr>
	<td>Window 7 操作系</td>
	<td>安装IAR和st-link的操作系统</td>
	</tr>
	<tr>
	<td>Keil(5.21以上版本)</td>
	<td>用于编译、链接、调试程序代码
	Vision V5.21.1.0
MDK-Lite Version:5.21a</td>
	</tr>
	<tr>
	<td>st-link_v2_usbdriver</td>
	<td>开发板与pc连接的驱动程序，用户加载及调试程
序代码</td>
	</tr>
</table>

**说明**
Keil工具需要开发者自行购买，ST-Link的驱动程序可以从st link的相关网站获取，采用J-Link还
是ST-Link需要根据具体使用的开发板来确定。这里以STM32F429为例，使用ST-Link。


## 5获取Huawei LiteOS 源码

首先我们需要通过网络下载获取Huawei LiteOS开发包。目前Huawei LiteOS的代码已经
开源，可以直接从网络上获取，步骤如下：
1. 打开http://developer.huawei.com/ict/cn/site-iot/product/liteos
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/9983c492-6568-413f-8b99-26ec0f5667dd.png)

1. 点击”获取源码”按钮,进入源代码下载页面
注：如果想下载源代码，请先注册成为会员。

1. 下载完成所有的源代码之后，将其解压到自定义的一个目录中。Huawei LiteOS的源代码目录的各子目录包含的内容如下：
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/39457cd2-105d-4571-85cc-40bd33001a4c.png)
1. 如果是从git上获取的代码，则目录结构如下：
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/76227f0d-7e70-4f03-ac68-32c8f0bf0e08.png)

关于代码树中各个目录存放的源代码的相关内容简介如下：
<table>
<tr>
	<td>一级目录</td>
	<td>二级目录</td>
	<td>说明</td>
</tr>
<tr>
	<td>kernel</td>
	<td>base</td>
	<td>此目录存放的是与平台无关的内核代码，包含核心提供给外部调用的接口的头文件以及内核中进程调度、进程通信、内存管理等等功能的核心代码。用户一般不需要修改此目录下的相关内容。</td>
</tr>
<tr>
	<td></td>
	<td>Include</td>
	<td>内核的相关头文件存放目录</td>
</tr>
<tr>
	<td>platform</td>
	<td>bsp</td>
	<td>目录下则是内核入口相关示例代码。用户自己实现的相关应用程序源代码都可以放到此文件夹下的子目录或者拷贝sample目录更名为其他名称再添加新的源代码。(注：总入口函数是main函数)</td>
</tr>
<tr>
	<td></td>
	<td>Cpu</td>
	<td>该目录以及以下目录存放的是与体系架构紧密相关的硬件初始化的代码。此目录最好按照芯片的体系结构以及芯片型号进行命名方便区分。比如目前我们实现了arm/cortex-m4这个芯片对应的硬件初始化内容。用户最好按照这样的划分进行新的芯片型号的添加</td>
</tr>
<tr>
	<td>api_example</td>
	<td>api</td>
	<td>此目录存放的是内核功能测试用的相关用例的代码</td>
</tr>
<tr>
	<td></td>
	<td>include</td>
	<td>内核功能测试的用例相关头文件</td>
</tr>
<tr>
	<td>projects</td>
	<td>stm32f411_iar</td>
	<td>stm32f411开发板的iar工程目录</td>
</tr>
<tr>
	<td></td>
	<td>stm32f429_iar</td>
	<td>stm32f429开发板的iar工程目录</td>
</tr>
<tr>
	<td></td>
	<td>stm32f429_keil</td>
	<td>stm32f429开发板的keil工程目录</td>
</tr>
<tr>
	<td>doc</td>
	<td></td>
	<td>此目录存放的是LiteOS的使用文档和API说明文档</td>
</tr>
</table>

**说明：**
通过上面给出的链接下载的压缩包中没有doc和projects下的stm32f411_iar、stm32f429_iar、stm32f429_keil目录

获取Huawei LiteOS源代码之后，我们就可以创建project然后编译调试我们的程序了，
详细可以参考后续的各个章节。详细的编程应用编程API请参考《HuaweiLiteOSKernelDevGuide》

## 6创建Huawei LiteOS 工程

在获取完成Huawei LiteOS的源代码和安装好IAR等相关的开发工具后，我们需要用IAR
集成开发环境创建编译Huawei LiteOS的工程，步骤如下：
- 打开Keil uVision5， 然后点击project->New uVision Project...创建一个新的工程
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/61d7bc0e-a102-42e8-9cc1-30245ececc3d.png)
- 保存工程名，比如stm32f429
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/adc358bc-d330-4b17-ac4c-49c3b8bf1aaa.png)
保存后会立即出现芯片型号选择的窗口，根据实际的开发板的芯片进行选择，比如stm32f429zi是目前demo使用的芯片
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/0d3b418c-6d0c-414b-9c4b-6dabc3419a3f.png)
- 然后选择要包含的开发基础库，比如CMSIS、DEVICE两个选项可以选择平台提供的支持包和启动汇编文件，不过目前LiteOS有自己的启动文件，并且不需要额外的驱动，所以可以直接点击OK跳过
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/4b5fbd6b-57c8-4cd2-bd1b-d4a0e11d5e31.png)
- 完成上面的芯片和支持包选择之后，可以添加LiteOS的源代码到工程中，并且配置相关编译选项。
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/b3cc4404-c282-4e0f-9b88-dbe0f36b5219.png)
- 配置target，如果我们需要调试log输出（printf）到ide里面，我们可以选择Use MicroLib
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/d9d6e08a-6121-4507-b8a5-2cf7c22d51d5.png)
- 配置头文件搜索路径，需要kernel/include kernel/base/include platform/include platform/include/keil .....等等，详细参考图片所示内容。
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/7db15225-8994-41db-89e3-5010ab2ede89.png)
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/929424f5-98c2-4d3c-b0f2-f04e08d35288.png)
注：外部下载的LiteOS的压缩包是不包含部分目录的。

- 配置分散加载文件
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/33cc48e4-d6f7-48a1-bcdf-4ea9d6b82863.png)
stm32f429的配置文件内容如下：
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/ea915964-acc4-4451-af30-c4dcf11140b2.png)
说明：分散配置文件中增加的是vector（中断向量表）的内容，LiteOS的中断向量表在stm32f429ZI这个芯片中定义的是0x400大小。如果不了解分散加载文件可以参考IDE的help中sct文件的说明。或者baidu、google分散加载文件相关内容。

- 配置debug使用的驱动
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/458cf688-7427-4943-8a1d-6fe43fe34b65.png)
如上图所示，使用ST-Link

- 如果需要使用printf输出调试log，可以使用软件仿真的方式
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/c6a2fbdc-5d15-4861-aac8-1485c0c36edd.png)

### 添加代码到工程目录
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/59f3a664-a952-42bf-a84f-10ef5046791c.png)
- 然后创建LiteOS的相关目录层级
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/2a7b9ba7-ab7e-4d73-8d0e-fe0b0bac7375.png)
比如我们最终可能添加完成如下内容
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/aa1a5235-b53e-4cf3-8a70-04b244eafac9.png)
说明：
1. los_dispatch.s、los_vendor.s这个文件在git上的代码时放在projects\stm32f429_keil\startup目录下的，如果是iar工程则是在projects\stm32f429_iar\startup目录下。他们因为工具不同所以汇编文件语法有些不一样。
2. 如果是下载的压缩包代码，则这两个文件是在platform\cpu\arm\cortex-m4目录下，并且只有IAR版本的。

### kernel API测试代码
如果需要测试LiteOS是否正常运行，可以将api_example\api添加到工程目录中。

### 测试代码使用
测试代码入口是los_demo_entry.c中的LOS_Demo_Entry()这个接口，使用方法los_config.c的main中调用

示例如下：

	extern void LOS_Demo_Entry(void)；
	int main(void)
	{
		UINT32 uwRet;
		uwRet = osMain();
		if (uwRet != LOS_OK) {
			return LOS_NOK;
		}
		LOS_Demo_Entry()；
		LOS_Start();

		for (;;);
		/* Replace the dots (...) with your own code.  */
	}

**如何选择测试的功能：**

在api_example\include\los_demo_entry.h 打开要测试的功能的宏开关
1. LOS_KERNEL_TEST_xxx，比如测试task调度打开 LOS_KERNEL_TEST_TASK 即可（//#define LOS_KERNEL_TEST_TASK 修改为 #define LOS_KERNEL_TEST_TASK）
2. 如果需要printf，则将los_demo_debug.h中的LOS_KERNEL_DEBUG_OUTLOS_KERNEL_TEST_KEIL_SWSIMU打开。如果是在IAR工程中则不需要打开LOS_KERNEL_TEST_KEIL_SWSIMU
3. 中断测试无法在软件仿真的情况下测试。

**在keil中需要使用printf打印可以有几种方法**
- 将printf重定位到uart输出，这个需要uart驱动支持，如果只有liteOS而没有相关驱动加入工程则不建议使用该方法。
- 使用软件仿真的方式在keil IDE的debug printf view中查看。

## 7编译调试
打开工程后，菜单栏Project→Clean Targets、Build target、Rebuild All target files，可编译文件。这里点
击Rebuild All target file，编译全部文件
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/5ee230ec-5a3b-43c7-9bca-6325547d1bfa.png)

## 8如何基于LiteOS 开发
在实际的应用场景中我们需要开发的内容主要包括两个方面：与硬件相关的驱动程序
以及基于驱动和内核功能的应用程序。下面将分别进行基于LiteOS的应用程序和硬件
驱动程序进行介绍。
在开始介绍之前我们需要对我们开发的代码的存放位置进行相关的规划，否则代码管
理将比较混乱不利于代码阅读以及管理。
1. 应用代码存放位置：建议将应用的代码存在到platform\bsp目录或者该目录下的自定义目录中。
2. 驱动程序存放位置：建议将硬件驱动代码按照设备创建子文件夹放入到platform\cpu\xxx\driver 这样的目录下，比如目前arm的cpu可以是platform\cpu\arm\driver这样的目录。目录不存在可以自己创建。
3. 多个驱动都需要用到的通用头文件可以创建专门的include文件夹来存放。
说明：目前只是针对当前开放的LiteOS的kernel部分来说明的，后续开放更多内容时，根据具体情况来确定目录的存放。


本章节将以通过GPIO接口控制两个板载LED灯交替闪灭来演示如何基于LiteOS开发嵌入式应用。

### 8.1 第三方资料获取
在开发之前我们需要获取STM32F429I-DISCO开发板的相关官方文档，
- 用户使用手册
- 芯片的data sheet
- 基于demo板的示例软件包等等。
在将LiteOS移植到任何其他的平台时，我们都需要获取到芯片厂商提供的demo板以及
基于demo板的各种硬件资料和芯片资料等。
STM32F429I-DISCOdemo板的相关资料我们可以根据需要从这里获取http://www.st.com/content/st_com/en/products/evaluation-tools/product-evaluationtools/mcueval-tools/stm32-mcu-eval-tools/stm32-mcu-discoverykits/32f429idiscovery.html

### 8.2 开发驱动程序
#### Huawei LiteOS 的中断使用
在驱动开发的过程中我们可能会使用到中断，因此我们需要注册自己的中断处理程
序。在Huawei LiteOS中有一套自己的中断的逻辑。
- 在OS启动后，ram的起始地址0x20000000到0x20000400是用来存放中断向量表的，并且系统启动的汇编代码中只讲reset功能写入到了对应的区域。并且用了一个全局的m_pstHwiForm[ ] 来管理中断。
- 开发者需要使用某些中断时，可以通过LOS_HwiCreate (…)接口来添加自己的中断处理函数。如果驱动卸载还可以通过LOS_HwiDelete(….)来删除自己的中断处理函数。系统还提供了LOS_IntLock()关中断LOS_IntRestore()恢复到中断前状态等接口。详细的使用方法可以参考LiteOS中已经使用的地方。

#### 根据硬件设计开发驱动程序
首先，根据需求我们需要查看开发板上有哪些LED灯。通过查看demo板的用户手册，我们发现有两个用户可以使用的LED灯LD3和LD4。
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/13ed0992-80eb-49f4-ab39-a8bddcdc83bf.png)
**说明**
如果是开发其他的设备的驱动程序，需要根据实际硬件原理图进行查找，或者直接让硬件工程师告诉开发者。

然后，在用户手册中找到LD3和LD4属于哪个IO口控制的，比如我们查看到如下内容：
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/e8c13f5d-82f7-4f88-a97c-116e22a52e80.png)
可以看到LD3是I/O PG13控制的，LD4是I/O PG14控制的

之后在stm32f429zit6这个芯片的芯片手册上找到PG14和14的相关IO寄存器信息
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/be173e9d-6a96-4435-b078-503d25beb6dd.png)
从上面可以看到PG13和PG14属于Port G（即属于GPIO G控制）

然后继续在芯片手册中找GPIO G的地址相关信息
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/89bc94f3-ba24-4adf-b51e-7b7cae55115c.png)
如上图所示，可以看到相关的起始地址和结束地址等信息。开发者需要对GPIO控制以及AHB1中其他的与GPIO控制相关的寄存器都进行相关的配置，比如RCC。在芯片手册中没有的话可以在st的官网去查找该芯片的寄存器手册。里面有详细的介绍

将GPIOG的相关寄存器找到后，我们需要将这些寄存器进行初始化以及设置相应的bit位来控制对应的GPIO的拉高拉低来操控外部的led器件。例如我们在代码中定义
	#define PERIPH_BASE 0x40000000U /*!< Peripheral base addr in the alias region*/
	#define APB2PERIPH_BASE (PERIPH_BASE + 0x00010000U)
	#define AHB1PERIPH_BASE (PERIPH_BASE + 0x00020000U)
	#define RCC_BASE (AHB1PERIPH_BASE + 0x3800U)
	#define GPIOG_BASE (AHB1PERIPH_BASE + 0x1800U)
	
注：为了快速的使用相关内容，我们可以将st官方提供的示例代码中的GPIO初始的相关内容都直接复用过来，不需要的代码直接去掉。比如stm32f4xx_hal_gpio.cstm32f4xx_hal_gpio.h文件就是GPIO初始化相关内容，我们可以将其需要用到的基地址定义以及寄存器集合而成的结构体等拷贝到自定义公共头文件中（例如定义一个头文件名为los_hw_driver_com.h）。


### 8.3 开发驱动测试程序
**开发一个led 灯的示例**
1. 首先可以在Huawei_LiteOS\platform\bsp\sample\config目录下创建一个c文件用来编写自己的demo。比如los_led_test.c。
2. 在C文件中实现一些接口通过调用GPIO的驱动程序来实现LED的控制，比如实现BSP_LED_init(),BSP_LED_on(), BSP_LED_off()
3. 在C文件中参考osIdleTaskCreate()创建一个自己的task，在task的处理函数中调用LED控制的相关接口。Task函数参考如下：
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/d7b23520-7930-4eee-8df6-f53c729f9d5e.png)

在完成上面的功能接口以后，还需要在kernel的主入口中进行相关的接口调用。比如目前LiteOS的入口platform\bsp\sample\config\los_config.c文件中的main()函数。
![](http://rnd-isourceb.huawei.com/images/SZ/20161213/c5187062-3cb0-4adf-a2ca-3e2ba3d3feeb.png)

上述完成后基本的工作就完成了，可以编译测试相关代码运行是否正常。


## 9如何移植LiteOS 到已有系统
本章节将讲述如何在已有平台的基础上使用LiteOS提供的功能。
### 修改sct配置文件
这个文件的修改是根据芯片提供的硬件来修改的，主要修改片上的rom的大小以及ram的大小。由于LiteOS使用了自己的中断向量管理机制，所以在配置文件中还需要加入LiteOS需要的向量的相关区域的定义。详细修改方法可以参考创建工程的章节进行配置。

**说明**
这个文件中定义了分散加载的相关内容，LiteOS主要就是要在其中加入中断向量所需要的地址范
围。中断向量的大小根据芯片实际的中断数目进行决定的。如果需要加入其它内容，开发者可以
自己定义。

### 修改startup_xxx.s文件

- 此文件是系统启动文件，目前示例用的demo板可以直接使用开源代码中platform\cpu\arm\cortex-m4目录下提供的los_vendor.s，该文件中主要定义的就是Reset相关内容。
- 比如stm32f429的演示工程中，将stm32官方提供的startup_stm32f429xx.s直接在工程中remove掉，直接使用LiteOS提供的.s文件。或者将LiteOS中的los_vendor.s文件实现的内容增加到startup_stm32f429xx.s中。
- 中断处理函数的实现可以在此.s文件中实现，也可以在c文件中实现，但是最后都必须使用kernel提供的中断注册函数进行注册。

**说明**
此文件中通常还会定义很多系统用到的中断处理函数。不过建议将中断在一个新的c文件实现，
并用LiteOS的中断向量表m_pstHwiForm[]管理或者LOS_HwiCreate()函数动态添加。

### 修改los_dispatch.s文件
- 此文件中是与锁以及进程调度相关的一些内容的实现，不同的芯片类型汇编代码以及寄存器都不一样这样需要根据实际的芯片来进行相关的修改。但是功能必须保证跟LiteOS中提供的汇编代码功能一致。当然用户也可以增加其他需要的功能。
- 目前LiteOS的los_dispatch.s中定义的osPendSV()这个函数是比较重要的与进程调度相关。如果涉及到不同芯片的汇编指令不一样需要特别注意。

### 修改los_config.h配置系统时钟主频
- 此文件中列出了所有的OS配置项，其中包含一个芯片移植过程中必配的选项OS_SYS_CLOCK，需需要将其修改为目标芯片的主频，以保证所有与时钟相关的操作均能正常触发，例如tick中断、任务delay等。
- demo板为STM32F429，硬件主频为可达180Mhz，就需要将OS_SYS_CLOCK配置为180000000，目前LiteOS的kernel并未根据stm32f429的芯片的寄存器进行配置，所以使用的是16M的默认时钟作为system tick的时钟源。

### 移植系统中断和外部中断
- 在los_vendor.s中Huawei LiteOS定义了自己的中断向量表，以STM32F429移植LCD示例为例，开发者需要重点关注系统中断里的tick处理，以及注册板载按键触发的外部中断。

- 系统tick的相关中断。Huawei_LiteOS\platform\bsp\sample\config\los_config.c中的LOS_Start()函数中调用osTickStart()完成了tick的中断处理函数的注册。这样LiteOS的调度相关的内容都会在每个tick中断到来时被执行。注：目前tick的配置是通过LOSCFG_BASE_CORE_TICK_PER_SECOND宏控制的，每秒1000次。即两个tick间隔是1毫秒。如果平台底层驱动有需要在tick中断中处理的，请修改osTickHandler()增加相关内容。

- 例如：stm32的lcd测试程序中注册了tick中断，调用SysTick_Handler()来更新驱动中用的tick。那么使用LiteOS后，中断处理程序由osTickHandler()接管，我们可以在osTickHandler中直接调用SysTick_Handler()来完成移植。不过目前kernel的时钟未进行配置，使用的是16M的时钟，建议采用led示例代码中关于system tick的配置。

- 注册按键响应的外部中断，调用LOS_HwiCreate(EXTI0_IRQn,0,0,EXTI0_IRQHandler,0)注册（经阅读HAL代码，板载按键的中断号为EXTI0_IRQn，响应函数为EXTI0_IRQHandler）

**说明：**
1. LiteOS启动的时候，即platform\bsp\sample\config\los_config.c中的main()调用的osMain()再继续调用到osHwiInit()将中断OS_M4_SYS_VECTOR_CNT开始的其他中断都注册成了defaut的处理函数，用户在需要使用外部中断的话（即中断号大于OS_M4_SYS_VECTOR_CNT的中断），则需要通过调用LOS_HwiCreate()来动态的添加。
2. 在OS_M4_SYS_VECTOR_CNT之前的中断都在m_pstHwiForm[]静态地添加。目前最重要的Reset的的中断处理中只是调用osEnableFPU来使能浮点运算，并没有初始化一些芯片其他的配置，如果需要我们可以将某些寄存器的初始化也放到Reset中进行实现。

### 修改los_config.h文件
目前los_config.h中的内容比较匹配arm cortex M4这种类型的芯片。该文件中在进行不
同芯片平台移植时需要适配修改的内容如下：
- OS_SYS_CLOCK 这个定义的是芯片的时钟
- LOSCFG_BASE_CORE_TICK_PER_SECOND 每秒的tick数，需要写入到相关寄存器中。
- LOSCFG_PLATFORM_HWI_LIMIT 中断的最大数目，这个需要根据实际的芯片进行定义
- LOSCFG_BASE_CORE_TSK_LIMIT 定义系统中能够创建的task的数目。
其他还有许多的宏，其中代码中有相关的注释可以参考，如果有需要可以修改其中的定义，不过修改这些值需要注意不要超过系统中能够使用的内存的大小。


### 关注los_hw.c文件
此文件中的osTskStackInit()中初始化了task stack的 context相关内容，其中这个保存的是task切换时寄存器等相关的内容，具体保存的内容是在los_dispatch.s中实现的。任务切换需要保存哪些内容到这个里面就是根据实际的芯片来决定的了。有些芯片的寄存器比较少，可能需要修改TSK_CONTEXT_S这个结构体中的内容。


### 添加LiteOS到已有的平台示例
本章节描述的内容是以stm32f429zi中的gpio中断示例程序为基础添加LiteOS。
- 首先将LiteOS的代码添加到已有工程中如下图所示：
![](http://rnd-isourceb.huawei.com/images/SZ/20161215/3cb13dfc-710d-4f91-9bf9-acbe478f0e08.png)
- 然后将原始工程的汇编启动文件去掉
![](http://rnd-isourceb.huawei.com/images/SZ/20161215/f76c5ce1-4711-4d5d-8588-5d4ba9cdaab7.png)
说明：如果使用系统原始的汇编文件，则LiteOS中的los_vendor.s不要添加，并且还需要将liteos的代码以及los_dispatch.s中的osPendSV使用的地方都换成工程原始的汇编的接口名称（比如PendSV_Handler）

- 之后我们需要配置好增加liteos后需要的头文件路径以及分散加载文件等内容。
**添加头文件搜索路径**
![](http://rnd-isourceb.huawei.com/images/SZ/20161215/0df67a0e-6b47-43dd-b9a8-43204f42341f.png)
如上图所示的头文件路径，以及编译时需要使用C99

**添加分散加载文件**
![](http://rnd-isourceb.huawei.com/images/SZ/20161215/ddef27c5-94e6-4a41-972a-9ed37f222ebf.png)
**其他需要注意的配置**
![](http://rnd-isourceb.huawei.com/images/SZ/20161215/f25c2945-03e3-4899-985a-4f8c9f775060.png)

修改工程中的main.c，添加和修改如下内容
![](http://rnd-isourceb.huawei.com/images/SZ/20161215/1ad4e2b8-c0e4-49c3-8d48-3610289a4880.png)
1. 将static void SystemClock_Config(void);修改为void SystemClock_Config(void);因为在外部需要调用来初始化时钟源为主PLL（180MHz）
2. 增加一个user按钮的中断处理接口void Gpio_Demo_IRQHandler(void);
3. 将main()函数修改为main_1()或者其他名字，保证和los_config.c中的不能重复定义，并且main()中调用SystemClock_Config()的地方需要将调用注释掉。
4. 重新实现HAL_GetTick()函数，保证已有平台的tick获取和LiteOS中的tick是相同的。

增加一个c文件来实现测试程序的task的创建,然后将该c文件放到liteos的工程目录下即可，并且要添加到工程中进行编译。
	#ifdef __cplusplus
	#if __cplusplus
	extern "C" {
	#endif /* __cpluscplus */
	#endif /* __cpluscplus */

	#include "los_task.h"
	#include "los_hwi.h"

	#include <string.h>

	#define  USART1_IRQn 37

	static UINT32 g_uwDemoTaskID;

	extern void Gpio_Demo_IRQHandler(void);
	extern int main_1(void);
	LITE_OS_SEC_TEXT VOID LOS_Demo_Tskfunc(VOID)
	{
		/* use LOS_HwiCreate to register button irq handler */
		LOS_HwiCreate(6, 0,0,Gpio_Demo_IRQHandler,0);
		/* Call main_1() init gpio and enalbe user button irq */
		main_1();
	}

	void LOS_Test_Gpio_Entry(void)
	{
		UINT32 uwRet;
		TSK_INIT_PARAM_S stTaskInitParam;

		(VOID)memset((void *)(&stTaskInitParam), 0, sizeof(TSK_INIT_PARAM_S));
		stTaskInitParam.pfnTaskEntry = (TSK_ENTRY_FUNC)LOS_Demo_Tskfunc;
		stTaskInitParam.uwStackSize = LOSCFG_BASE_CORE_TSK_IDLE_STACK_SIZE;
		stTaskInitParam.pcName = "GpioDemo";
		stTaskInitParam.usTaskPrio = 10;
		uwRet = LOS_TaskCreate(&g_uwDemoTaskID, &stTaskInitParam);

		if (uwRet != LOS_OK)
		{
			return ;
		}
		return ;
	}

	#ifdef __cplusplus
	#if __cplusplus
	}
	#endif /* __cpluscplus */
	#endif /* __cpluscplus */

修改los_config.c文件中的main()函数，完成相关接口的调用

	extern void SystemInit(void);
	extern void LOS_Test_Gpio_Entry(void);
	extern void SystemClock_Config(void);
	/*****************************************************************************
	 Function    : main
	 Description : Main function entry
	 Input       : None
	 Output      : None
	 Return      : None
	 *****************************************************************************/
	LITE_OS_SEC_TEXT_INIT
	int main(void)
	{
		UINT32 uwRet;
		/*Init system and some register */
		SystemInit();

		uwRet = osMain();
		if (uwRet != LOS_OK) {
			return LOS_NOK;
		}
		/*create gpio irq test task */
		LOS_Test_Gpio_Entry();
		/*config system clock to pll */
		SystemClock_Config();

		LOS_Start();
		for (;;);
		/* Replace the dots (...) with your own code.  */
	}

修改los_config.h中的systick配置
	#define OS_SYS_CLOCK 16000000修改为#define OS_SYS_CLOCK 180000000

经过以上步骤，完成了代码的初步移植，然后可以编译调试运行，通过按板子上的USER按键控制LED灯的亮灭。到此为止一个简单的移植过程就完成了。

**移植需要注意的地方**
汇编启动文件必须保证正常运行，可以使用平台已经有的启动文件再加上los_dispatch.s或者直接使用liteos提供的汇编文件而不使用原工程中的汇编文件。
系统时钟必须根据配置的内容进行修改。
如果涉及到芯片跟文档中使用的型号不一样，那么还需要熟悉汇编的人员修改los_dispatch.s来达到在新平台上能够正常运行。
分散加载文件必须保证正确的配置，如需修改请根据liteOS中提供的分散加载文件进行不同平台的适配

	; *************************************************************
	; *** Scatter-Loading Description File generated by uVision ***
	; *************************************************************

	LR_IROM1 0x08000000 0x00200000  {    ; load region size_region
	  ER_IROM1 0x08000000 0x00200000  {  ; load address = execution address
	   *.o (RESET, +First)
	   *(InRoot$$Sections)
	   .ANY (+RO)
	  }
	  VECTOR 0x20000000 0x400 { ;vector
		* (.vector.bss)
	  }
	  ARM_LIB_STACKHEAP 0x20000400 EMPTY 0x200 {

	  }
	  RW_IRAM1 0x20000600 0x0003FA00  {  ; RW data
	   .ANY (+RW +ZI)
	   * (.data, .bss)
	  }
	}

主要增加了VECTOR 和 ARM_LIB_STACKHEAP 以及内存中加载* (.data, .bss)这个段的内容。


## 其他说明
目前git上提供的代码中直接提供了IAR和Keil的示例工程，可以直接用来进行参考。










