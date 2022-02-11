## 物件移动方法 

\* 关于坐标系 

- Unity2D场景上的“原始坐标系”：x轴向右y轴向上z轴向里（左手螺旋）
- 以物件为原点的两个坐标系：世界坐标系（轴向与“原始坐标系”相同），自身坐标系（初始轴向与世界坐标系重合，会随旋转改变）
- 子物件的坐标（position值）为它在**父物件自身坐标系**下的坐标 
- 物件的转动：
`Transform组件.Rotate(xAngle, yAngle, zAngle,坐标系);`最后一项“坐标系”填作`Space.World`代表按世界坐标系的轴进行旋转，若填作`Space.Self`则按此物件自身坐标系的轴进行旋转（默认值为`Space.Self`） 

\* 由于此操作不改变子子孙孙物件的position值，所以看起来是子子孙孙物件极其自身坐标系与父物件极其自身坐标系“绑定”在一起绕父物件原点旋转

more：四元数、欧拉角……   

### 使用Transform组件移动（与物理系统无关） 

（1）`Transform组件.Translate(new Vector3(deltaX,deltaY,deltaZ),坐标系);`写在Update()函数中 

这里deltaX指Update()函数每刷一次间隔时间内x方向的位移量，deltaX = Time.deltaTime * speedX 

最后一项坐标系填作`Space.World`代表按世界坐标系进行这个变换，若填作`Space.Self`则按此物件自身坐标系进行这个变换（默认值为`Space.Self`） 
 
（2）`Transform组件.position += new Vector3(deltaX, deltaY, deltaZ);` 

与Translate的方法看似大同小异，实则（从坐标系的角度看）差异很大，这个方法直接修改position的值，相当于是在该物体**父物体的自身坐标系**（没有父物体则为“原始坐标系”）下变换

### 使用物理系统移动 

（1）施加力（世界坐标方向）：`刚体组件.Addforce(xForce, yForce, zForce);` 

（2）施加速度（世界坐标方向）：`刚体组件.velocity = new Vector(xVelocity, yVelocity, zVelocity);` 

### 使用Unity现成的CharactorController 

## 移动脚本示例 

```C#

```
