## Monobehaviour生命周期 

[盗图](https://upload-images.jianshu.io/upload_images/5408097-dcc261ee317a816e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1029/format/webp) 

运行机制：可以看作魔法搜索每个挂载在场景中物体上的脚本，找到其中的特殊名字的函数\[Start()、Update()等（子类优先），这些函数Monobehaviour类中是本没有的，并不是虚函数、抽象类、重写等机制而是magic\]，将这些函数按生命周期整理排列，然后依次执行 

## Function（函数） 

- `void Start()` 

挂载此脚本的游戏物体一旦在场景中生成，将会调用一遍`Start()`函数 

- `void Update()` 

刷新的时间`Time.deltaTime`不固定 

- `void FixedUpdate()` 

刷新的时间`Time.fixedDeltaTime`固定（可在edit-project setting-time中修改），常用于update物理系统相关代码 

- 关于碰撞见[碰撞检测](https://github.com/Vincent-zz/Unity/blob/main/NotesAboutCollision.md) 

\* `void Update()`与`void FixedUpdate()`的区别： 

看了很多资料，似乎仍然不太懂（捂脸 

## Input（输入） 

**GetKey** 

示例： 

`Input.GetKey(KeyCode.Space)` 按住空格时为true 

`Input.GetKeyDown(KeyCode.W)` 按下W时为true 

`Input.GetKeyUp(KeyCode.S)` 松开S时为true 

**GetButton**虚拟按钮 

将上面的Key替换为Button即可，虚拟按钮可在edit - project settings - input manager中设置 

示例：`Input.GetButtonDown("Fire1")`按下Fire1对应的键时为true 

**GetAxis**虚拟轴 

## Debug 

常用：`Debug.Log();`通过输出debug 

射线检测时画线：`Debug.DrawRay(起始position, 方向向量, 颜色, 持续时间);` 

`Debug.DrawLine(起始position, 结束position, 颜色, 持续时间);` 

### 更多关于代码的笔记： 

[C#继承](https://github.com/Vincent-zz/Unity/blob/main/AboutC%23Inheritance.md) 

[更多C#语法](https://github.com/Vincent-zz/Unity/blob/main/AboutC%23.md)
---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)