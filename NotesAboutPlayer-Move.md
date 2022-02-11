## 物件移动方法 

\* 关于坐标系 

- Unity2D世界坐标x轴向右y轴向上z轴向里（左手螺旋） 
- 游戏物件旋转时会带着它自身坐标系旋转 
- 子物体的坐标为它在**父物体自身坐标系**下的坐标 

### 使用Transform组件移动（与物理系统无关） 

（1）`Transform组件.Translate(new Vector3(deltaX,deltaY,deltaZ),坐标系);`写在Update()函数中 

这里deltaX指Update()函数每刷一次间隔时间内x方向的位移量，deltaX = Time.deltaTime * speedX 

最后一项坐标系填作`Space.World`代表按世界坐标系的x，y，z方向进行这个变换，若填作`Space.Self`则按此物件自身坐标系的x，y，z方向进行这个变换（默认值为`Space.Self`） 
 
（2）`Transform组件.position += new Vector3(deltaX, deltaY, deltaZ);` 

与Translate的方法看似大同小异，实则（从坐标系的角度看）差异很大，这个方法直接修改position的值，相当于是在该物体父物体的自身坐标系（没有父物体则为世界坐标系）下变换

\* （3）物件的转动：`Transform组件.Rotate(xAngle, yAngle, zAngle,坐标系);` 

最后一项“坐标系”决定了绕哪个坐标系的x，y，z轴进行旋转

more：四元数、欧拉角…… 

### 使用物理系统移动 

（1）施加力：`刚体组件.Addforce(xForce, yForce, zForce);` 

（2）施加速度：`刚体组件.velocity = new Vector(xVelocity, yVelocity, zVelocity);` 

### 使用Unity现成的CharactorController 

## 移动脚本示例 

```C#

```
