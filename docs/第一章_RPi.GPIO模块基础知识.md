# 第一章 RPi.GPIO模块基础知识

## 导入模块
要导入RPi.GPIO模块：
```
import RPi.GPIO as GPIO
```
通过这种方式，您可以在这个程序脚本的其余部分将其称为GPIO。
可以通过抛出异常来检查导入模块是否成功：
```
try:
    import RPi.GPIO as GPIO
except RuntimeError:
print("Error importing RPi.GPIO!  This is probably because you need superuser privileges.  You can achieve this by using 'sudo' to run your script")
```
## 引脚编号
在RPi.GPIO中，有两种方法可以对Raspberry Pi上的IO引脚进行编号。第一种是使用BOARD编号系统。这是指Raspberry Pi板P1头上的引脚编号。使用此编号系统的优势在于，无论RPi的主板版本如何，您的硬件都将始终有效。您无需重新连接连接器或更改代码。
第二个编号系统是BCM编号。这是一种较低级别的工作方式 - 它指的是Broadcom SOC上的通道号。您必须始终使用哪个通道编号指向Raspberry Pi板上哪个引脚的图表。您的程序脚本可能会在Raspberry Pi板的修订版之间被破坏。
```
GPIO.setmode(GPIO.BOARD)
    # 或
GPIO.setmode(GPIO.BCM)
```
要检测已设置的引脚编号系统（例如，在多个Python模块使用GPIO）：
```
mode = GPIO.getmode()
```
模式的值为10（GPIO.BOARD），11（GPIO.BCM）或为空（未设置）。

## 警告
您可能在Raspberry Pi的GPIO上有多个程序脚本/电路。因此，如果RPi.GPIO检测到某个引脚已配置为默认值（输入）以外的其他内容，则在再次尝试配置脚本时会收到警告。要禁用这些警告：
```
GPIO.setwarnings(False)
```

## 配置一个通道（引脚）
您需要将您正在使用的每个通道设置为输入通道或输出通道。要将通道配置为输入：
```
GPIO.setup(channel, GPIO.IN)
```
（其中channel是基于您指定的编号系统（BOARD或BCM）的通道编号）。
有关设置输入通道的更多高级信息，请参见[此处]()。
要将通道设置为输出：
```
GPIO.setup(channel, GPIO.OUT)
```
（其中channel是基于您指定的编号系统（BOARD或BCM）的通道编号）。
您还可以为输出通道指定初始值：
```
GPIO.setup(channel, GPIO.OUT, initial=GPIO.HIGH)
```

## 使用一条指令设置多个通道
您可以为每个呼叫设置多个通道（从0.5.8开始）。例如：
```
chan_list = [11,12]    # 设置一个列表并添加任意数量的通道！
                   # 您也可以将列表改为元组，即：
                   #		chan_list = (11,12)
GPIO.setup(chan_list, GPIO.OUT)
```

## 输入
要读取GPIO引脚的值：
```
GPIO.input(channel)
```
（其中channel是基于您指定的编号系统（BOARD或BCM）的通道编号）。这将返回0 / GPIO.LOW / False或1 / GPIO.HIGH / True。

## 输出
要设置GPIO引脚为输出状态：
```
GPIO.output(channel, state)
```
（其中channel是基于您指定的编号系统（BOARD或BCM）的通道编号）。
状态可以是0 / GPIO.LOW / False或1 / GPIO.HIGH / True。

## 输出到多个通道
您可以在同一个指令中对多个通道设置输出（从0.5.8开始）。例如：
```
chan_list = [11,12]                             # 也可以将列表改为元组
GPIO.output(chan_list, GPIO.LOW)                # 设置所有通道为 GPIO.LOW
GPIO.output(chan_list, (GPIO.HIGH, GPIO.LOW))     # 设置第一个通道为高电平，第二个通道为低电平
```

## 清理已配置的引脚状态
在任何程序脚本的最后，最好对您可能使用过的任何资源进行清理。这在RPi.GPIO的使用中也没有什么不同。将所有使用过的通道返回到没有上拉/下拉的输入，可以通过短接引脚来避免意外损坏RPi。请注意，这只会清理程序脚本使用过的GPIO通道。请注意，GPIO.cleanup（）也会清除正在使用的引脚编号系统。
在程序脚本结束时对引脚状态进行清理：
```
GPIO.cleanup()
```
当程序退出时，可能不希望清理每个通道而留下一些设置。您可以清理单个通道，或者使用列表或通道元组对多个通道进行清理：
```
GPIO.cleanup(channel)
GPIO.cleanup( (channel1, channel2) )
GPIO.cleanup( [channel1, channel2] )
```

## Raspberry Pi板信息和RPi.GPIO版本
要查看有关您的Raspberry Pi板的信息：
```
GPIO.RPI_INFO
```
要查看Raspberry Pi板修订版：
```
GPIO.RPI_INFO['P1_REVISION']
GPIO.RPI_REVISION    （已弃用）
```
要查看RPi.GPIO的版本：
```
GPIO.VERSION
```
