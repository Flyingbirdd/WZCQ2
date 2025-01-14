# AI玩手机游戏
 ## 说明
一、运行环境win10；win7未测试，估计是可以，还需要添加 PyQt5模块用于截图参考（requirements.txt）。  
环境配置参考视频 1  
链接：https://pan.baidu.com/s/1fJRyX-scxbeOJ2lsddTLiA   
提取码：msr5  
二、需要1060或以上算力的显卡。  
三、需要一台打开安卓调试并能玩王者荣耀的手机。  
四、需要下载[scrcpy](https://github.com/Genymobile/scrcpy/blob/master/README.zh-Hans.md)  的windows版本。 把所有文件解压到项目根目录即可（这是我的笨办法） 。  
五、pyminitouch库运行时会自动安装minitouch。如果无法自动安装则需要手动安装[minitouch](https://github.com/openstf/minitouch) ，比较麻烦。  
还有，minitouch不支持Android10及以上系统  

## 运行游戏AI
  
1. 下载预训练模型
你可以通过以下链接下载训练过的模型：

Google 云盘
百度网盘 （提取码：oiar）
下载完成后，将模型文件放入 weights 文件夹。

注意： 如果需要加载不同的模型，请修改 模型_策略梯度.py 文件中的第 261 行。
2. 启动 scrcpy
运行脚本 “启动和结束进程.py” 启动 scrcpy。
3. 启动王者荣耀并进入5v5人机对战
启动 王者荣耀 游戏并进入 5v5 人机对战模式。
然后运行脚本 “训练数据截取_A.py” 开始收集训练数据。
## 生成训练数据（半自动）
运行 “训练数据截取_A.py” 这时就可以生成训练用的数据。  
按"i"键则结束或则是重新运行  
按键'w' 's ' 'a' 'd'控制方向  左、下、右箭头对应是1、2、3技能，上箭头长按则攻击。其它按键请参考源码。   
注意！！ 如果用按键控制则会记录按键操作数据，否则会记录AI玩游戏的数据。  
根据我的经验，随着模型训练次数增加，手动干预的次数会越来越小。但总体来说训练数据的获取依然需要人为干预，因为游戏结束
后要重新开始需要手动操控（我并没有做自动化脚本）。

# 如何训练主模型
一、下载状态判断模型 你可以从[google云盘](https://drive.google.com/file/d/1eqy-xX29sjEguuQI_1m8qaLEX3g4KAQ7/view?usp=sharing) 下载训练过的模型，也可以百度网盘下载  
链接：https://pan.baidu.com/s/1-UCuPutZQck3Iawot9bGrw 
提取码：545t  
后放入weights文件夹下 
二、数据预处理  
将图片用resnet101预处理后再和对应操作数据一起处理后用numpy数组储存备用。  
具体要做的就是运行 “处理训练数据5.py”   
三、训练  
预处理完成以后运行 “训练X.py”即可。  
注意！模型保存路径 在 模型_策略梯度.py 295和296行更改。  
# 如何训练状态判断模型
状态判断模型概述
状态判断模型实际上是一个图像分类神经网络，结构与主模型基本相同，但参数有所不同。一、获取标注数据
标注数据是在游戏运行过程中进行的。请运行 状态标注.py 脚本进行标注。
标注规则：

Key.left：击杀小兵或野怪，或推掉塔。
Key.down：击杀敌方英雄。
Key.right：被击塔攻击。
Key.up：被击杀。
注意： 标注模型会自动参与数据标注，减轻人工标注的工作负担。

二、校正标注数据
由于前一步获取的标注数据可能不准确，需要手动进行校准。
运行 筛选事件特征图片.py 脚本进行校正。
具体操作参考代码中的第 68 至 81 行，注意：其中“过”表示认同原始标注。三、训练
运行 训练状态判断模型A.py 脚本开始训练状态判断模型。

