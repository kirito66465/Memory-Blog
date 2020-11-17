# 记录我的NUC黑苹果之路

## 一、我的配置

**NUC型号** ：NUC8i5BEH
**内存** ：金士顿DDR4 2666MHz 16GB ✖️1（单通道）
**硬盘** ：西数蓝盘500GB SATA SSD
**网卡** ：黑苹果免驱BCM94360CS2
注释：在买NUC之前看了好多已经上了NUC黑苹果的朋友的视频或文章，NUC的8代i5性能已经可以了，上i7散热会压不住；内存方面，预算只能先上一条16GB内存条，后期会再加一条16GB组成双通道；由于买了黑苹果的免驱网卡又没能力自己硬改，所以M.2位置就放了免驱网卡，BEH有SATA硬盘位就放了西数的SSD。在安装黑苹果之前，NUC一到手先安装了Windows10，进入查看了一下基本信息，性能对比起我的那台小米笔记本强的真不是一点半点，这台NUC的配置可以看下图。



![CPU-Z](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggn6wgh5ttj30dr0dtac1.jpg)

![GPU-Z](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggn6xa374cj30e70iadig.jpg)

![AIDA64内存测试](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggn6yfbpizj30i90hlmzq.jpg)


## 二、安装前准备

**硬件** ：32GB U盘
**软件** ：balenaEtcher
**镜像** ：黑果小兵MacOS10.15.4 & @豪客88 NUC8专用镜像
**BIOS** ：0078，设置按照百度“NUC黑苹果BIOS”

## 三、安装过程

### 1、使用黑果小兵镜像
按照安装流程一步步安装，在抹掉磁盘并选择磁盘安装时，会卡在最后2分钟界面，试过多次都是一样的结果。好在黑果小兵网站有对应的解决方法，就是按照如下图替换对应文件。

![卡在最后2分钟解决方法](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggn6yxy21kj30nu07xmxp.jpg)

替换完成之后，好在安装成功能进入系统，但是一打开启动台就特别卡，打开关于本机一看，Iris plus 655只有14M显存，这样不卡才怪呢！于是搜索视频教程用Hackintool生成修复补丁替换掉EFI里的config.plist，重启无论是进入安装盘还是系统盘都卡住，代码一查就是核显没有驱动成功。

### 2、使用NUC8专用镜像
接下来换用豪客88文章里的镜像，但是也出现问题，Mac的磁盘管理想抹盘时发现找不到SATA固态，但是BIOS里和PE里都能成功识别，百度一查发现是SATA驱动的问题。和@维奇在他的一篇文章里交流了下，他的M.2和SATA都成功驱动，EFI文件本身不存在什么多大问题。但是一想，黑果小兵的镜像能成功驱动SATA但是没有驱动核显，而NUC8专用镜像能驱动核显但不能驱动SATA，何不把他俩的结合一下呢？（真是个小机灵鬼！）

### 3、结合EFI安装
从黑果小兵的镜像里的EFI-CLOVER-KEXTS-OTHER目录中找到SATA字样的.kexts文件，放入NUC8专用镜像里的EFI，然后重新安装黑苹果。binggo!果然，我的想法没错，这回成功识别出SATA硬盘并能成功安装完成。打开关于本机，核显成功驱动可惜显示1536M显存还是有问题，其余WIFI、蓝牙免驱都成功识别，内存插槽情况也成功识别出来了，试了一下开机关机重启睡眠唤醒都正常，接下来就是尽可能的完善黑苹果了。

## 四、完善黑苹果

### 1、修复显存
还是使用Hackintool生成修复2048M显存补丁，替换EFI里的config.plist，这下重启就正常显示2048M显存了，想来一开始是没有驱动核显的缘故才没有修复14M显存的问题。打开videoproc软件，核显加速也已开启，打开启动台也很流畅不卡顿。

### 2、开启HIDPI
由于显示器是2K分辨率，系统无法原生开启HIDPI了，这时就需要自己操作打开了。按照百度开启HIDPI的教程，发现GitHub的那个项目地址已无法打开显示404，就只能另辟蹊径了。好在万能的网络还是找到了开启HIDPI的脚本文件，拖到终端里一运行，重启就成功打开HIDPI了

### 3、修改三码/摇号
由于大量的人有可能使用同一EFI，这会导致序列号一样，这时再登录AppleID有可能会大致封号，当然不登录AppleID的人无所谓，而登录了AppleID就可以正常在AppStore中下载以及iCloud管理、iMessage、Facetime，为此就需要修改三码或称为摇号来获得和白果同样的体验。修改三码我是使用的CloverConfiger，具体操作可以看我的B站视频BV1dE411T7yH（视频中演示机是小米笔记本6200U安装的黑苹果以及和USB网卡和热点连接网络，所以会有明显的卡顿和网速慢问题）。

## 五、完善情况

### What Works:
1. HDMI 2K 60Hz视频输出+音频输出
2. 全部USB接口
3. 2.5英寸SATA
4. WIFI与蓝牙
5. AppStore与iCloud
6. 自动休眠、蓝牙或键盘唤醒
7. Sidecar随航
8. Airdrop隔空投送
9. Continuity接力
10. 按电源键询问关机or重启

### Not Working/Known Problems:
1. SD卡无法驱动
2. 雷电3接口，身边只有小米Type-C的降噪耳机，在小米Air上的Type-C接口上能成功识别成声卡，而在NUC上的雷电3接口上不行
3. 蓝牙连接蓝牙耳机，会和WIFI的频段冲突，导致播放声音卡顿
4. 3.5mm耳机孔插拔会有爆音问题
5. Intel 9560AC网卡无法驱动，在安装黑苹果免驱网卡时，拔原装天线拔了插在了免驱网卡上

## 使用的黑苹果安装教程以及EFI文件分享来源

1. @豪客88 NUC8BEX黑苹果维护教程（https://www.jianshu.com/p/2b8516276147）
2. @weachy NUC8（豆子峡谷）黑苹果新手指南Q&A（https://www.jianshu.com/p/b298da6afef3）
3. 黑果小兵的部落阁——macOS安装教程兼小米Pro安装过程记录（https://blog.daliansky.net/MacOS-installation-tutorial-XiaoMi-Pro-installation-process-records.html）

最后，再次感谢@weachy的交流帮助以及黑果小兵的部落阁以及@豪客88。
即使是黑苹果，MacOS的稳定以及苹果生态也是Windows远远不及的，就目前来讲，IDEA和pycharm的打开速度和编译速度都比Windows下快了不少，再加上网页全屏过程中的缩放如丝般顺滑，MacOS还是相当推荐的。
如果觉得看完本文对您有所帮助，请还点赞支持一波，奥利给！
如有意见不合之处，欢迎交流讨论一波。
——2020年4月29日