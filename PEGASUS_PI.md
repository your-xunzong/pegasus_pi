# PEGASUS部分

## **代码结构**

![image-20230724133657476](https://raw.githubusercontent.com/your-xunzong/pegasus_pi/master/imgs/202307242301125.png)

项目运行在app目录下，项目名为udp。

### UDP目录

![image-20230724134749233](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724134749233.png)

#### XX.c文件

udp中pegasus_server.c提供流量转发服务，而pegasus_button.c提供了方向确认服务。

#### 文件夹

include文件夹包含了程序所需要的库文件，而src文件夹中包含了开发板的WiFi连接的代码库。

#### **配置文件**

BUILD.gn包含了本项目所需要的一切库函数链接。

sources中包含需要执行的源文件。

include_dirs包含了源文件所使用到的库函数地址。

```gn

static_library("udp") { 		# 项目名称，即后一个udp
    sources = [
        "pegasus_server.c", 	# 需要执行的源文件
        "pegasus_button.c",		# 需要执行的源文件
        "src/wifi_connect.c",	# 需要执行的源文件
    ]
    
    cflags = [ "-Wno-unused-variable" ]
    include_dirs = [
    	# 以下是库文件目录
        "//utils/native/lite/include",
        "//kernel/liteos_m/components/cmsis/2.0",
        "//base/iot_hardware/peripheral/interfaces/kits/wifiiot_lite",
        "//foundation/communication/interfaces/kits/wifi_lite/wifiservice",
        "//foundation/communication/wifi_lite/interfaces/wifiservice",
        "//vendor/hisi/hi3861/hi3861/third_party/lwip_sack/include/",
        "include",
        "//base/iot_hardware/peripheral/interfaces/kits",
        "//device/soc/hisilicon/hi3861v100/hi3861_adapter/hals/communication/wifi_lite/wifiservice",
        "//device/soc/hisilicon/hi3861v100/hi3861_adapter/kal",
    ]
}
```



### app目录

![image-20230724134726859](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724134726859.png)

#### udp项目

udp文件夹即项目代码所在地，前一个udp

#### 配置文件

BUILD.gn代表整体项目的启动配置，需要在features中加入字段"udp:udp"，

前一个udp代表项目目录名称，后一个是udp项目中的项目名

```
lite_component("app") {
    features = [
        "udp:udp",
        # "car_demo:appDemoCar",
        "startup",
        # "udp_demo:udpServer"
    ]
}
```



## 项目部署

### P板部署

P板需要使用线连接到电脑端，或者直接连电源。

如果需要修改代码，重新烧写程序、显示程序串口信息，就要连电脑，要是不需要看串口信息，就连电源即可。

### app部分

将udp项目文件夹整个放入app文件夹中

```
src\applications\sample\wifi-iot\app\udp
```

然后修改app文件下的BUILD.gn：

```
# Copyright (c) 2020 Huawei Device Co., Ltd.
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

import("//build/lite/config/component/lite_component.gni")

lite_component("app") {
    features = [
        "udp:udp",
        # "car_demo:appDemoCar",
        "startup",
        # "udp_demo:udpServer"
    ]
}
```

### udp部分

#### pegasus_server.c中WiFi热点部署

pegasus_server.c代码中WifiConnect代码中可以设置当前的热点SSID和密码，修改即可。

![image-20230724140956707](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724140956707.png)

#### pegasus_server.c端口、服务器ip设置

开发板和树莓派的ip可以通过手机热点查看并修改。

PORT代表了Pegasus板开放的端口号，可以收到这个端口的信息。

serverC_ip代表了树莓派的ip地址，根据实际情况修改即可。

serverC_port代表了树莓派的接收端口，一般不用调整。

![image-20230724141134321](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724141134321.png)

#### pegasus_button.c端口、ip设置

SERVER_IP填树莓派ip。

SERVER_PORT不用调整。

![image-20230724141656550](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724141656550.png)

### 代码编译、烧写

**如果以上代码经过任意逻辑、参数修改后，就要重新编译烧写程序。**

**如果以上代码经过任意逻辑、参数修改后，就要重新编译烧写程序。**

**如果以上代码经过任意逻辑、参数修改后，就要重新编译烧写程序。**

#### 编译程序

编译需要该插件，直接点击Rebuild，开始编译代码为二进制文件。当编译完成后，执行烧写，将二进制文件部署到开发板上。

![image-20230724164915576](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724164915576.png)

#### 烧写代码

烧写代码至P板之前，先确定P板是否连接到电脑端，同时串口工具Monitor是否关闭。

![image-20230724165146063](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724165146063.png)

如果已经关闭串口工具，并且连接到电脑后，就可以执行烧写命令Upload。

![image-20230724165332980](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724165332980.png)

Upload的刚开始，会提示要点击开发板上的reset按钮，点击即可。

### 验证功能

当开发板烧写完成后，可以验证一下通讯部分是否成功。

#### 连接电脑时

使用串口工具Monitor，当显示菜单时，点击开发板上reset按钮，即可读取到串口信息。

若手机热点发现名为“DEFAULT”的设备时，则说明连接到网络中了。

此时T板发送信息，串口可以看到发来的显示。

则说明部署完成。



## 代码功能

### Pegasus_server.c

连接wifi。

代码负责代理转发ai分析的实时方向。

从8888端口接受到信息，转发到树莓派所在ip的5566端口

#### 连接WiFi热点

通过函数调用到src中的wifi_connect.c文件来连接到当前的热点。

![image-20230724142314173](C:\Users\meijiaming\AppData\Roaming\Typora\typora-user-images\image-20230724142314173.png)

#### 接受来自T板的方向指令

使用udp协议来做网络通信，此时T板为客户端，P板为服务端。

```c
sin_size = sizeof(struct sockaddr_in);

        // 接收来自客户端A的数据
        ssize_t ret;
        if ((ret = recvfrom(sock_fd, recvbuf, sizeof(recvbuf), 0, (struct sockaddr *)&client_sock, (socklen_t *)&sin_size)) == -1)
        {
            printf("recv error \r\n");
        }
        printf("Received data from client A: %s\r\n", recvbuf);
```



#### 转发到树莓派的方向指令

P板接受方向指令后，将同样利用udp协议将该消息发送给树莓派。此时P板是客户端，而树莓派作为服务端。

```cc
// 将数据转发给服务器C
        struct sockaddr_in serverC_sock;
        bzero(&serverC_sock, sizeof(serverC_sock));
        serverC_sock.sin_family = AF_INET;
        serverC_sock.sin_addr.s_addr = inet_addr(serverC_ip);
        serverC_sock.sin_port = htons(serverC_port);

        if ((ret = sendto(sock_fd, recvbuf, strlen(recvbuf), 0, (struct sockaddr *)&serverC_sock, sizeof(serverC_sock))) == -1)
        {
            perror("sendto: ");
        }
        // printf("Forwarded data to server C\r\n");
```

### Pegasus_button.c

通过p板上的按键控制，来发送udp确认指令。

#### 判断按键

P板上的三个按键user键、S1键、S2键全是GPIO_5脚管，唯一区分这三个按键就是电压不同，

按键S1对应的电压均值在0.4-0.6v之间，

按键S2对应的电压均值在0.8-1.1v之间，

user键对应的电压均值在0.01-0.3v之间。

未触发对应电压均值在3v以上

首先代码会获取64次电压，取这64次的平均电压，然后根据平均电压来判断此时的按键是哪个按键。

```cc
hi_void convert_to_voltage(hi_u32 data_len)// data_len取值次数64
{
    hi_u32 i;
    float vlt_max = 0;
    float vlt_min = VLT_MIN;

    float vlt_val = 0;

    hi_u16 vlt;
    for (i = 0; i < data_len; i++) {
        vlt = g_adc_buf[i];
        float voltage = (float)vlt * 1.8 * 4 / 4096.0;  /* vlt * 1.8 * 4 / 4096.0: Convert code into voltage */
        vlt_max = (voltage > vlt_max) ? voltage : vlt_max;
        vlt_min = (voltage < vlt_min) ? voltage : vlt_min;
    }
    // printf("vlt_min:%.3f, vlt_max:%.3f \n", vlt_min, vlt_max);

    vlt_val = (vlt_min + vlt_max)/2.0; //求得平均电压

    if((vlt_val > 0.4) && (vlt_val < 0.6))//判断按键类型
    {
        if(key_flg == 0)
        {
            key_flg = 1;
            key_status = KEY_EVENT_S1;
        }
    }
    if((vlt_val > 0.8) && (vlt_val < 1.1))
    {
        if(key_flg == 0)
        {
            key_flg = 1;
            key_status = KEY_EVENT_S2;
        }
    }

    if((vlt_val > 0.01) && (vlt_val < 0.3))
    {
        if(key_flg == 0)
        {
            key_flg = 1;
            key_status = KEY_EVENT_S3;//s3是user键
        }
    }

    if(vlt_val > 3.0)
    {
        key_flg = 0;
        key_status = KEY_EVENT_NONE;//未触发
    }
}
```

#### 发送确认指令

在判断按键是否触发后，将调用UDPClientFunction(message)函数，该函数可以发送不同按键包含的指令信息给树莓派，从而给小车下达最终指令。

```cc
switch(get_key_event())
        {
            case KEY_EVENT_NONE:
            {
                // UDPClientFunction(message);//未触发时
                // printf("KEY_EVENT_NONE \r\n");
            }
            break;

            case KEY_EVENT_S1:
            {
                const char *message = "1"; // S1确认前进
                UDPClientFunction(message);
                printf("KEY_EVENT_S1 \r\n");
            }
            break;

            case KEY_EVENT_S2:
            {
                const char *message = "3"; // S2急停指令
                UDPClientFunction(message);
                printf("KEY_EVENT_S2 \r\n");
            }
            break;

            case KEY_EVENT_S3:
            {
                const char *message = "2"; // 暂时没用
                UDPClientFunction(message);
                printf("KEY_EVENT_S3 \r\n");
            }
            break;
        }
```



# 树莓派部分

## 首次部署树莓派

首次开机需要先把树莓派连上热点，所以需要用到屏幕进行操作。

### 开机

将存有数据的TF卡插入树莓派，HDMI线接到屏幕中，然后开机通电，当绿灯闪烁时表明正在开机。

### 接入WiFi

进入开始界面后，右上角连接到对应的热点即可。

###  树莓派控制程序

树莓派的控制程序在/home的demo文件夹内，叫car.py。

启动程序即sudo python car.py即可。

```shell
sudo python car.py
```

### 摄像头模块启动

调用motion模块执行一下命令：

```sh
sudo motion
```

### 显示摄像头画面

保证显示设备在同一热点下，使用浏览器打开树莓派的ip:8081端口，（ip在手机端可以看到）

```
192.168.43.55:8081   //以192.168.43.55为例
```



## 远程连接树莓派

和电脑连接服务器一样，使用远程ssh工具连接树莓派，树莓派的ip可以在手机热点看到。用户名mei，密码123789456

```
user:mei
psd:123789456
```

**注意，需要电脑和树莓派都在一个热点内。**

当远程连上树莓派后，就可以在远程工具中启动程序了。

## 摄像头模块

### motion

摄像头模块使用motion库来调用，motion库提供多种数据配置可以设置，内置运动检测框等设置。具体可以百度。

## car.py代码

暂时没树莓派，晚点更新。











