情景：在某个动画之后自动播放另一个动画并循环，例如拔枪动画后自动循环播放开枪动画，这样的效果则需要判断第一个动画播放完的时刻 

法一：帧事件 

拔枪动画animation窗口选中最后一帧添加事件，inspector窗口中输入该事件执行的函数名（以及参数、来源的脚本等），在此情景下该函数的作用是控制animator播放射击动画 

法二：animator的API函数 


---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)