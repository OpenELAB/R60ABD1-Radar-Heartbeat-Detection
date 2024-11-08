&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__Hi👋__，在这篇文章中，我们将带您一步步探索如何使用 R60ABD1 雷达模块 📡，帮助您快速掌握它的安装、配置和核心功能应用 🎉。通过这个模块，您可以实现对人体心率 💓、呼吸 💨、位置 📍 和体动参数的精准监测，非常适合应用于智能家居 🏠 和医疗护理 🏥 场景。
接下来，将通过以下几个步骤📜，带你深入源码，轻松上手这个项目！准备好了吗？让我们开始吧🚀！  
- 📝 项目简介
- ✨ 功能特点
- 🏗 项目代码
- 🚀 安装与运行
- 🔧 使用说明

# R60ABD1 项目
## 📝项目简介:
R60ABD1 雷达模块 是一款基于 60GHz 毫米波雷达技术的高科技睡眠监测设备 🛏️，专为监测人体心率 💓、呼吸 💨、位置 📍和体动参数设计。通过精准的毫米波探测技术，R60ABD1 在不受环境影响的情况下依然能够提供可靠的数据采集，非常适合智能家居 🏠 和医疗护理 🏥 等场景。R60ABD1 模块不仅支持 Zigbee、Wi-Fi、4G 等多种通信协议 📡，还能通过多种安装方式轻松适应不同需求，让用户实现实时、精准的人体监测。无论是守护家人的健康还是辅助医疗护理，它都能轻松胜任！

## ✨功能特点
- 多天线设计 📡：采用一发三收的天线布局，宽波束设计适用于置顶安装。通过算法控制探测角度，可精确扫描人体的全身动作层次，为您提供全面的监测数据。
- 多功能探测 💡：具备睡眠监测、呼吸 💨 和心率 💓 探测功能。监测距离覆盖 1.5 米（睡眠质量监测）和 0.4–2 米（心率和呼吸监测），满足多种人体监测需求。
- 环境适应性强 🌍：不受温度、湿度、噪声、气流、尘埃和光照等外部环境的影响，在各种条件下都能保证监测的准确性。
- 丰富的通信接口 🔗：支持 Zigbee、Wi-Fi 和 4G 等多种通信协议，便于与其他设备集成，实现高效的数据传输。
- 灵活的安装方式 ⚙️：支持置顶、斜下、水平等多种安装方式，安装高度建议在 2.75 米左右，以更好地监测呼吸和心率。
- 低功耗小体积 🔋：模块功耗仅为 5V/93mA，体积小巧（≤35mm×31mm×7.5mm），适合智能家居和医疗设备等对节能和空间有限的应用场景。
## 🏗项目代码
``` 
try:
    import RPi.GPIO as GPIO
except RuntimeError:
    print("Error importing RPi.GPIO!  This is probably because you need superuser privileges. "
          " You can achieve this by using 'sudo' to run your script")
# -*- coding: utf-8 -*
import serial
from time import sleep

ser = serial.Serial("/dev/ttyAMA0",115200,timeout=2)

if not ser.isOpen():
    print("open failed")
else:
    print("open success: ")
    print(ser)

try:
    while True:
        x = 0
        d = 0
        data_all = []
        i = 0
        while ser.inWaiting()> 0:
            str_n = ser.read()  # Unprocessed raw data
            str_y = str(str_n.hex())  # Processed data
            i += 1
            data_all.append(str_y)
        if data_all != []:
            if (int(data_all[2][0], 16) * 10 + int(data_all[2][1], 16) == 80) and (int(data_all[3][0], 16) * 10 + int(data_all[3][1], 16) == 3):
                #print(data_all)
                d = int(data_all[6][0], 16) * 16 + int(data_all[6][1], 16)
                print("BodyMoveData:%d" % d)
            if (int(data_all[2][0], 16) * 10 + int(data_all[2][1], 16) == 80) and (int(data_all[3][0], 16) * 10 + int(data_all[3][1], 16) == 4):
                #print(data_all)
                d = int(data_all[6][0], 16) * 16**3 + int(data_all[6][1], 16) * 16**2 + int(data_all[7][0], 16) * 16 + int(data_all[7][1], 16)
                print("BodyDistanceData:%d" % d)
            if (int(data_all[2][0], 16) * 10 + int(data_all[2][1], 16) == 80) and (int(data_all[3][0], 16) * 10 + int(data_all[3][1], 16) == 5):
                #print(data_all)
                d = int(data_all[6][1], 16) * 16**2 + int(data_all[7][0], 16) * 16 + int(data_all[7][1], 16)
                if int(data_all[6][0], 16) == 8:
                    print("BodyPosition_x:-%d" % d)
                else:
                    print("BodyPosition_x:%d" % d)
                d = + int(data_all[8][1], 16) * 16**2 + int(data_all[9][0], 16) * 16 + int(data_all[9][1], 16)
                if int(data_all[8][0], 16) == 8:
                    print("BodyPosition_y:-%d" % d)
                else:
                    print("BodyPosition_y:%d" % d)
                d = int(data_all[10][1], 16) * 16**2 + int(data_all[11][0], 16) * 16 + int(data_all[11][1], 16)
                if int(data_all[10][0], 16) == 8:
                    print("BodyPosition_z:%-d" % d)
                else:
                    print("BodyPosition_z:%d" % d)
            if (int(data_all[2][0], 16) * 10 + int(data_all[2][1], 16) == 85) and (int(data_all[3][0], 16) * 10 + int(data_all[3][1], 16) == 2):
                #print(data_all)
                d = int(data_all[6][0], 16) * 16 + int(data_all[6][1], 16)
                print("BodyHeartData:%d" % d)
            if (int(data_all[2][0], 16) * 10 + int(data_all[2][1], 16) == 81) and (int(data_all[3][0], 16) * 10 + int(data_all[3][1], 16) == 2):
               #print(data_all)
                d = int(data_all[6][0], 16) * 16 + int(data_all[6][1], 16)
                print("BodyBreathData:%d" % d)
        sleep(0.05)
except KeyboardInterrupt:
    if ser != None:
        ser.close()
```
## 🚀安装与运行

