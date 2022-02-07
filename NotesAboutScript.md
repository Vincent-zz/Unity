## Get GameObject & Component（获取游戏物件及其组件） 

0、`public`获取 

将组件类型的成员变量定义为`public`，在Unity中直接拖拽获取 

1、`Getcomponent<T>()`获取组件 

script的类继承了MonoBehaviour的`Getcomponent<T>()`来获取 **此脚本挂载的游戏物件** 的组件 

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

2、`游戏物件.Getcomponent<T>()`获取某个游戏物件的组件 

\*首先辨明`gameObject`与`GameObject`的区别： 

~显然，在于开头的大小写~ **单纯的**`gameObject`在脚本中的意思就是**此脚本所挂载的游戏物件**，例如在脚本中添加`Destroy(gameObject);`，执行时将会删除脚本所挂载的实例；而`GameObject`是游戏物件的类名，`Hierarchy`窗口中的所有东西都是`GameObject`的实例，在脚本中也可以定义`GameObject`类的成员变量，如`public GameObject 变量名`，再在Unity中将prefab拖拽到此处。 

**游戏物件的获取** ： 

当前游戏物件： 

即`gameObject`; 

获取特定的游戏物件： 

（1）`GameObject.Find("游戏物件名")`获取（`Find()`函数应该是`GameObject`类的静态函数） 

也可以指定路径，例如`GameObject.Find("游戏物件名/子游戏物件名/子游戏物件的子游戏物件名")`，可以减小开销 

（2）`transform.Find("游戏物件名")`用于获取子物体的Transform组件，例如每一个clone出来的游戏物件enemy都有叫firepoint的子物体来表示子弹生成的位置（开火位置）， 

要获取firepoint的Rigidbody2D示例：

```C#
...
private Rigidbody2D rig;
...
void Start(){
  rig = transform.Find("firepoint").gameObject.GetComponent<Rigidbody2D>();
}
...
``` 

\*关于`transform`与`Transform`以及`transform.gameObject`，`transform.gameObject`的套娃调用： 



## Function（函数） 

1、`void Start()` 

挂载此脚本的游戏物件一旦在场景中生成，将会调用一遍`Start()`函数 

2、`void Update()`与`void FixedUpdate()` 

3、`//碰撞` 

4、`//`
## Input（输入） 
