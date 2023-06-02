## 切换工程师模式密码 524940 

## 点击设置 然后点击 System 



## host是开发者模式

## devices U盘模式


## 切换adb命令
~~~

su
echo gadget > /sys/devices/platform/5b0d0000.usb/ci_hdrc.0/role
settings put global adb_enabled 1


~~~

## 查看minicom的进程
sudo lsof /dev/ttyUSB0

## 杀死某个进程

sudo kill pid 
adb shell am start com.android.settings/.DevelopmentSettings

// 使用pm命令安装app的时候，先执行关闭安全性检查
## 关闭安全性检查
setenforce 0

## 安装
pm install -r /mnt/media_rw/0CF0-744C/Music-release.apk

com.autoai.m01cbu.music

## 查看进程id
ps -A |grep scan

## 查看指定进程log
logcat |grep 6547

## ubuntu系统，切换Fn功能
echo options hid_apple fnmode=2 | sudo tee -a /etc/modprobe.d/hid_apple.conf
sudo update-initramfs -u -k all
sudo reboot

## 一些外观设置的扩展
sudo apt-get install gnome-tweak-tool
sudo apt-get install  gnome-shell-extension-dash-to-panel (第二条命令已经改动)
重启或者注销后
gnome-tweaks


## 提交时的push命令

git push aosp HEAD:refs/for/ivi/android/app/p9-ga2.0/master

## 有些项目的launcher编译过不了新版Android Studio
1. 补全gradle文件

2. 升级gradle版本，和build tool插件版本到 7.0 + 

distributionUrl=https\://services.gradle.org/distributions/gradle-7.2-all.zip
classpath 'com.android.tools.build:gradle:7.1.2'


## google protobuf 编译过不了

1. 补全maven仓库
maven {
            url "https://plugins.gradle.org/m2/"
        }

2. protobuf的插件版本升级

        classpath "com.google.protobuf:protobuf-gradle-plugin:0.9.1"


## mgftools-uuu烧写

下载 

mgftools-uuu工具下载URL: http://10.20.21.15:9008/ivi/android/mfgtools/mfgtools-uuu-imx8-ga2.0.zip

下载后解压


烧写分 MCU 和 SOC

我们一般只做SOC的烧写， MCU则找MCU的同事进行

去升级包下载网站，下载SOC文件   image.tar.gz  

下载后解压 

一步步找到Image文件夹下，把所有文件复制 放到  烧录工具目录内部的ivi-image下面

烧录工具的目录保存了烧录脚本 ，里面的uuu.ivi-b0，可能需要修改，28G和7G


车机调整到mfg模式。OTG口连接PC。车机上电


sudo ./uuu uuu.ivi-c0.txt

车机调整到正常模式。重启车机

具体每套板子的烧写流程未必相同，跟板子走

烧写相关 

http://10.20.21.4:9006/index.php/NXP_i.MX8_Projects#.E7.83.A7.E5.86.99

远离1 是上，靠近1是下

1 下，2上，正常模式

1 上，2下，烧录模式

安装 Fx11System11 的方法


方法1 ： adb push

1. 启动minicom 连接串口

2. 通过串口命令，打开adb

3. 另起命令行通过，adb devices 查看设备 

4. adb root 获取权限


adb root
作用：获取管理员权限

5. adb remount  

作用：挂载分区，可使系统分区重新可写

'adb remount' 将 '/system' 部分置于可写入的模式，默认情况下 '/system' 部分是只读模式的。这个命令只适用于已被 root 的设备。

在将文件 push 到 '/system' 文件夹之前，必须先输入命令 'adb remount'。

'adb remount' 的作用相当于 'adb shell mount -o rw,remount,rw /system'


之后要知道车机的SystemUI apk的保存位置

这个需要通过串口进入机器的文件系统，记录SystemUI.apk的文件位置 


本次是 /system/priv-app/Fx11SystemUI/ 下面


//拷贝命令   GS11_Instruction
adb push /home/zhaosibo/下载/GS11_Instruction.apk /system/app/GS11_Instruction


adb push /home/zhaosibo/下载/GS11_Instruction.apk /system/app/GS11_Instruction

拷贝命令  SystemUI
adb push /home/zhaosibo/下载/Fx11SystemUI.apk /system/priv-app/Fx11SystemUI


adb push /home/zhaosibo/下载/M01SystemUI.apk /system/priv-app/M01SystemUI


