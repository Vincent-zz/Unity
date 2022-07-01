### 几个常见概念 
- UI -user interface
- GUI -graphical UI
- UGUI -unity GUI
- NGUI -unity的一个UI插件（next-gen UI）
- FGUI -[fairyGUI](https://fairygui.com/)，UI编辑器
### UGUI 

**关于Rect Transform** 

锚点（Anchors）/中心（Pivot）位置以父物体/物体左下角为原点，父物体/物体长高分别为横纵单位的坐标中表示（Anchors中max、min的x、y值定位四个小花瓣构成的矩形） 

中心：缩放中心-缩放时中心与各边界距离之比不变；旋转中心-旋转轴所在 

锚点：有前面的定位方式可知锚点会随父物体变换而变化，物体矩形外框的各边与锚点花瓣矩形对应边距离保持不变，物体按照这个规律随父物体变换而变换（常用形式：四个花瓣重合，则物体与花瓣相对位置不变且大小不随父物体缩放变化） 

**控件** 

- Canvas：放置UI的画布，所有UI物件都将作为Canvas的子物体
- Panel：放置UI，统一管理有一定相对位置关系的UI，类似于二级canvas
- Text/TextMeshPro：文字UI
- 
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)