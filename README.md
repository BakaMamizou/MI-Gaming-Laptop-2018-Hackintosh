# MI-Gaming-Laptop-2018-Hackintosh
小米游戏本八代增强版EFI
2023.2.6 更新支持macOS Ventura Opencore版本更新至0.8.8
2020.11.21 更新:升级OC版本至0.6.3 
使用Airportitlwm.kext替换itlwm
clover方式引导文件不再更新

### 支持的MacOS版本:
* macOS 10.13 High Sierra
* macOS 10.14 Mojave
* macOS 10.15 Catalina
* macOS 11.00 Big Sur
* macOS 12.00 Monterey
* macOS 13.00 Ventura

### 在开始之前
* 一台i7-8750h/i5-8300h的小米笔记本
* 一台Mac或虚拟机来制作安装媒介
* 一个8G或更大的U盘 (推荐3.0的盘 更快)
* 这个项目里的EFI文件
* (如果要多引导) 请安装第二块 (M.2 SATA) SSD
* (如果使用OpenCore，此项为必要)解锁主板的CFG Lock
* 外接鼠标键盘(如果内建触摸板键盘不能用)
* 安装macOS 10.15时可能需要一个苹果原生支持的网卡以继续安装流程

### 正常工作的部件
* 原生电源管理
* 休眠与唤醒
* Intel核显
* 音频(AppleALC仿冒声卡)
* 触摸板(手势支持，不支持压感)
* USB 3.0
* NVMe和Sata硬盘
* 电池电量显示
* Fn亮度调节
* 内建摄像头
* 内建麦克风 
* 蓝牙
* FileVault磁盘加密 (每次启动之前会自动备份，不建议开启)
* FN快捷键
* 读卡器

### 半完美的部件
* Intel9560AC无线网卡(原生网络支持 半完美HandOff 不支持WPA2-Enterprise加密 隔空传送半残废)

### 不工作的部件:
* Nvidia独立显卡(10.13/14可以用Web Driver驱动，之后的版本不支持)
* HDMI输出
* 隔空传送 隔空投射(无线网卡不能正常驱动的锅)

## 安装过程
制作安装媒介:
* 在您的Mac/虚拟机上创建安装媒介(e.g. https://support.apple.com/zh-cn/HT201372)
* 挂载U盘上的ESP(EFI)分区 : (注意在终端挂载分区时的磁碟号，一定要挂载U盘上的！！！)
* 终端输入示例
```
diskutil list
sudo diskutil mount /dev/disk3s1(注意！是U盘的EFI分区！)
```

* 将Clover EFI文件夹里的EFI文件夹复制到U盘的EFI分区上.
* 弹出U盘，插入到笔记本上
开始安装黑苹果:
* 开机 F12选择U盘引导
* 打开磁盘工具，将要安装MacOS的分区格式化成APFS格式(注意！此操作会删除您该磁盘分区内所有文件!!!)
* 关闭磁盘工具，选择安装MacOS
* 选择要安装的分区，等待
* 安装进度条走完后,自动重启
* F12选择U盘引导，Clover/OC启动界面选择从磁盘启动MacOS以完成接下来的安装
* 等待，MacOS第一次引导跑进度条
* 进入设定界面，按照屏幕上的指示完成开机配置
* 挂载磁盘上的ESP (EFI)分区(注意！挂载时的磁盘序号)

```
diskutil list
sudo diskutil mount /dev/disk0s1
```
### 使用OpenCore方式的:
* 复制此项目的OpenCore EFI内的EFI文件夹到黑苹果的EFI分区
* 将U盘从电脑弹出，重启，开机选择从硬盘引导
* 开始你的黑苹果之旅
### 使用CloverEFI的
* 复制此项目的Clover EFI内的EFI文件夹到黑苹果的EFI分区
* 复制EFI/CLOVER/kexts/Other内的所有文件到 /Library/Extensions folder (若不进行这一步骤，有些硬件可能无法被驱动，如触摸板，网卡等)
* 在终端内输入以下指令以重建Kext缓存:

```
sudo chmod -R 755 /System/Library/Extensions/
sudo chown -R root:wheel /System/Library/Extensions/
sudo chmod -R 755 /Library/Extensions/
sudo chown -R root:wheel /Library/Extensions/
sudo touch /System/Library/Extensions/
sudo touch /Library/Extensions/
sudo kextcache -i / && sudo kextcache -u /
```
* 将U盘从电脑弹出，重启，开机选择从硬盘引导
* 开始你的黑苹果之旅



# 疑难解答
### 一些设备在MacOS下不驱动
用Clover引导:
* 确保所有的kext文件在 /Library/Extensions/ 中:
* 在终端输入以下指令重建Kext缓存(sudo指令完会要求你输入电脑密码)
```
sudo chmod -R 755 /System/Library/Extensions/
sudo chown -R root:wheel /System/Library/Extensions/
sudo chmod -R 755 /Library/Extensions/
sudo chown -R root:wheel /Library/Extensions/
sudo touch /System/Library/Extensions/
sudo touch /Library/Extensions/
sudo kextcache -i / && sudo kextcache -u /
```
* 重启电脑

使用OpenCore引导:
*  确保所有的kext文件在硬盘ESP分区中的EFI/OC/kexts 文件夹内.
*  重启,在OpenCore界面重置NVRAM(OpenCore界面出现时,按压空格并选择重置NVRAM)

# CFG解锁方法
## 注意！修改BIOS属于高风险操作，本人不对因修改BIOS出现的任何问题负责  
  
- 复制BIOS Mod中的EFI文件夹到U盘上的EFI分区
- 重启，F12选择从U盘引导
- 按 = 键来显示项目列表,接着Ctrl+F搜索 setup_var
- 找到0x3E的地址码(屏幕左上角有显示)     
- 将0x01的数值改为0x00 
- 按Ctrl+W来写入修改的数值,重启电脑. 
   现在你可以在hackintool中的日志输出检查CFG LOCK的状态了  
