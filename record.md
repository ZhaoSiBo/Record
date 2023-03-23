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


## 进入车机的模型界面

power_key的线，对接一下电源

## 如何连接电源线

从右往左，两根红，两根黑

第一根红线，接IGN 黑线

第二根红线，接 BATT 黄线，ACC红线（绑一起）

第三根黑线，接GND黑线

BTAA 电源线， GND 地线  （通电了）

ACC 按下启动信号

IGN 启动发车


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
grep -nr "01:53"

## 解决GS11 Scrcpy直接调用没有投屏幕

scrcpy --video-encoder=OMX.google.h264.encoder


