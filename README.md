# ESP8266_xiaohei
### 小黑固件是什么


小黑固件是作者比特针对ESP8266开发的一个基于浏览器控制的固件。该固件兼容所有ESP8266模块和开发板。


为了方便使用，该固件完全基于浏览器（手机或电脑均可）通过内置的web服务器完成wifi配网、串口发送数据（同时实现了一个控制MP3模块的功能）、PWM控制舵机、灵活的GPIO控制（可配置不同的GPIO控制LED或者继电器等）。


做这个固件的目的主要包括几点：

    方便不懂得编程的人玩8266

    方便想学习编程的人学习固件开发（目前代码没开源是因为现在的代码结构太垃圾，需要重构，会随着我的后续文章逐步讲解并完善代码，等讲解完代码，最后开源一份质量稍微好一点的代码）

    别人以诗会友，以酒会友，我们程序员以固件会友，以代码会友





### 获取和使用小黑固件


小黑固件默认上传在github，地址如下：

https://github.com/gxku/ESP8266_xiaohei_bin.git


为了升级方便，我还是把固件放在github。可能有些人访问github不方便，可以公众号联系我单独发送。


固件结构如下：

.   
├── bin  
│   ├── app.bin  
│   ├── bootloader.bin   
│   └── partitions_example.bin   
├── esptool.py   
├── flash.sh   
└── README.m   

bin目录就是我们要烧写到8266中的固件文件。
      

如果你本地有8266 idf风格的环境，那么确保连线正确，只要执行

./flash.sh

即可完成固件刷新。

      
如果你本地有其他刷机工具，也可以完成固件刷新。

只要按照这个配置刷机即可

python XXX/esptool.py 
--chip esp8266
--port /dev/ttyUSB0 
--baud 921600 
--before default_reset 
--after hard_reset 
write_flash -z 
--flash_mode dio 
--flash_freq 40m 
--flash_size 1MB 
0x0000 XXX/bootloader.bin
0x10000 XXX/app.bin 
0x8000 XXX/partitions_example.bin

三块橙色的分别为bootloaer.bin、partitions_example.bin、app.bin.

      

烧写完成后重启，会出现find_xiaoohei-XXX的免密码热点，手机或电脑连接，在浏览器输入

192.186.4.1

尽情把玩这个好玩的家伙吧。




      
      
      
      


### 接线方式



看上面的原理图，虽然写的07/08/12E的最小系统，前面说过其实所有的模块都一样。


22引脚为8266 TX引脚-----连接-----USB转串口RX引脚

21引脚为8266 RX引脚-----连接-----USB转串口TX引脚

8266的地线------------------连接-----USB转串口的地线

这样就可以传输数据了，我一般会把8266电源正极接USB转串口3.3V为其供电。


这里要说的是：

如果GPIO0（18引脚）接地上电，8266就会进入烧写模式。

如果GPIO0（18引脚）不接地上电，8266就会正常启动。


在网上找了个01模块的连线图




      
      
      
      



### 小黑固件界面介绍


这是我从电脑上登陆web服务器的截图：

首页目前没有特别的功能（这2个按钮都还不能用，下一个版本会逐步改进），增加了版本说明和作者信息，因为这是一个早期版本，问题可能比较多，包含的功能也很有限，如果大家发现固件有什么bug或者对固件有什么建议，可在公众号联系到我。



当初次烧写完成后进入web界面即可控制串口、PWM、GPIO等。但是因为连的8266的热点，此时你的手机不能上网，所以你可以输入自己的路由器SSID和密码让8266连接路由器（以后想远程控制也需要8266联网）。


点击设置WIFI后界面如下：

出现了获取IP的按钮，点击获取IP，可得到一个有效的IP（不是全0），那么根据提示，手机连接路由器热点，使用新获取的IP控制8266。

注：

    一旦8266连接路由器，将在300S后关闭自身热点

    设置WIFI密码后，8266重新上电会自动连接路由器，如果想让8266忘记路由器，那么设置一个空的SSID即可，重启又可以看见find_xiaohei-XXX了。




这是通过串口1的TX引脚GPIO2以 96008N1 发送数据，下面串口数据是发送任意数据，上面MP3命令封装了一下，接一个可以串口控制的MP3模块--DFPlayer。点开可以看见：下一曲、上一曲、音量设置、单曲循环等各种常用操作，自己加个外壳，就做好了web控制的MP3了。

DFPlayer 如下图，而且只卖几块钱，还不快试试？





这个是PWM控制舵机的界面（我试了9g舵机），目前只支持将GPIO2作为PWM输出（不要问我为什么只支持GPIO2，因为我想在ESP8266 S1上使用）。在Duty设置[500,2500]之间的数值可控制舵机转到不同方向。

注：PWM一旦开启，GPIO2一直产生信号，不能再作为其他功能，除非重启。





可以看到当前页面是GPIO控制界面，可以指定GPIO号，并通过后面的选择框控制指定GPIO口为0或者1。可控制LED灯的开关，继电器的通断。有了继电器就可以控制别的大功率设备。


请注意用8266控制外设，最好给外设单独供电，并保证外设和8266地线（负极）相连，ESP8266只提供控制信号。




好了这就是小黑固件V0.1版本的基本情况，有什么建议或者bug请在下方评论区告诉我，或者公众号后台发消息，让我们一起把8266做的更加好用、有趣。

----------我是比特,一个想把问题搞清楚的青年才俊----------     

这里不能直接贴图片，还是公众号看起来方便一点。       
微信公众号：找小黑（find_xiaohei）


