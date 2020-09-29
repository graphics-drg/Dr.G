version : v1.9
Dr.G support RK3326(8.1 9.0 10.0 11.0), RK3288(7.1 8.1 9.0 10.0 11.0), RK3399(7.1 8.1 9.0 10.0 11.0), RK3328(8.1 9.0 10.0 11.0), RK3126C(8.1 9.0 10.0 11.0), RK3368(7.1 8.1 9.0 10.0 11.0) RK356x(10.0 11.0)

v1.9:
	1、添加-hwc feature，便捷使用hwc相关调试手段
	2、修复特殊固件，因为没有开启对应节点，导致dumpinfo命令卡死问题
	3、支持Android R

v1.8:
	1、整理dr-g release的形式
	2、ddr频率获取兼容绝大多数较新的固件
	3、修复扫频命令带来的gpu max-load 段错误问题

v1.7:
	1、修复RK3126C ddr频率 无法获取问题
	2、修复kernel 4.19 部分平台vpu节点路径更变导致io命令无法使用问题
	3、修复Android 8.1部分固件无法抓anr的log问题
	4、添加新Feature：dr-g -ext 命令
	5、添加新Feature：gpu拷机扫频功能
	6、添加新的log检测的rule
	7、添加快捷命令：dsf、vi
	8、添加新Feature：修改voltage命令

v1.6:
	1、支持Android Q
	2、修复kernel 4.19 频率节点路径的修改导致 定频功能bug问题
	3、修复GPU获取available频率时，只去最前面的频率为最高频问题
	4、改善push脚本，现在push脚本后，全都能pause住
	5、解决部分平台需要cp librga.so至system/lib目录问题
	6、修复kernel 4.19 dump fence信息节点错误问题
	7、添加 "dr-g -ext" 新feature，可往其拓展其他的binary与脚本等，如：rk-msch-probe-3 测试带宽功能

v1.5:
	1、添加pc端log-ans的使用
	2、修改rk3328和3126c的gpu负载测试的对比参数
	3、adb添加自动检测pc端是否已经自带adb功能
	4、重构dr-g -log-ans ，基于rule.xml匹配规则

v1.4:
	1、修复-gpu gles30测试回显中支持3.0问题
	2、修复rga测试 help逻辑
	3、修复vulkan显示宽高适配机器初始宽高
	4、关闭push等脚本的回显，使得格式更加清晰
	5、添加Rockchip_Dr.G_User_Guide.docx
	6、修改RGA资源文件

v1.3:
	1、修复-sys-set 的 ddr_freq显示频率固定 问题
	2、添加rga源数据,并修改对应文档
	3、修复Android Q上编译问题
	4、回退源码编译dr-g binary的路径至out目录下，并添加do_cp_from_out.sh脚本拷贝回dr-g-release
	5、-log-ans添加对"3288w上使用3288的libGLES_mali.so这种情况出现的DDK报错"的解析

v1.2:
	1、在多次收集信息之后，再收集一次logcat与dmesg。目的：可以收集到在循环期间的log。
	2、添加对外readme，加快办公效率。
	3、cl测试：添加版本打印。
	4、添加对hwc的log收集选项，命令为： dr-g -dump-info -hwc

