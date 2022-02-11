## 物件移动方法 

\* 关于坐标系 

### 使用Transform组件移动（与物理系统无关） 

（1）`Transform组件.Translate(new Vector3(deltaX,deltaY,deltaZ),坐标系);`写在Update()函数中 

这里deltaX指Update()函数每刷一次间隔时间内x方向的位移量，deltaX = Time.deltaTime * speedX 

最后一项坐标系填作`Space.World`代表按世界坐标系进行这个变换，若填作`Space.Self`则按此物件自身坐标系进行这个变换（默认值为`Space.Self`） 
 
（2）`Transform组件.position += new Vector3(deltaX, deltaY, deltaZ);`与Translate的方法大同小异 

\* （3）物件的转动：`Transform组件.Rotate(绕x轴,绕y轴,绕z轴,坐标系)` 

四元数、欧拉角…… 

### 使用物理系统移动 

rigidbody2d addforce velocity 

```C#

```
