# Dr.G自动诊断工具使用指南

文件标识：RK-PC-YF-0031

发布版本：V1.0.0

日期：2020-10-09

文件密级：□绝密   □秘密   □内部资料   ■公开

---

**免责声明**

本文档按“现状”提供，瑞芯微电子股份有限公司（“本公司”，下同）不对本文档的任何陈述、信息和内容的准确性、可靠性、完整性、适销性、特定目的性和非侵权性提供任何明示或暗示的声明或保证。本文档仅作为使用指导的参考。

由于产品版本升级或其他原因，本文档将可能在未经任何通知的情况下，不定期进行更新或修改。

**商标声明**

“Rockchip”、“瑞芯微”、“瑞芯”均为本公司的注册商标，归本公司所有。

本文档可能提及的其他所有注册商标或商标，由其各自拥有者所有。

**版权所有** **© 2019** **瑞芯微电子股份有限公司**

超越合理使用范畴，非经本公司书面许可，任何单位和个人不得擅自摘抄、复制本文档内容的部分或全部，并不得以任何形式传播。

瑞芯微电子股份有限公司

Rockchip Electronics Co., Ltd.

地址：     福建省福州市铜盘路软件园A区18号

网址：     [www.rock-chips.com](http://www.rock-chips.com)

客户服务电话： +86-4007-700-590

客户服务传真： +86-591-83951833

客户服务邮箱： [fae@rock-chips.com](mailto:fae@rock-chips.com)

----

**前言**

本文主要对 **Dr.G自动诊断工具各个命令的使用方法** 进行说明，提供相关工程师调试参考。

**读者对象**

本文档主要适用一下工程师：

技术支持工程师

软件开发工程师

**修订记录**

| 日期       | 版本 | 作者   | 修订说明 |
| ---------- | ---- | ------ | -------- |
| 2020-10-09 | V1.9 | 林志雄 | 初始版本 |

**目录**

------

[TOC]


## 1 RGA 测试

* 打印 RGA 相关信息

  `dr-g -rga info`

  打印RGA版本，硬件支持的数据格式，缩放倍数。最大输出分辨率信息。

* RGA 功能测试

  `dr-g -rga test`

  根据RGA支持的格式做格式转换，缩放，旋转，合成的测试。所有转换的结果转为crc数据和预先保存的crc数据做比较。

* RGA 性能测试

  `dr-g -rga perf`

  RGA 效率测试根据 获取到的ddr 频率以及rga的aclk 计算出720p 1080p 4k 数据理论下的耗时与当前测试耗时做比较，从而得出当前运行的RGA 测试是否符合预期。

* rga fast demo 通过命令参数让rga执行对应的功能。输出的源数据和目标数据放在/data/dr-g-file/rga目录下，请按照librga文档说明的格式命名输入数据。 

  命令格式: `dr-g -rga demo sw1280sh720sf0dw1280dh720df0rt2sx0sy0dx0dy0`  注意：`-rga demo` 后的参数串需要连续不能有空格

* 参数说明：

    1. sw sh sf sx sy后面跟着的数据为src 的宽、 src的高、 src的数据格式、src的x偏移y偏移， dw dh df dx dy为dst的对应参数。
    2. alpha功能 bd1 表示 alpha blit 105 ，bd2 表示 alpha blit 405
    3. 旋转功能 rt1 rotate 90, rt2 rotate180, rt3 rotate270, rt4 mirrorH, rt5 mirrotV
    4. 格式对应 sf0 RGBA8888, sf1 RGBX8888, sf2 BGRA8888, sf3 RGB888, sf4 RGB565, sf5 NV12, sf6 NV21, sf7 YV12, sf8 YV21, sf9 NV16, sf10 NV61, sf11 YV16, sf12 YV61


## 2 VOP 测试

* 最大负载测试

  `dr-g -vop -c chip-type`

  `chip-type`可设置的参数有：`rk3399` , `rk3326`, `rk3368` ,  `rk3328` , `rk3288` , `rk3128` , `rk3188`.

  该命令设置后，dr-g 会配置vop，并增加系统ddr负载，用以测试在预设模式下ddr条件是否可以正确输出图像

* 打印 vop 相关信息

  `dr-g -vop -p`

  该命令会输出所有底层配置的vop信息提供调试

* 配置 vop 输出图像

  `dr-g -vop -w <win_id>@<crtc_id>:<src_left>,<src_top>,<src_w>,<src_h>:<dst_left>,<dst_top>,<dst_w>,<dst_h>[#zpos][@<format>]`

  该命令会配置对应 win_id 输出图像，详细的配置信息如下：

   `<win_id>` : 配置输出图像的vop图层id信息，可选值为 `0`,`1`,`2`,`3`.

   `<crtc_id>`: 配置输出图像的vop crtc id信息，可以通过 `dr-g -vop -p` 命令查询。也可以设置为`0`,当设置为`0`时，dr-g会根据设备的连接情况自动选择对应的crtc进行配置，即缺省值为`0`。

   `<src_left>,<src_top>,<src_w>,<src_h>`: 配置源数据的`left`,`top`,`w`,`h`信息。

   `<dst_left>,<dst_top>,<dst_w>,<dst_h>`: 配置目标显示区域的`left`,`top`,`w`,`h`信息。

   `[#zpos]`: 配置输出图层的zpos信息。

   `[@<format>]`:  配置输出图层的format信息，常见的type有：`AR24`,`XR24`,`NV12`等。

*  设置源数据信息

  `dr-g -vop -f <w>x<h>[@<format>][#<file_path>]`

  该命令会配置源数据的宽高，格式，以及bin文件的路径，`format` 缺省值为`XR24`, `file_path`缺省则使用彩条（CPU生成图像数据）。

* 设置系统DDR负载等级

  `dr-g -vop -d level`

  该命令会创建CPU memcpy线程，与 RGA copy线程，不断的读写DDR来提高DDR负载。`level` 可设置值范围为`1-32`.

* 输出 -vop 帮助信息

  `dr-g -vop -h`


## 3 GPU 测试

* 最大负载测试 

  `dr-g -gpu max-load`

  同时可以给定run 的时间,后面直接加时间（单位秒），`dr-g -gpu max-load 100`,  最大负载运行100秒。

* 最大负载测试 ，后台运行，不显示在前端，不影响当前显示内容

  `dr-g -gpu max-load-bg`

  同时可以给定run 的时间,后面直接加时间（单位秒），`dr-g -gpu max-load-bg 100`,  最大负载运行100秒。

* 最大负载测试 ，gpu扫频功能

  `dr-g -gpu max-load -scan-freq`

  `dr-g -gpu max-load-bg -scan-freq`

  同时后面可以给定扫频的频率，直接加时间（单位秒），`dr-g -gpu max-load -scan-freq 5`, 每5s切换一个频率（默认为5s）。

* 最大帧率

  `dr-g -gpu max-fps`

  运行成功后，shell 打印 GPU 应用在当前系统下可以达到的最高帧率，如果shell 打印的帧率小于60fps，那么，请排查屏的刷新率配置。

* 性能测试

  `dr-g -gpu perf`

  测试完成会打印出当前平台是否符合预期性能。

* GLES1.1 测试

  `dr-g -gpu gles11`

  对gles1.1 测试，运行后，shell 打印 egl 信息表示 GLES1.0 运行成功

* GLES2.0 测试

  `dr-g -gpu gles20`

  对gles2.0 测试，运行后，shell 打印 egl 信息表示 GLES20 运行成功

* GLES3.0 测试

  `dr-g -gpu gles30`

  对gles3.0 测试，运行后，shell 打印 egl 信息表示 GLE30 运行成功。

## 4 OpenCL 测试

* CL 测试

  `dr-g -cl `

  系统是否支持openCL，运行后如果提示"OpenCL test OK !" 表示该平台支持opencl。

## 5 Vulkan 测试

* 打印 Vulkan 相关信息

  `dr-g -vk info`

  打印vulkan版本信息，检测支持的vk扩展, 检测支持的vk功能。  

* Vulkan 测试

  `dr-g -vk test`

  移植Google vulkan sample，绘制一个简单的三角形。

## 6 Dump信息

* 收集设备信息

  `dr-g -dump-info <num> <-hwc>`

  收集如下信息：

   1.判断平台：根据属性值判断当前平台。如：3288 7.1；3399 9.0；3326 8.1。

   2.单次收集：平台与各组件版本信息、manifest.xml、commit_id.xml、build.prop、anr_trace文件、logcat、dmesg。

   3.多次收集：CPU+GPU+DDR当前频率与电压、CPU+GPU+DDR最大频率、vop_clk_summary信息、当前的fence状态信息、dumpsys SurfaceFlinger、ps当前进程。

   4.< num >: 多次收集的次数，默认值为1。

   5.< -hwc >: 是否开启hwc debug属性。

   6.Info存储路径：/data/dr-g-file/dumpInfo/

   7.压缩包生成路径：/data/dr-g-file/dumpInfo.tar

## 7 总线优先级设置

* 获取总线优先级

  `dr-g -io-qos get`

  获取总线优先级，并打印出对应的name表格:

  ```
rk3326_mid:/ # dr-g -io-qos get
io_qos get...
set cmds: dr-g -io-qos set [io_num] [pri_num]
    demo: dr-g -io-qos set 4 1
    demo: dr-g -io-qos set 0 0
[io_num]:            NAME:  io_mem_addr: pri_num
     [0]:         gpu_qos:   0xff520008:   1
     [1]:          vop_m0:   0xff550108:   3
     [2]:          vop_m1:   0xff550188:   3
     [3]:        rga_rd_m:   0xff550008:   1
     [4]:        rga_wr_m:   0xff550088:   1
     [5]:         cpu_qos:   0xff508008:   1
  ```

* 修改总线优先级

  `dr-g -io-qos set <io_num> <pri_num>`

  设置总线优先级，按照提示内容，查找表格对应项设置，pri_num可选值为0~3，设置成功后会提示：" io_qos set SUCCESS !!! " 
  Examples:
      `dr-g -io-qos set 4 1`       设置get提示中序号为[4]的优先级为 1 。
      `dr-g -io-qos set 1 2`       设置get提示中序号为[1]的优先级为 2 。
      `dr-g -io-qos set 0 1`       设置get提示中序号为[0]的优先级为 0 。

## 8 VDD电压设置

* 获取系统VDD电压表

  `dr-g -vol get`

  获取/d/regulator/ 下的所有vdd节点，并打印出如下对应的name表格。

  ```
rk3399_Android10:/ # dr-g -vol get
============[0]============
vdd_name:[ vdd_center ]
voltage:
900000 uV
consumers:
Device-Supply                      Min_uV   Max_uV  load_uA
dmc-center                         900000  1350000        0
vdd_center                              0        0        0
============[1]============
vdd_name:[ vdd_cpu_b ]
voltage:
1175000 uV
consumers:
Device-Supply                      Min_uV   Max_uV  load_uA
cpu4-cpu                          1175000  1250000        0
vdd_cpu_b                               0        0        0
  ```

* 设置VDD电压

  `dr-g -vol set <vdd_num> <voltage>`

  设置vdd_num 对应的vdd电压，按照提示内容。设置成功后会提示：" Voltage set success! "

  Examples:
      `dr-g -vol set 2 800000`        设置get提示中序号为[2]的电压为 800000 uV 。
      `dr-g -vol set 1 1000000`       设置get提示中序号为[1]的电压为 1000000 uV 。

## 9 诊断logcat中的报错

* 分析logcat

  `dr-g -log-ans`

  匹配rule.xml中通用的报错log, 并结合其他信息,给出进一步检查或处理的建议.

  检测到异常后，报错格式如下 :

  ```
========================WARNING:3========================
find_err:Failed to set damage region on surface
advice:此报错多发生在安卓8.1系统，是hwui框架代码，在abandon之后，无返回直接报fatal导致的。可以参考redmine:189373
==========================END============================
========================WARNING:4========================
find_err:beginning of crash
advice:发现有Android Crash信息，可以搜索关键字：beginning of crash定位，查看堆栈
==========================END============================
  ```

* 解析指定logcat文件的错误

  `dr-g -log-ans -f PATH`

  使用-f选项指定log文件的路径

  Examples:

    `dr-g -log-ans -f /data/dr-g-file/logAns/logcat_ans.log`

* 添加rule.xml文件的匹配规则

  在\dr-g-release\文件夹下 编辑rule.xml文件，参考已有的条目，添加新的< rule >元素.
  其中: < log_id >为ID号，+1递增；< match_str >为需要匹配的关键log；< advice >为检测到信息后，反馈的建议。

## 10 系统频率设置和查询

* 设置性能模式

  `dr-g -sys-set perf`

  该命令执行后，CPU，GPU，DDR 设置最高频率，温控关闭。回显打印设置后的各频率。

  ```
  rk3326_mid:/ # dr-g -sys-set perf
  CPU0    freq=1.51 Ghz                   //CPU频率
  GPU     freq=520 Mhz                    //GPU频率
  DDR     freq=666 Mhz                    //DDR频率
  GPU utilisation 0~100%:                 //GPU负载: 此时GPU负载为0
  0
  Temperature control :	                  //温控策略: 此时CPU0的温控策略为user_space
  CPU0 :
  user_space
  ```

* 获取当前状态

  `dr-g -sys-set info`

  该命令执行后，回显打印当前CPU，GPU，DDR 当前频率，温控状态。

* 设置GPU三档频率，常用于GPU 瓶颈的应用调试

  `dr-g -sys-set gpu low`

  CPU ，DDR 设置最高频，GPU频率在频率表里面最低档。回显打印出当前设置后的各频率

  `dr-g -sys-set gpu mid`

  CPU ，DDR 设置最高频，GPU频率在频率表里面中档。回显打印出当前设置后的各频率

  `dr-g -sys-set gpu high`

  CPU ，DDR 设置最高频，GPU频率在频率表里面高档。回显打印出当前设置后的各频率

* 设置CPU三档频率，常用于CPU 瓶颈的应用调试

  `dr-g -sys-set cpu low`

  GPU ，DDR 设置最高频，CPU频率在频率表里面最低档。回显打印出当前设置后的各频率

  `dr-g -sys-set cpu mid`

  GPU ，DDR 设置最高频，CPU频率在频率表里面中档。回显打印出当前设置后的各频率

  `dr-g -sys-set cpu high`

  GPU ，DDR 设置最高频，CPU频率在频率表里面高档。回显打印出当前设置后的各频率

* 设置DDR三档频率，常用于GPU 瓶颈的应用调试

  `dr-g -sys-set ddr low`

  CPU ，GPU 设置最高频，DDR频率在频率表里面最低档。回显打印出当前设置后的各频率

  `dr-g -sys-set ddr mid`

  CPU ，GPU设置最高频，DDR频率在频率表里面中档。回显打印出当前设置后的各频率

  `dr-g -sys-set ddr high`

  CPU ，GPU 设置最高频，DDR频率在频率表里面高档。回显打印出当前设置后的各频率

* 恢复动态调频机制

  `dr-g -sys-set reset`

  该命令运行后，CPU，DDR，GPU 恢复系统调频模式，温控恢复。

