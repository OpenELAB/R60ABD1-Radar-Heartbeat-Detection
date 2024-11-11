&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__Hi there!__ 👋 In this article, we'll guide you step-by-step in exploring how to use the R60ABD1 Radar Module 📡, helping you quickly master its installation, configuration, and core functional applications 🎉. With this module, you can achieve accurate monitoring of human heart rate 💓, respiration 💨, location 📍, and body movement parameters, making it ideal for smart home 🏠 and medical care 🏥 applications. The following steps 📜 will take you deeper into the source code, enabling you to easily get started with this project. Ready? Let’s dive in 🚀!
 
- 📝 Project Introduction
- ✨ Functional Features
- 🏗 Project Code
- 🚀 Installation and Operation
- 🔧 Instructions for Use

# Project R60ABD1
## 📝Project Profile
The R60ABD1 Radar Module is a high-tech sleep monitoring device based on 60GHz millimeter-wave radar technology 🛏️, designed to monitor human heart rate 💓, respiration 💨, position 📍, and body movement parameters. With precise millimeter-wave detection technology, the R60ABD1 ensures reliable data collection regardless of environmental factors, making it ideal for applications in smart home 🏠 and medical care 🏥. The module supports multiple communication protocols such as Zigbee, Wi-Fi, and 4G, and can be easily adapted to diverse needs through various mounting methods, allowing users to achieve real-time and accurate human monitoring. Whether for family health monitoring or medical assistance, the R60ABD1 makes it easy!

## ✨Functional Features
- 📡 Multi-Antenna Design: Utilizing a layout with one transmitter and three receiver antennas, the wide beam design is optimized for top mounting. Through algorithmic control of the detection angle, it accurately scans the full range of human body movements, delivering comprehensive monitoring data.
- 💡 Multi-Function Detection: Capable of sleep monitoring, respiration 💨, and heart rate 💓 detection. The monitoring range extends up to 1.5 meters for sleep quality monitoring and 0.4–2 meters for heart rate and respiration monitoring, accommodating various human monitoring needs.
- 🌍Strong Environmental Adaptability: The module maintains accurate monitoring under various conditions, remaining unaffected by external factors such as temperature, humidity, noise, airflow, dust, and light.
- 🔗Rich Communication Interfaces: Supports Zigbee, Wi-Fi, 4G, and other communication protocols, making it easy to integrate with other devices and enabling efficient data transmission.
- ⚙️Flexible Installation: Compatible with various installation methods, including overhead, angled, and horizontal setups. For optimal monitoring of respiration and heart rate, an installation height of around 2.75 meters is recommended.
- 🔋 Low Power Consumption and Compact Size: The module operates at only 5V/93mA, with a compact size (≤35mm × 31mm × 7.5mm), making it ideal for applications in smart home and medical equipment where energy efficiency and space are essential.
## 🏗 Project Code
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
## 🚀Installation and operation

### precondition
Software dependencies: __Raspberry Pi 4B__, etc. 
Hardware requirements: __R60ABD1__, Dupont cable, OpenELAB expansion board, etc. 

### Pinout and Wiring
| Interface   | Pin | Description | Typical | Description       | 
|-------------|-----|-------------|--------|--------------------|
| Interface 1 | 1   | 5V          | 5.0V   | Power Inputs       |
| Interface 1 | 2   | GND         |        | Ground             | 
| Interface 1 | 3   | RX          | 3.3V   | Serial Receive     |
| Interface 1 | 4   | TX          | 3.3V   | Serial Transmit    |
| Interface 1 | 5   | GP2         |        | Spare Expansion Pin|
| Interface 1 | 6   | GP1         |        | Spare Expansion Pin|
| Interface 2 | 1   | 3V3         | 3.3V   | Power Inputs       |
| Interface 2 | 2   | GND         |        | Ground             | 
| Interface 2 | 3   | SL          |        | Reserve            |
| Interface 2 | 4   | SD          |        | Reserve            |
| Interface 2 | 5   | GP3         |        | Spare Expansion Pin|
| Interface 2 | 6   | GP4         |        | Spare Expansion Pin|
| Interface 2 | 7   | GP5         |        | Spare Expansion Pin|
| Interface 2 | 8   | GP6         |        | Spare Expansion Pin|

![image](https://github.com/user-attachments/assets/178e2533-651c-471a-a1c7-2bca8356a97d)
![image](https://github.com/user-attachments/assets/d0230453-d59e-4c60-8469-3d6b54d33c6d)

### concrete step
1. First, we need to enable the Raspberry Pi's serial port. Click on the Raspberry Pi icon in the upper-left corner, then go to Preferences > Raspberry Pi Configuration. Select the Interfaces tab and enable SSH, VNC, and Serial Port. Ensure Serial Console is disabled, then click OK.
![image](https://github.com/user-attachments/assets/ce31ceba-b602-4984-ab16-e0ee4c2f8b5f)
![image](https://github.com/user-attachments/assets/5a8ba9aa-8c4c-4084-a3fb-1f435c2ede71)
2. Install the serial port and enable the mini UART serial port by running sudo raspi-config. Navigate to Interfacing Options, then select Serial. When prompted, select No to disable the login shell over serial, then Yes to enable the serial hardware.
![image](https://github.com/user-attachments/assets/2e98034b-3772-44e6-8f00-c491b5a4c2ce)
![image](https://github.com/user-attachments/assets/82cb96ec-15f5-493e-8c44-335f88bc0281)
![image](https://github.com/user-attachments/assets/4e14690c-863b-442b-a0a2-d1659b25c94e)
![image](https://github.com/user-attachments/assets/cc9f5364-eaf2-4332-90d1-6b6a470a9a88)
3. Edit the file sudo nano /boot/firmware/config.txt Add dtoverlay=pi3-miniuart-bit at the end, press ctrl+x to exit and save it
![image](https://github.com/user-attachments/assets/f90a38bb-3b20-44ce-8fe2-076a439a065f)
![image](https://github.com/user-attachments/assets/5c2aef05-2506-4c5c-af4b-8d350365534b)
4. Open our source code by typing geany /Desktop/heart.py in the input box
![image](https://github.com/user-attachments/assets/d629ab57-77e2-489b-be3a-fc1dbdb9fa8f)
5. Paste the source code into our heart.py and compile it to start running
![image](https://github.com/user-attachments/assets/a5db568d-88f4-4a85-85ab-45d748a7facd)
6. Demonstration
https://github.com/user-attachments/assets/f3ac3ad3-3289-4292-aa3b-e998bf6bffe6
## How to contact the maintainer or developer
__OpenELAB:__   
[![OpenELAB_logo_resized_150](https://github.com/user-attachments/assets/5d3de375-359c-46a3-96bb-aaa211c6c636)](https://openelab.io)  
__YouTube:__  
[![youtube_logo_200x150](https://github.com/user-attachments/assets/d2365e7f-4ffe-4124-bf62-21eba19a71e4)](https://www.youtube.com/@OpenELAB)  
__X :__  
[![X_logo_150x150](https://github.com/user-attachments/assets/4ad5095f-2573-4791-9360-b355530093bf)](https://twitter.com/openelabio)  
__FaceBook:__  
[![facebook_logo_cropped_150x150](https://github.com/user-attachments/assets/52f2dc9a-a564-49a5-b72e-30eafbbc281f)](https://www.facebook.com/profile.php?id=61559154729457)  
__Discord:__  
[![resized_image_150x150](https://github.com/user-attachments/assets/93ecd098-3391-45bb-9d80-b166c197a475)](https://discord.gg/VQspWyck)  