### 先决条件
软件依赖：__Raspberry Pi 4B__、等   
硬件要求：__R60ABD1__、杜邦线、OpenELAB拓展板等  

### 引脚介绍以及接线
| 接口    | 引脚   |描述         | 
|---------|--------|--------------|
| 接口1   | 1      |5.0V         |
| 接口1   | 2      |GND          |
| 接口1   | 3      |3.3V         |
| 接口1   | 4      |3.3V         |
| 接口1   | 5      |自定义        |

### 编译运行
1、完成安装依赖后，打开好下载的压缩包  

![QQ_1726107516108](https://github.com/user-attachments/assets/cb2362f7-1871-418e-94dd-92ddfe7284b7)  

2、使用USB-C将Plus2连接至电脑，选择Tools->Port选择自己的端口  

![QQ_1726107673971](https://github.com/user-attachments/assets/17f0392a-b753-4aba-946c-ede75ba9092f)  

3、点击编译，待编译完成后再点击上传  

![QQ_1726107957719](https://github.com/user-attachments/assets/c1f953ad-5355-44e8-af0c-ac5da7542aa6)  

## 使用说明
- ### 图片顺序、个数
老虎机共五列，每一列都可以放置 10 个图标，而且你可以随意调整它们的顺序！💡目前，我们已经准备了 6 个 48x48 像素的素材图标，它们的 RGB565 十六进制数据已经在代码里了，分别对应 slot_symbols 数组中的 0 到 5 号元素。如果你想调整每列的图标顺序和数量，只需要轻松修改 symbolIndices 数组中的数字，就能改变每一列的图标显示效果！🔧🎨  

![QQ_1726108827608](https://github.com/user-attachments/assets/45b5878d-3624-47b5-a671-fc40937d1898)

- ### 列与列、图与图的间隔
通过更改PAD_X以及PAD_Y可以更改列与列、图与图的间隔，通常默认是2，0  

![QQ_1726109192019](https://github.com/user-attachments/assets/3e14c412-8342-486d-ba00-b6a0f4d357ac)

- ### 转盘转动速度、停止减小速度
```
#define Speed_MAX 800//老虎机旋转的最高速度
#define Speed_MIN 50//老虎机旋转最低速
#define Acceleration_MAX 12 //老虎机加速时的加速度
#define Acceleration_MIN -20//老虎机减速时的加速度
```
  ![QQ_1726109492610](https://github.com/user-attachments/assets/aaa6b4a0-79b1-491a-8dbd-ca76cc8c1eee)

## 下期预告
下期将详细介绍怎么更改老虎机的图片，我们会通过对图片取模获得图片的十六进制参数，并调整成我们想要的格式，然后在老虎机上呈现出我们所需要的图片 __敬请期待!!!__  

![QQ_1726122393803](https://github.com/user-attachments/assets/71507de5-dad0-4688-84bf-56cc25878e35)  

[第二部分链接](https://github.com/OpenELAB/OpenELAB-M5StickCPlus2-Slot-2.git)
## 如何联系维护者或开发者
__OpenELAB:__   
[![OpenELAB_logo_resized_150](https://github.com/user-attachments/assets/5d3de375-359c-46a3-96bb-aaa211c6c636)](https://openelab.io)  
__YouTube:__  
[![youtube_logo_200x150](https://github.com/user-attachments/assets/d2365e7f-4ffe-4124-bf62-21eba19a71e4)](https://www.youtube.com/@OpenELAB)  
__X :__  
[![X_logo_150x150](https://github.com/user-attachments/assets/4ad5095f-2573-4791-9360-b355530093bf)](https://twitter.com/openelabio)  
__FaceBook:__  
[![facebook_logo_cropped_150x150](https://github.com/user-attachments/assets/52f2dc9a-a564-49a5-b72e-30eafbbc281f)](https://www.facebook.com/profile.php?id=61559154729457)  
__Discord__  
[![resized_image_150x150](https://github.com/user-attachments/assets/93ecd098-3391-45bb-9d80-b166c197a475)](https://discord.gg/VQspWyck)  

__源码改自于__
[M5StickCPlus](https://github.com/Sarah-C/M5StickC_Plus_Slot_Machine)


