# 第五章 gpio_function（通道）
## 显示GPIO通道的功能。
例如：
```
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BOARD)
func = GPIO.gpio_function(pin)
```
将返回以下值：
GPIO.IN，GPIO.OUT，GPIO.SPI，GPIO.I2C，GPIO.HARD_PWM，GPIO.SERIAL，GPIO.UNKNOWN