adb push /home/zhaosibo/extend/workspace/M01_KD/out/target/product/m01/system/priv-app/M01SystemUI/M01SystemUI.apk /system/priv-app/M01SystemUI/

/system/app/M01_Instruction


拷贝之后要通过串口修改已经拷贝进入的apk文件的读写权限，到777

chmod 777 Fx11SystemUI.apk

之后通过 ls -l 验证 apk文件是否权限是 rwrwrw

之后通过reboot 直接重启 即可生效


之后通过adb push命令

方法2 ： 通过U盘拷贝到手机上


## 使用Pcan模拟信号

需要 Pcan的信号线，笔记本

链接笔记本和Pcan信号线


然后信号线的红黑架子对接Can-H和Can-L线 （红 H，黑 L）

之后 在信号线等变绿之后。证明对接成功


之后在笔记本上打开软件，模拟信号量，GS11板子第二套，关注发出和接受项，选一个B什么什么4的选项，之后就是各种信号的模拟



## 解决 Persistent apps 问题

android:persistent="true"

然后通过串口进入预装app的文件夹 

/vendor/priv-app/GS11_Music

Music就这这个文件路径下 

重命名预装的 Music app

mv GS11_Music.apk GS11_Music.apk.bak

解决 mv: bad 'GS11_Music.apk': Read-only file system 问题

mount -o rw,remount /vendor  （重新赋予某文件夹读写权限）

之后重启设备，音乐项目已经删除

mount -o rw,remount /vendor


## 关于凯翼项目 的themID和主题的对应关系


themeId==1 刀锋  绿色

themeId ！=1 彗星 蓝色

## 关于ADB 机器需要对接黑口usb才能连上



## 关于源码编译
 

source build/envsetup.sh

lunch m01-userdebug

make update-api && make


## adb启动service 

adb shell am startservice -n com.autoai.carpowerservice/com.autoai.carpowerservice.CarPowerService


## 设置签名 

signingConfigs {
        release {
            storeFile file('/home/zhaosibo/extend/workspace/GS11_CKD/vendor/bon-auto/packages/common/KeyStore/imx8.keystore')
            storePassword 'x-auto'
            keyAlias = 'cowin-platform'
            keyPassword 'cowin'
        }
}


buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
        debug{
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.release
        }
    }


## 查看当前activity    

adb shell dumpsys activity top | grep ACTIVITY


## 本地通过串口写入符号

settings get global autoai_power_mode 1


## 查询文件夹下面的所有包含的内容
grep -nr "**videoApp-BrightnessHelper**"

## 解决GS11 Scrcpy直接调用没有投屏幕

scrcpy --video-encoder=OMX.google.h264.encoder


## 关闭kernel log的命令
su
echo 0 > /proc/sys/kernel/printk


## 串口下，如何知道自己是不是root
执行命令 
uid = 0 是root
id

## 查看当前 挂载状态 

mount


## 设置Home 目录 直接 cd 回到根目录
export HOME=/


## 用程序的进程ID、用户ID、权限角色等信息

ps -ef | grep com.lorizhao.adbtool

只查看id

pidof com.lorizhao.adbtool

我使用Runtime.getRuntime().exec("ls -l")
process.waitFor()
并且捕获了异常，获得提示ls: .: Permission denied


## Jenkins 查看关键日志

FAILURE 

BUILD FAILED

Build failed

error


## fastboot 烧录

su
echo 0 > /proc/sys/kernel/printk   //关闭kernel log
reboot bootloader   进入烧写模式
fastboot devices
查看设备

sh flash_atc

## 解决 8015的adb no permissions问题 (user in plugdev group; are your udev rules wrong?)

sudo usermod -aG plugdev $LOGNAME
sudo apt-get install android-sdk-platform-tools-common

lsusb
得到
Bus 001 Device 035: ID 2be1:0000 ATC AC8015
得到
SUBSYSTEM=="usb", ATTR{idVendor}=="18d1", ATTR{idProduct}=="4eed", MODE="0666", GROUP="plugdev"
执行
$ sudo gedit /etc/udev/rules.d/51-android.rules

把上面SUBSYSTEM开头这个句话，写入到这个文件
$ sudo gedit /etc/udev/rules.d/51-android.rules

sudo udevadm control --reload-rules

