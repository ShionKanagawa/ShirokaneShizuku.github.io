# Xperia 1 II/5 II Lineage OS 刷入教程

## 英文原文（[LineageOS Wiki](https://wiki.lineageos.org/devices/pdx203/install)）

#### 鉴于存在大量中国网友对于英文原版文档感到迷惑，特此翻译与额外添加内容

#### 原文发布于酷安，但鉴于胡松华过于死妈，故重新发在博客上。

## 一. 基本需求

1. 在开始刷机之前，**最好要读一遍说明文档**，以**避免**因**遗漏任何步骤**而出现任何不必要的问题

2. **确保你的电脑有 adb 和 fastboot 工具**，你可以参考 Lineage OS [这篇原文教程](https://wiki.lineageos.org/adb_fastboot_guide.html)去配置它

3. 确保你的设备开启了 USB 调试（您可以在设置-关于手机中点击多次版本号以启用它），并且在开发者选项中开启了**允许 OEM 解锁**选项

   > 如果  **允许 OEM 解锁** 选项为灰色不可选中，请检查您的手机是否能访问 Google

4. 确保你的确没有下载错误的 ROM，比如说 1 II 不能刷 5 II 的包（pdx203 为 Xperia 1 II，pdx206 为 Xperia 5 II）

5. 刷机之前最好重启到系统看一下，是不是所有功能都正常工作（尤其是 VoLTE）

   

## 二. 解锁 Bootloader

1. 在索尼官方系统的拨号器下 输入：*#*#7378423#*#*  (*#*#SERVICE#*#*)  来打开 Service Menu

2. 打开 service info > configuration and check rooting status，如果显示为  **Bootloader unlock allowed: Yes**，那么你可以继续跟着这个教程去解锁 Bootloader。

   如果说显示 **Bootloader unlock allowed: No**，那么你可能需要找某宝去看一下了，或者说使用 **S1Tools/qUnlockTools**，这通常意味着你的手机是日本运营商定制版（如 au, docomo, softbank 版）。经过 qUnlocktools 修改之后， **Bootloader unlock allowed **的值为 **Yes** 之后，再继续跟着本教程解锁 Bootloader

   > S1Tools 的解锁价格通常在 140～150 元左右，希望看到这篇文章的日版用户不要买贵了。

   > AU 版本的 Xperia 5 II 据说解锁 Bootloader 后有可能基带丢失，请自行想办法解决

3. 通过 USB 数据线将手机和电脑链接

4. 在电脑终端输入指令： adb reboot bootloader （如果说你没有配置环境变量的话，可能会无法直接执行）。当然，你也可以通过在关机时，插入 USB 线的同时按住音量上进入 bootloader 模式，**这个时候你手机的 LED 灯应该是蓝色的**

5. 终端使用 **fastboot devices** 来确认设备是否成功连上电脑

6. 然后根据[索尼官方的教程](https://developer.sony.com/open-source/aosp-on-xperia-open-devices/get-started/unlock-bootloader)生成解锁码（酷安也有中文教程，可以看一看）

7. 还是在终端输入 fastboot oem unlock 0x<这一段是你得到的解锁码，没有外面的括号>，比如说你的解锁码是 114514，那么你要输入

   ```
    fastboot oem unlock 0x114514
   ```

8. 通常这个时候 bootloader 就已经解锁了

  解锁成功后，您的手机在开机时会有一个白色的锁的标志 

  

## 三. 通过 Fastboot 安装第三方 Recovery

1. 你可以考虑安装 OrangeFox 或 TWRP，但是这台机器在 OTA 时会用 Lineage OS Recovery 覆盖掉它们，所以本文主要还是讲如何刷入 Lineage OS Recovery

2. 去 Lineage OS 官网下载你所需要的 ROM 和 Recovery（Xperia 1 II : [查看链接](https://download.lineageos.org/devices/pdx203/builds)，Xperia 5 II :[查看链接](https://download.lineageos.org/devices/pdx206/builds)， Recovery.img 就是 Recovery 镜像

3. 依旧是通过 Fastboot 链接手机，手机要处于 Bootloader 模式（LED 灯为蓝色）

4. 电脑终端输入：

   ```
    fastboot flash recovery <你下载到的 recovery 文件名，没有外面的括号，并且你要保证 recovery 在你当前执行命令的路径下>
   ```

   比如说我下载的 recovery 文件叫 homo.img，那么我刷入的时候的指令为 

   ```
   fastboot flash recovery homo.img
   ```

   又假设我把 recovery 下载到 D:\114514\homo.img，那么我可以 

   ```
   fastboot flash recovery "D:\114514\homo.img"
   ```

5. 然后你可以重启到 Recovery 开始刷机了，终端输入 

   ```
   fastboot reboot recovery
   ```

6.  OrangeFox 以及 TWRP 都可以这样刷，没什么本质区别。

   

## 四.通过 adb sideload 刷入 Lineage OS

1. 如果你是第一次刷入，您需要格式化 Data，操作如下：

   > 进入 Recovery 之后点击 Factory Reset 然后点击  Format data / factory reset 来格式化 Data，然后返回主页面

2. 如果你当前没有进入 Recovery，你可以在开机的时候**按住电源键和音量减键**以进入 Recovery

3. 在进入 Recovery 模式的手机上点击 Apply Update 然后 点击 “Apply from ADB” 

4. 在电脑端终端输入 adb sideload <你当前下载的 Lineage OS 的文件名>，比如说为 LineageOS114514.zip，那么应当输入 

   ```
   adb sideload LineageOS114514.zip
   ```

   

5. 现在应该开始刷入了，注意，卡在 47% 很正常，不要惊慌

6. 由于 Xperia 1/5II 为 A/B 设备，所以我建议 A/B 两个插槽都刷一遍，方法很简单，在当前插槽刷完一遍后重启到 Recovery 再重复 3~5 步骤。

7. 如果你不需要 Google 服务，那么你现在可以重启了，如果需要，请往下看：

8. 可以刷入 NikGapps，也可以刷入 MindGapps 。如果你是 Android 13 的话（Lineage OS 20 是 Android 13），可以 Google 搜一下，如果你仅仅需要 Google 服务和 Play Store 那么我建议您刷入 Core 版本，文件名通常为：NikGapps-core-arm64-13-xxxx-signed.zip，刷入方法依旧是

   ```
    adb sideload <你下载到的压缩包的文件名>
   ```

   **依旧是建议两个插槽都刷一遍。**

   > 如果提示 Signature verification failed, 那么在手机端选择 Yes 或者 Continue 以跳过签名校验



## 五. Root/刷入 Magisk

1. 将设备重启到 Recovery 并且连接电脑

2. 在进入 Recovery 模式的手机上点击 Apply Update 然后 点击 “Apply from ADB” 

3. 由此处下载 Magisk （GitHub Releases）：https://github.com/topjohnwu/Magisk/releases

4. 电脑端 adb sideload 输入 （官方现在不推荐的方法，方便一些，但是能用），您可以尝试下面第七步的方法

   ```
   adb sideload Magisk-vxx.x.apk
   ```

   > 从 Magisk 22.0 开始，您可以将 Magisk.apk 当作 Magisk 的安装包使用
   >

5. 如果提示 Signature verification failed, 那么在手机端选择 Yes 或者 Continue 以跳过签名校验

6. 重启到系统，安装刚才下载的 Magisk-vxx.x.apk，完成

7. fastboot 刷写修补过的镜像：您可以在 Lineage OS 的官方下载页面找到当前 ROM 的 Boot 镜像：Xperia 1 II : [查看链接](https://download.lineageos.org/devices/pdx203/builds)，Xperia 5 II :[查看链接](https://download.lineageos.org/devices/pdx206/builds)，请在手机上使用 Magisk Manager 对 boot.img 进行修补，将修补后得到的镜像（通常名为 Magisk_Patched_xxxxx.img）拷贝到电脑，而后将手机置于 Bootloader 模式下使用下面的指令：

   ```
   fastboot flash boot Magisk_Patched_xxxxx.img
   ```

   而后使用

   ```
   fastboot reboot
   ```

   重启到系统



## 六. 感谢

- [@hellobbn](https://github.com/hellobbn) 为 Xperia 1 II 编译了 Lineage OS
- [@Kyasu](https://github.com/kyasu) 为 Xperia 5 II 编译了 Lineage OS