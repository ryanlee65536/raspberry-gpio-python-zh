# 第四章 PWM
创建PWM实例：
```
p = GPIO.PWM(channel, frequency)
```
启动PWM：
```
p.start(dc)   # dc为占空比 (0.0 <= dc <= 100.0)
```
改变已创建的PWM通道的工作频率：
```
p.ChangeFrequency(freq)   # freq为新的工作频率，单位为Hz
```
改变已创建的PWM通道的占空比：
```
p.ChangeDutyCycle(dc)  # 0.0 <= dc <= 100.0
```
停止PWM：
```
p.stop()
```
请注意，如果实例变量“p”超出范围，PWM也将停止。

## 每两秒钟闪烁一次LED的示例：
```
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)

p = GPIO.PWM(12, 0.5)
p.start(1)
input('Press return to stop:')   # 在Python 2请使用raw input函数
p.stop()
GPIO.cleanup()
```
## LED呼吸灯的示例：
```
import time
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(12, GPIO.OUT)

p = GPIO.PWM(12, 50)  # channel=12 frequency=50Hz
p.start(0)
try:
    while 1:
        for dc in range(0, 101, 5):
            p.ChangeDutyCycle(dc)
            time.sleep(0.1)
        for dc in range(100, -1, -5):
            p.ChangeDutyCycle(dc)
            time.sleep(0.1)
except KeyboardInterrupt:
    pass
p.stop()
GPIO.cleanup()
```