## 关闭 kernel log
su
echo 0 > /proc/sys/kernel/printk

## fast boot烧录

su
echo 0 > /proc/sys/kernel/printk   //关闭kernel log

reboot bootloader   进入烧写模式

查看设备
fastboot devices

sh flash_atc


## 查看mcu 日期的命令

rpc-test getInfoSync 24 1 0

## 8015 adb
su
echo 0 > /proc/sys/kernel/printk

su
setprop sys.usb.config nocarplay
sleep 2
setprop sys.usb.config adb
sleep 2
echo device > /sys/class/dual_role_usb/dual-role-usb20/data_role
settings put global adb_enabled 1

su
getprop sys.usb.config
cat /sys/class/dual_role_usb/dual-role-usb20/data_role
settings get global adb_enabled

## 解决adb权限问题命令

dmesg -w |grep monitors                                            

logcat |grep -e CS_ -e monitors&  

logcat |grep avc &

## PCan 信号

### 设防信号
    BCM_4 ArmingSts 
### RVC 倒车信号
    BCM_TCU_G BCM_TCM_2_G = 13 / 0
### 空调展示
    EIPM_2 DisplayActive = 0
### 车速
    ESP_G ASB_ESP_4_G_VehicleSpeed = 0



### 22.04 gerrit 问题

1. code  /etc/ssh/sshd_config 
2. 结尾增加 
HostKeyAlgorithms ssh-rsa
3. 删除.ssh 文件夹

4. 重新身成公钥
生成ed25519密钥
ssh-keygen -t ed25519 -C "your_email@example.com"

5. ssh-add

6. 将~/.ssh/id_ed25519.pub的文本添加到gerrit或git用户配置的SSH keys中


### 22.04 需要额外安装编译工具

./allmake.sh -p gs11_cbu allota

libtinfo5
python3
apt-get install qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools qtcreator

### ffmpeg使用

ffmpeg --version  查看ffmpeg环境

ffplay 文件路径  查看文件具体的编码格式

### 音频焦点关键日志  

MediaFocusControl


### 亮度调节

cGrpId_RpcIviCan1_0x392_BCM_4
Parklightsts
Lightdetected

### 亮度模式

背光：白天模式 ParkLightSts：0  LightDetected：1
背光：白天模式 ParkLightSts：1  LightDetected：1
背光：白天模式 ParkLightSts：0  LightDetected：0
背光：夜晚模式 ParkLightSts：1  LightDetected：0


##Music###-Audio


老车机挂电话

