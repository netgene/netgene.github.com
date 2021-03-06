---
layout: post
title: Raspberry Pi & OBD & Sensors
date: 2016-12-18 20:20:20
categories:
- 折腾吧
tags:
- Raspberry Pi
---

### Raspberry Pi

> Raspberry Pi，中文名为树莓派，是一款基于Linux的单板机电脑。它是由英国的树莓派基金会所开发，并于2012年正式发布，鉴于其极易获得，开源、Python开发等优点，目前树莓派不论在嵌入式领域还是在物联网领域都是最流行的一款开发平台。  
> 本文采用树莓派3 代B 型板进行相关的测试工作。树莓派3代B型板拥有1.2GHz 四核 Broadcom BCM2837 64 位 ARMv8 处理器，板载低功耗蓝牙 (BLE)，板载 BCM43438 WiFi，4 个 USB 2 端口，1 GB RAM，40 针扩展 GPIO，HDMI 和 RCA 视频输出。40针扩展的GPIO，可以方便的扩展加入传感器。


![Raspberry-Pi-3-Flat-Top](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Raspberry-Pi-3-Flat-Top.jpg/1280px-Raspberry-Pi-3-Flat-Top.jpg)

![gpio](https://www.element14.com/community/servlet/JiveServlet/previewBody/68203-102-6-294412/GPIO.png)

### OBD

```python
from obdython import Device, OBDPort  
import time  
dev = Device(Device.types['bluetooth'], bluetooth_mac="AA:BB:CC:11:22:33", bluetooth_channel=1)  
port = OBDPort(dev)  
time.sleep(0.1) # Program needs to wait for adapter to come online  
port.connect()  
time.sleep(0.1)  
port.ready()  
print(port.sensor('rpm'))  
```

### GPS

```python
```

### 温湿度DHT11

> DHT11采用串行接口单线双向传输，数字线DATA用于与DHT11的通信和同步，采用单总线数据格式，一次通信时间4ms左右，一次完整数据传输为40位，数据分为整数和小数部分。  
> 8bit湿度整数数据|8bit湿度小数数据|8bit温度整数数据|8bit温度小数数据|8bit校验和

```python
#!/usr/bin/python
import RPi.GPIO as GPIO
import time
 
channel =4
data = []
j = 0
 
GPIO.setmode(GPIO.BCM)
 
time.sleep(1)
 
GPIO.setup(channel, GPIO.OUT)
GPIO.output(channel, GPIO.LOW)
time.sleep(0.02)
GPIO.output(channel, GPIO.HIGH)
GPIO.setup(channel, GPIO.IN)
 
while GPIO.input(channel) == GPIO.LOW:
  continue
while GPIO.input(channel) == GPIO.HIGH:
  continue
 
while j < 40:
  k = 0
  while GPIO.input(channel) == GPIO.LOW:
    continue
  while GPIO.input(channel) == GPIO.HIGH:
    k += 1
    if k > 100:
      break
  if k < 8:
    data.append(0)
  else:
    data.append(1)
 
  j += 1
 
print "sensor is working."
print data
 
humidity_bit = data[0:8]
humidity_point_bit = data[8:16]
temperature_bit = data[16:24]
temperature_point_bit = data[24:32]
check_bit = data[32:40]
 
humidity = 0
humidity_point = 0
temperature = 0
temperature_point = 0
check = 0
 
for i in range(8):
  humidity += humidity_bit[i] * 2 ** (7-i)
  humidity_point += humidity_point_bit[i] * 2 ** (7-i)
  temperature += temperature_bit[i] * 2 ** (7-i)
  temperature_point += temperature_point_bit[i] * 2 ** (7-i)
  check += check_bit[i] * 2 ** (7-i)
 
tmp = humidity + humidity_point + temperature + temperature_point
 
if check == tmp:
  print "temperature :", temperature, "*C, humidity :", humidity, "%"
else:
  print "wrong"
  print "temperature :", temperature, "*C, humidity :", humidity, "% check :", check, ", tmp :", tmp
 
GPIO.cleanup()
```

```python
# -*- coding: utf-8 -*-
"""
Created on Sun Jan 26 14:24:43 2014
 
@author: pi
"""
 
import RPi.GPIO as gpio
import time
gpio.setwarnings(False)
gpio.setmode(gpio.BOARD)
time.sleep(1)
data=[]
def delay(i): #20*i usdelay
    a=0
    for j in range(i):
        a+1
j=0
#start work
gpio.setup(12,gpio.OUT)
#gpio.output(12,gpio.HIGH)
#delay(10)
gpio.output(12,gpio.LOW)
time.sleep(0.02)
gpio.output(12,gpio.HIGH)
i=1
i=1
  
#wait to response
gpio.setup(12,gpio.IN)
 
 
while gpio.input(12)==1:
    continue
 
 
while gpio.input(12)==0:
    continue
 
while gpio.input(12)==1:
        continue
#get data
 
while j<40:
    k=0
    while gpio.input(12)==0:
        continue
     
    while gpio.input(12)==1:
        k+=1
        if k>100:break
    if k<3:
        data.append(0)
    else:
        data.append(1)
    j+=1
 
print "Sensor is working"
#get temperature
humidity_bit=data[0:8]
humidity_point_bit=data[8:16]
temperature_bit=data[16:24]
temperature_point_bit=data[24:32]
check_bit=data[32:40]
 
humidity=0
humidity_point=0
temperature=0
temperature_point=0
check=0
 
 
 
for i in range(8):
    humidity+=humidity_bit[i]*2**(7-i)
    humidity_point+=humidity_point_bit[i]*2**(7-i)
    temperature+=temperature_bit[i]*2**(7-i)
    temperature_point+=temperature_point_bit[i]*2**(7-i)
    check+=check_bit[i]*2**(7-i)
 
tmp=humidity+humidity_point+temperature+temperature_point
if check==tmp:
    print "temperature is ", temperature,"wet is ",humidity,"%"
else:
    print "something is worong the humidity,humidity_point,temperature,temperature_point,check is",humidity,humidity_point,temperature,temperature_point,check
```

### PM2.5 SDS011

> SDS011采用串口通讯协议：96008N1。	串口上报通讯周期1.5秒。  
> 数据帧10字节包括报文头、指令号、数据(6字节)、校验和、以及报文尾。	  
> PM2.5计算：PM2.5(ug/m3) = ((PM2.5高字节 * 256) + PM2.5低字节) / 10。  


```python
#!/usr/bin/python
import serial
import time
  
ser = serial.Serial('/dev/ttyAMA0', 9600)  
ser.setTimeout(1.5)

def get_pm25():
    while True:
        ser.flushInput()
        time.sleep(0.5)
        result = ser.read(10)
    
        if len(result) == 10:
            if(result[0] == 0xaa and result[1] == 0xc0):
                checksum = 0
                for i in range(6):
                    checksum = checksum + int(result[2+i])
    
                if checksum % 256 == result[8]:
                    pm25 = (int(result[3]) * 256 + int(result[2])) / 10.0
                    print("pm2.5:%.1f" %pm25)

if __name__ == '__main__':  
    get_pm25()
```