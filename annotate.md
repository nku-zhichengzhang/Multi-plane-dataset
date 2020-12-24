# 软件基础

需要安装[Labelme](https://github.com/wkentaro/labelme)，基于PyQt且支持Ubuntu/macOS/Windows多平台。<br>
容器可用Anaconda一类python虚拟环境或docker。<br>

    labelme --labels label.txt
简单使用，更多详细的快捷键会在文中介绍。

# 实例标注信息
你需要标注多个场景下的多个视频，视频已经事先处理好为带黑边图片序列。<br>
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="http://qlu3j5vd3.hn-bkt.clouddn.com/candidate.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">一个场景中所有可能的候选instance</div>
</center>
<br>
在同一场景下，不同图片序列中所含的平面实例是大致相同的，但他们不一定都在序列的第一帧中出现，我们会给出每一个场景下所有可能的候选平面实例，再根据选择策略来在图片序列中选择应该被标注的实例。<br><br>

**对每个图片序列，你需要标注：**
- 每一个实例的位置信息
- 每一个实例的类别信息
- 每一个实例的Id信息


## 位置信息
对于每一个平面实例，用一个四边形框住，保证包含所有属于该平面的像素又具有最小的面积。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="http://qlu3j5vd3.hn-bkt.clouddn.com/quardrangle.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">单一一个平面实例的位置信息</div>
</center>

## 类别信息
我们预先给出了一个22类的list (label.txt)，包含室内外场景中我们需要标注的类别。
## Id信息
为图片序列中出现的实例分配一个非负整数(0~N)。
## 选择策略
为了避免主观性，对在图片序列中的每一帧图片中选择实例标注，我们有以下几条细则：
- **What？** 所有标注的实例来自于每个场景给出的候选中。
- **When？** 尽可能早的开始分配实例Id，尽可能晚的结束这个实例Id。
- **遮挡/超出视角！** 指平面对象间或特定物体造成的遮挡/平面对象超出摄像头外。在能通过简单的恒定速度模型推断出实例位置时继续标注并保持Id，否则不进行标注，并在下一次该对象出现时分配新的Id。
- **超出视角的判断！** 对于平面四边形，存在3条及以上的边出现，则进行标注，否则不进行标注并在下一次该对象出现时分配新的Id。

# 标注过程
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="http://qlu3j5vd3.hn-bkt.clouddn.com/annotate.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">每一帧图片的标注方式。其中，object label一栏需要填入平面实例的类别信息，group id需要填入分配给平面实例的Id</div>
</center>

# 快捷键
新建多边形：Ctrl+N
编辑多边形：Ctr+J
撤销：Ctrl+Z
删除多边形：Del
建议设置：自动保存，保留上一张的标注结果