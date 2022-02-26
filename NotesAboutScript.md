## Get GameObject & Component（获取游戏物件及其组件） 

### `public`获取 

将组件类型的成员变量定义为`public`，在Unity中直接拖拽获取 

### `Getcomponent<T>()`获取组件 

script类继承了MonoBehaviour类的`Getcomponent<T>()`来获取 **此脚本挂载的游戏物件** 的组件 

（此方法继承自MonoBehaviour所以`Getcomponent<T>()`与`this.Getcomponent<T>()`完全相同）

示例： 

```C#
...
private Rigidbody2D rig;
...
void Start(){
  rig = GetComponent<Rigidbody2D>();
}
...
``` 

注意：`Getcomponent<T>()`一般写在`Start()`函数中，尽量不要在`Update()`函数中使用 

### `游戏物件.Getcomponent<T>()`获取某个游戏物件的组件 

\* 首先辨明`gameObject`与`GameObject`的区别： 

  ~显然，在于开头的大小写~ 脚本中的`gameObject`是一个现成的（继承而来的？）成员，代表了此脚本挂载的游戏物件，无需再通过其他方式获取，类似的，`组件.gameObject`代表了这个组件所挂载的游戏物件；而`GameObject`则是游戏物件的**类名**，Hierarchy窗口中的所有东西都是`GameObject`的实例，在脚本中也可以定义`GameObject`类的成员变量（如`public GameObject 变量名`，再在Unity中将prefab拖拽到此处）。 

 **游戏物件的获取** ： 

当前游戏物件：即`gameObject`; 

获取特定的游戏物件： 

（1）`GameObject.Find("游戏物件名")`获取（`Find()`函数应该是`GameObject`类的静态函数） 

也可以指定路径，例如`GameObject.Find("游戏物件名/子游戏物件名/子游戏物件的子游戏物件名")`，可以减小开销 

\* 找不到被隐藏的物件 

（2）`transform.Find("游戏物件名")`用于获取子物体的Transform组件，例如每一个clone出来的游戏物件enemy都有叫gun的子物体来表示武器， 

要获取gun的Rigidbody2D示例：

```C#
...
private Rigidbody2D rig;
...
void Start(){
  rig = transform.Find("gun").gameObject.GetComponent<Rigidbody2D>();//transform.Find("gun")获取了gun的Transform组件
}
...
``` 

（3）`transform.parent`获取父物体的Transform组件 

 \* `transform`与`Transform`:（类比`gameObject`与`GameObject`）  
 
 `transform`代表了此脚本挂载的游戏物件的Transform组件，与`gameObject`类似，是现成的（毕竟任何一个游戏物件都必须有Transform组件）；`Transform`则是Transform组件的类名
  
 \* `transform`与`gameObject`：
  
`游戏物件.transform` & `组件.gameObject` 使得二者可以套娃调用，比如：`...gameObject.transform.gameObject.transform.gameObject` <=> `gameObject`

## Function（函数） 

- `void Start()` 

挂载此脚本的游戏物件一旦在场景中生成，将会调用一遍`Start()`函数 

- `void Update()` 

刷新的时间`Time.deltaTime`不固定 

- `void FixedUpdate()` 

刷新的时间`Time.fixedDeltaTime`固定（可在edit-project setting-time中修改），常用于update物理系统相关代码 

- 关于碰撞见[碰撞检测]() 

\* `void Update()`与`void FixedUpdate()`的区别： 

看了很多资料，似乎仍然不太懂（捂脸 

## Input（输入） 

## Debug 

常用：`Debug.Log();`通过输出debug 

射线检测时画线：`Debug.DrawRay(起始position, 方向向量, 颜色, 持续时间);` 

`Debug.DrawLine(起始position, 结束position, 颜色, 持续时间);` 

[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)