## 物体移动方法 

\* **关于坐标系** 

- Unity2D场景上的“原始坐标系”：x轴向右y轴向上z轴向里（左手螺旋）
- 以物体为原点的两个坐标系：世界坐标系（轴向与“原始坐标系”相同），自身坐标系（初始轴向与世界坐标系重合，会随旋转改变）
- 子物体的坐标（position值）为它在**父物体自身坐标系**下的坐标 
- 获取方向的简便方式：`transform.right`自身坐标系x轴方向，`transform.up`...y轴方向，`transform.forward`...z轴方向；将transform替换为Vector3则表示世界坐标的坐标轴方向
  
### 物体的转动

**Rotate函数旋转** 

`Transform组件.Rotate(xAngle, yAngle, zAngle,坐标系);` 

最后一项“坐标系”默认值为`Space.Self`，即按此物体自身坐标系的轴进行旋转，若填作`Space.World`则代表按世界坐标系的轴进行旋转 

**四元数（Quaternion）** 

Transform组件的Inspector窗口中Rotation是以欧拉角表示的（x, y, z），但Unity内部以四元数储存旋转（x, y, z, w），四元数涉及了一些beyond me的数学知识\[捂脸\]，正如Unity官方文档所述“除非您十分了解四元数，否则不要直接进行此种修改。” 

不过仍然可以通过欧拉角表示旋转，例如：`transform.rotation = Quaternion.Euler(xAngle, yAngle, zAngle);`（P.S.`Quaternion Euler(float , float , float )`是Quaternion类的静态函数）

\* 由于旋转操作不改变子子孙孙物体的position值，所以看起来是子子孙孙物体极其自身坐标系与父物体极其自身坐标系“绑定”在一起绕父物体原点旋转

### 使用Transform组件移动

\* （与物理系统无关，会与物理系统发生冲突） 

（1）`Transform组件.Translate(new Vector3(deltaX,deltaY,deltaZ),坐标系);`写在Update()函数中 

这里deltaX指Update()函数每刷一次间隔时间内x方向的位移量，deltaX = Time.deltaTime * speedX 

最后一项坐标系默认值为`Space.Self`，即按此物体自身坐标系进行这个变换，若填作`Space.World`则代表按世界坐标系进行这个变换 

（2）`Transform组件.position += new Vector3(deltaX, deltaY, deltaZ);` 

与Translate的方法看似大同小异，实则（从坐标系的角度看）差异很大，这个方法直接修改position的值，相当于是在该物体*父物体的自身坐标系*（没有父物体则为“原始坐标系”）下变换

### 使用物理系统移动

\* （比较推荐） 

（1）施加力：`刚体组件.Addforce(力向量);` 

（2）施加速度：`刚体组件.velocity = 速度向量;` 

\* 用Transform移动刚体与这个刚体的物理系统实际上是有冲突的，最好只用物理系统移动刚体 

### 使用Unity现成的CharactorController 

…… 

## 移动脚本示例 

```C#

```

---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)