01-01 01:33:17.214   772  4104 I MediaFocusControl: abandonAudioFocus() from uid/pid 10057/7380 clientId=android.media.AudioManager@477e2d1com.bonauto.bt.service.app.phone.BtPhoneCommand$2@ccd7136
01-01 01:33:17.215   772  4104 I MediaFocusControl: AudioFocus  removeFocusStackEntryEx(): removing entry for android.media.AudioManager@477e2d1com.bonauto.bt.service.app.phone.BtPhoneCommand$2@ccd7136
01-01 01:33:17.215   772  4104 D MediaFocusControl: It is not mix stream type return
01-01 01:33:17.215   772  2855 D MediaFocusControl: switchSoundEffectIfNeed oldStreamType:0, newStreamType:-1, isPhoneAndRaderOn:false
01-01 01:33:17.215   772  4104 W ContextImpl: Calling a method in the system process without a qualified user: android.app.ContextImpl.sendBroadcast:1005 com.android.server.audio.MediaFocusControl.broadcastCurre
ntAudioFocus:1644 com.android.server.audio.MediaFocusControl.setConnectRoute:1556 com.android.server.audio.MediaFocusControl.removeFocusStackEntryEx:389 com.android.server.audio.MediaFocusControl.abandonAudioFoc
us:1264 
01-01 01:33:17.305   772  4104 D MediaFocusControl: abandon audioFocus mFocusStack: stack_size=1
01-01 01:33:17.306   772  4104 D MediaFocusControl: mFocusStack[0] mStreamType=12, clientId=android.media.AudioManager@42f556com.autoai.gs11.music.audio.-$$Lambda$Audio$rwE5X-L9LEMoRlnJEEjWOYZ3DCM@9c6d9d7, mFocu
sGainRequest=1, misFocusOwner=true, UID1000
01-01 01:33:17.309   772  4104 D MediaFocusControl: abandonAudioMicFocus mFocusMicStack: stack_size=0
01-01 01:33:17.342   772  2855 D MediaFocusControl: onSetSettingsGlobal name:audio_arkamys_mode value:4
01-01 01:33:17.343   772  2855 D MediaFocusControl: setArkamysMode str:set_arkamys_mode,mode:4,for_tbox:0
01-01 01:33:17.347   772  1007 I MediaFocusControl: requestAudioFocus() from uid/pid 1000/3797 clientId=android.media.AudioManager@cfdb2e0com.autoai.carpowerservice.CarPowerService$7@dc59999 callingPack=com.auto
ai.carpowerservice req=2 flags=0x0 sdk=19
01-01 01:33:17.348   772  1007 D MediaFocusControl: request has higher priority  loss current focus:12
01-01 01:33:17.348   772  1007 D MediaFocusControl: handleExternalFocusGain fr stream:8
01-01 01:33:17.348   772  1007 D MediaFocusControl: hasMessage remove MSG_NOTIFY_FOCUS_GAIN clientId:android.media.AudioManager@42f556com.autoai.gs11.music.audio.-$$Lambda$Audio$rwE5X-L9LEMoRlnJEEjWOYZ3DCM@9c6d9
d7
01-01 01:33:17.349   772  1007 W ContextImpl: Calling a method in the system process without a qualified user: android.app.ContextImpl.sendBroadcast:1005 com.android.server.audio.MediaFocusControl.broadcastCurre
ntAudioFocus:1644 com.android.server.audio.MediaFocusControl.setConnectRoute:1556 com.android.server.audio.MediaFocusControl.requestAudioFocus:1218 com.android.server.audio.AudioService.requestAudioFocus:7130 
01-01 01:33:18.008   772  2855 D MediaFocusControl: switchSoundEffectIfNeed oldStreamType:12, newStreamType:8, isPhoneAndRaderOn:false
01-01 01:33:18.047   772  1007 D MediaFocusControl: requestAudioFocus success mFocusStack: stack_size=2
01-01 01:33:18.047   772  1007 D MediaFocusControl: mFocusStack[0] mStreamType=8, clientId=android.media.AudioManager@cfdb2e0com.autoai.carpowerservice.CarPowerService$7@dc59999, mFocusGainRequest=2, misFocusOwn
er=true, UID1000
01-01 01:33:18.047   772  1007 D MediaFocusControl: mFocusStack[1] mStreamType=12, clientId=android.media.AudioManager@42f556com.autoai.gs11.music.audio.-$$Lambda$Audio$rwE5X-L9LEMoRlnJEEjWOYZ3DCM@9c6d9d7, mFocu
sGainRequest=1, misFocusOwner=false, UID1000


老车机Music


01-01 01:01:44.082  6954  7073 D ##Music###-Audio->: MSG_RESUME
01-01 01:01:44.082  6954  7073 D ##Music###-Audio->: curUsbSource:1
01-01 01:01:44.090  6954  6954 D ##Music###-Audio->: OnAudioFocusChange
01-01 01:01:44.090  6954  6954 D ##Music###-Audio->: AUDIOFOCUS_LOSS_TRANSIENT
01-01 01:01:44.673  6954  7073 D ##Music###-Audio->: focusPackageName:com.autoai.carpowerservice
01-01 01:01:44.673  6954  7073 D ##Music###-Audio->: focusType:8
01-01 01:01:44.673  6954  7073 D ##Music###-Audio->: gainFocus
01-01 01:01:44.673  6954  7073 D ##Music###-Audio->: focusGain1:false
01-01 01:01:44.677  6954  7073 D ##Music###-Audio->: focusPackageName:com.autoai.carpowerservice
01-01 01:01:44.677  6954  7073 D ##Music###-Audio->: focusType:8
01-01 01:01:44.685  6954  6954 D ##Music###-Audio->: autopowerObserver onChange() called with: selfChange = [false], uri = [content://settings/global/autoai_power_mode]
01-01 01:01:44.687  6954  7073 D ##Music###-Audio->: focusGain:false
01-01 01:01:44.687  6954  7073 D ##Music###-Audio->: focus:false
01-01 01:01:44.687  6954  7073 D ##Music###-Audio->: MSG_LOSS_FOCUS
01-01 01:01:44.687  6954  7073 D ##Music###-AudioFast->: resetFast
01-01 01:01:44.687  6954  7073 D ##Music###-AudioPlayer->: currentState =6
01-01 01:01:44.688  6954  7073 D ##Music###-AudioPlayer->: lossFocus
01-01 01:01:44.688  6954  7073 D ##Music###-AudioPlayer->: currentState:6
01-01 01:01:44.688  6954  7073 D ##Music###-AudioPlayer->: targetState:5
01-01 01:01:44.690  6954  6954 D ##Music###-Audio->: powerObserver onChange() called with: power = [3]
01-01 01:01:44.890  6954  6954 D ##Music###-Audio->: handleMessage powerState = [3]




