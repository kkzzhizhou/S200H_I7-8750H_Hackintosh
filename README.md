## 硬件清单

| 硬件              | 说明                                   |
| ----------------- | -------------------------------------- |
| CPU               | i7-8950H(Coffee Lake)                  |
| 主板芯片组        | CM238芯片组（Intel 100系列）           |
| 显示屏            | 建议1080P以上分辨率                    |
| **核显**          | **UHD630**(HDMI+DP双输出)              |
| 独显              | 可以通过M.2转接外接独立显卡            |
| 硬盘              | 查阅下面硬盘支持列表（不全，仅供参考） |
| **声卡**          | **ALC269**                             |
| 有线网卡          | Realtek 8168E                          |
| **无线网卡+蓝牙** | 94360 NGFF + **BCM2045A0**             |
| USB               | 四个USB3.0+1个TypeC(5Gbps)             |
| 摄像头            | 谷客HD98(免驱)                         |

##Clover版本：4903

## MacOS版本：Mojave 10.14.4

## 实现情况

| 项目            | 实现情况                    | 存在问题                                                     |
| --------------- | --------------------------- | ------------------------------------------------------------ |
| HDMI音频        | 修改Framebuffer成功驱动     | 无                                                           |
| ALC269          | 注入ID14成功驱动            | 无                                                           |
| UHD630          | 注入设备3E9B0007成功驱动    | 无                                                           |
| 双屏输出        | 修改Framebuffer成功驱动     | 无                                                           |
| PM 961 NVME固态 | 作为系统盘暂无问题          | 无                                                           |
| 有线网卡        | 识别en0，网卡内建，正常使用 | 无                                                           |
| HiDPI+夜览      | 仿冒为iMac显示器驱动        | 开启HiDPI导致不定时屏幕闪烁<br />开机第二屏短暂花屏（8苹果问题） |
| 睡眠            | 睡眠唤醒正常                | 睡眠唤醒后无法打开Chrome<br />Safari无法播放HTML5视频<br />系统无法播放视频 |
| 蓝牙            | 20703A1正常使用             | 无                                                           |
| 无线网卡        | 943602BAE正常使用           | 无                                                           |
| USB             | 使用Hackintool定制正常使用  | 无                                                           |

## 软件问题

| 软件名称           | 使用故障           | 情况   |
| ------------------ | ------------------ | ------ |
| IORegistryExplorer | 能使用，但非常卡顿 | 已解决 |
| MaciASL            | 打开DSDT报错       | 已解决 |

## 折腾经验

1. ### 成功添加IMEI设备能够显著加快黑苹果系统启动速度

   打开IORegistryExplorer，搜索IMEI，如果没有此设备，则使用Clover配置文件中添加AddIMEI和AddDTGP两个Fixes。

   在IMEI设备添加之前，从Clover进入系统登录界面的时间大约在23s左右，添加后，时间缩短至14S。

2. ### 睡眠唤醒后无法网卡速度变慢

   在节能选项中取消选中"唤醒以供网络访问"

3. ### IOregistryExplorer使用卡顿问题

   设置framebuffer-stolenmem为00009003 

   设置framebuffer-cursormem为00000003

   设置framebuffer-fbmem为00000003

4. ### UHD630成功驱动后不定时黑屏

   使用Hackintosh开启HiDPI后出现，不使用HiDPI不会出现闪屏。如果出现画面模糊的情况，则可以使用强制RGB渲染。猜测原因是DVMT内存最大才64MB的缘故，后期可能需要刷写BIOS修正。

5. ### BCM943602BAE中蓝牙2045A0（芯片组20703A1）固件上传问题

   VID：0x0489/PID：0xE0A1

   boot中添加dart=0参数。

6. ### 连接HUB导致蓝牙固件上传失败的问题

   认真检查USB端口定制，如果连接着USB3.0HUB，正常的显示应该是如下图所示。即可同时显示一个USB2.0 HUB和一个USB3.0 HUB
   ![image-20190401190345137](https://ws3.sinaimg.cn/large/006tKfTcly1g1nc7ziwcxj30dg0iltbd.jpg)

7. 无法打开系统DSDT

   删除Hackintosh建议的补丁：change H_EC to EC

8. 屏蔽无用**Generic AHCI Controller**

   在BIOS中只保留SATA接口1，其他接口关闭即可。

......
<!-- more --> 