新车机挂电话瞬间

01-01 01:12:49.998  2099  6051 I MediaFocusControl: abandonAudioFocus() from uid/pid 10044/10825 clientId=android.media.AudioManager@5fb145ccom.bonauto.bt.service.app.phone.BtPhoneCommand$2@743e365
01-01 01:12:49.998  2099  6051 D MediaFocusControl: abandon before: mFocusStack: stack_size=2
01-01 01:12:49.998  2099  6051 D MediaFocusControl: mFocusStack[0] mStreamType=0, usage=2, clientId=android.media.AudioManager@5fb145ccom.bonauto.bt.service.app.phone.BtPhoneCommand$2@743e365, mFocusGainRequest=
2, UID10044, isActive=true
01-01 01:12:49.998  2099  6051 D MediaFocusControl: mFocusStack[1] mStreamType=3, usage=1, clientId=android.media.AudioManager@31eaa26com.autoai.gs11.music.audio.-$$Lambda$Audio$rwE5X-L9LEMoRlnJEEjWOYZ3DCM@b4a36
67, mFocusGainRequest=1, UID1000, isActive=false
01-01 01:12:49.998  2099  6051 D MediaFocusControl: removeFocusStackEntry --> mFocusStack top is 2
01-01 01:12:50.006  2099  6051 D MediaFocusControl: cuFr[0]:3
01-01 01:12:50.008  2099  6051 W ContextImpl: Calling a method in the system process without a qualified user: android.app.ContextImpl.sendBroadcast:1012 com.android.server.audio.MediaFocusControl.broadcastCurre
ntAudioFocus:2014 com.android.server.audio.MediaFocusControl.abandonAudioFocus:1258 com.android.server.audio.AudioService.abandonAudioFocus:7912 android.media.IAudioService$Stub.onTransact:654 
01-01 01:12:50.009  2099  6051 D MediaFocusControl: abandon after: mFocusStack: stack_size=1
01-01 01:12:50.009  2099  6051 D MediaFocusControl: mFocusStack[0] mStreamType=3, usage=1, clientId=android.media.AudioManager@31eaa26com.autoai.gs11.music.audio.-$$Lambda$Audio$rwE5X-L9LEMoRlnJEEjWOYZ3DCM@b4a36
67, mFocusGainRequest=1, UID1000, isActive=true
01-01 01:12:50.011  2099  5797 V MediaFocusControl: getActiveAudioFocusType:  fse get
01-01 01:12:50.013  2099  6051 D MediaFocusControl: abandonAudioMicFocus mFocusMicStack: stack_size=0
01-01 01:12:50.113  2099  6051 I MediaFocusControl: requestAudioFocus() from uid/pid 1000/6306 clientId=android.media.AudioManager@6b7d273com.autoai.carpowerservice.CarPowerService$7@655d530 callingPack=com.auto
ai.carpowerservice req=2 flags=0x0 sdk=19




新车机 Music


01-01 01:12:50.007 10628 10628 D ##Music###-Audio->: OnAudioFocusChange
01-01 01:12:50.007 10628 10628 D ##Music###-Audio->: AUDIOFOCUS_GAIN
01-01 01:12:50.012 10628 10703 D ##Music###-Audio->: MSG_GAIN_FOCUS
01-01 01:12:50.012 10628 10703 D ##Music###-AudioPlayer->: gainFocusAutoNext
01-01 01:12:50.012 10628 10703 D ##Music###-AudioPlayer->: targetState:5
01-01 01:12:50.012 10628 10703 D ##Music###-Audio->: gainFocus needPlay=> falsefocusFalseUsbSource==> -1curUsbSource==> 1
01-01 01:12:50.018 10628 10703 D ##Music###-Audio->: MSG_GAIN_FOCUS playBean==>AudioBean{medium='562286074', path='/storage/udisk2/媒体/一路上有你-张学友.128.mp3', title='一路上有你-张学友.128', titleId3=' ', du
rtist=' ', artistId=1, album=' ', albumId=1, fileFolder='/storage/udisk1/媒体', playType=1, usbSource=1}
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: targetState:5
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: currentState:6
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: start
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: ready
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: flagPlayer:true
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: flagState:true
01-01 01:12:50.019 10628 10703 D ##Music###-AudioPlayer->: currentState:6
01-01 01:12:50.027 10628 10703 D ##Music###-Audio->: ######onStart#####
01-01 01:12:50.054 10628 10628 D ##Music###-AudioPlayer->: currentState =5
01-01 01:12:50.060 10628 10703 D ##Music###-Audio->: #####savePPM######
01-01 01:12:50.060 10628 10703 D ##Music###-AudioPlayer->: currentState =5
01-01 01:12:50.060 10628 10703 D ##Music###-AudioPlayer->: ready
01-01 01:12:50.060 10628 10703 D ##Music###-AudioPlayer->: flagPlayer:true
01-01 01:12:50.060 10628 10703 D ##Music###-AudioPlayer->: flagState:true
01-01 01:12:50.060 10628 10703 D ##Music###-AudioPlayer->: currentState:5
01-01 01:12:50.065 10628 10703 D ##Music###-AudioBean->: toPPM
01-01 01:12:50.071 10628 10703 D ##Music###-AudioBean->: PpmBean{medium='562286074', path='/storage/udisk2/媒体/一路上有你-张学友.128.mp3', playType=1, lastTime=1577812370065, folder='/storage/udisk1/媒体', curP
24}
01-01 01:12:50.071 10628 10703 D ##Music###-AudioPpm->: putPpmBean
01-01 01:12:50.071 10628 10703 D ##Music###-AudioPpm->: PpmBean{medium='562286074', path='/storage/udisk2/媒体/一路上有你-张学友.128.mp3', playType=1, lastTime=1577812370065, folder='/storage/udisk1/媒体', curPo
4}
01-01 01:12:50.076 10628 10703 D ##Music###-Audio->: sendMeter
01-01 01:12:50.245 10628 10703 D ##Music###-Audio->: MSG_RESUME
01-01 01:12:50.245 10628 10703 D ##Music###-Audio->: curUsbSource:1
01-01 01:12:50.246 10628 10703 D ##Music###-Audio->: focusPackageName:com.autoai.carpowerservice
01-01 01:12:50.246 10628 10703 D ##Music###-Audio->: focusType:0
01-01 01:12:50.246 10628 10703 D ##Music###-Audio->: gainFocus
01-01 01:12:50.246 10628 10703 D ##Music###-Audio->: focusGain1:true
01-01 01:12:50.252 10628 10628 D ##Music###-Audio->: OnAudioFocusChange
01-01 01:12:50.252 10628 10628 D ##Music###-Audio->: AUDIOFOCUS_LOSS_TRANSIENT
01-01 01:12:50.257 10628 10703 D ##Music###-Audio->: focusPackageName:com.autoai.carpowerservice
01-01 01:12:50.257 10628 10703 D ##Music###-Audio->: focusType:0
01-01 01:12:50.259 10628 10628 D ##Music###-Audio->: autopowerObserver onChange() called with: selfChange = [false], uri = [content://settings/global/autoai_power_mode]
01-01 01:12:50.261 10628 10703 D ##Music###-Audio->: focusGain:false
01-01 01:12:50.261 10628 10628 D ##Music###-Audio->: powerObserver onChange() called with: power = [3]
01-01 01:12:50.261 10628 10703 D ##Music###-Audio->: focus:false
01-01 01:12:50.261 10628 10703 D ##Music###-Audio->: MSG_LOSS_FOCUS
01-01 01:12:50.261 10628 10703 D ##Music###-AudioFast->: resetFast
01-01 01:12:50.263 10628 10703 D ##Music###-AudioPlayer->: currentState =5
01-01 01:12:50.264 10628 10703 D ##Music###-AudioPlayer->: lossFocus
01-01 01:12:50.264 10628 10703 D ##Music###-AudioPlayer->: currentState:5
01-01 01:12:50.264 10628 10703 D ##Music###-AudioPlayer->: pause
