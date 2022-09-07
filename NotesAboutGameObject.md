## Get GameObject & Component（获取游戏物体及其组件） 

### `public`获取 

将组件类型的成员变量定义为`public`，在Unity中直接拖拽获取 

### `Getcomponent<T>()`获取组件 

script类继承了MonoBehaviour类的`Getcomponent<T>()`来获取 **此脚本挂载的游戏物体** 的组件 

（此方法继承自MonoBehaviour所以`Getcomponent<T>()`与`this.Getcomponent<T>()`完全相同）

示例： 

```C#
...
private Rigidbody2D rig;
...
void Start()
{
  rig = GetComponent<Rigidbody2D>();
}
...
``` 

注意：`Getcomponent<T>()`一般写在`Start()`函数中，尽量不要在`Update()`函数中使用 

### `游戏物体.Getcomponent<T>()`获取某个游戏物体的组件 

\* 首先辨明`gameObject`与`GameObject`的区别： 

  ~显然，在于开头的大小写~ 脚本中的`gameObject`是一个现成的（继承而来的？）成员，代表了此脚本挂载的游戏物体，无需再通过其他方式获取，类似的，`组件.gameObject`代表了这个组件所挂载的游戏物体；而`GameObject`则是游戏物体的**类名**，Hierarchy窗口中的所有东西都是`GameObject`的实例，在脚本中也可以定义`GameObject`类的成员变量（如`public GameObject 变量名`，再在Unity中将prefab拖拽到此处）。 

### 游戏物体的获取 

当前游戏物体：即`gameObject`; 

获取特定的游戏物体： 

（1）`GameObject.Find("游戏物体名")`获取（`Find()`函数应该是`GameObject`类的静态函数） 

也可以指定路径，例如`GameObject.Find("游戏物体名/子游戏物体名/子游戏物体的子游戏物体名")`，可以减小开销 

\* 找不到被隐藏的物体 

（2）`transform.Find("游戏物体名")`用于获取子物体的Transform组件，例如每一个clone出来的游戏物体enemy都有叫gun的子物体来表示武器， 

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
 
 `transform`代表了此脚本挂载的游戏物体的Transform组件，与`gameObject`类似，是现成的（毕竟任何一个游戏物体都必须有Transform组件）；`Transform`则是Transform组件的类名
  
 \* `transform`与`gameObject`：
  
`游戏物体.transform` & `组件.gameObject` 使得二者可以套娃调用，比如：`...gameObject.transform.gameObject.transform.gameObject` <=> `gameObject` 

## 粘贴、销毁、对象池 

### 粘贴：Instantiate 

`Instantiate(游戏物体, 位置矢量, 旋转欧拉角);` 

### 销毁：Destroy 

`Destroy(游戏物体, 时间（float）);`，时间默认为0（然而销毁总是发生在*当前Update结束后*） 

### 对象池 

频繁地粘贴和销毁会吃性能，比如射击游戏就会有很多子弹的粘贴与销毁，那就要采用对象池来解决这个问题 

对象池思路举例：建立一个空物体pool（以子物体形式存放重复对象），第一发子弹发出，由于pool无子物体，所以在玩家枪口位置直接粘贴；然后第二发子弹发出，pool中仍无子物体，所以仍是粘贴；不久后第一发子弹碰到障碍物，并不执行销毁，而是SetActive(false)并将其改为pool的子物体（修改transform.parent）；此时发射第三发子弹，由于此时pool中已有子物体，所以不执行粘贴，而是SetActive(true)并将其位置移到枪口处，实现了子弹的重复利用，大幅减小粘贴次数。 

射击脚本示例： 

```C#
//实现按下空格时玩家射击
...
public GameObject bulletprefab;//Unity中拖入子弹预制体（名为bullet）
public GameObject pool;//拖入对象池
private Transform fp;
void Start()
{
    fp = transform.Find("firepoint");//firepoint是一个空物体，作为玩家子物体来表示开火位置
    ...
}
void Update()
{
    if(Input.GetKeyDown(KeyCode.Space))
    { 
        Transform bulletInPool = pool.transform.Find("bullet(Clone)");
        if(bulletInPool != null)
        {
            bulletInPool.gameObject.SetActive(true);
            bulletInPool = fp;//子弹出池
        }
        else
        {
            Instantiate(bulletprefab, fp.position, fp.rotation);//空池则粘贴子弹
        }
    }
    ...
}
//以下为挂载在子弹预制体上的脚本：
...
public float speed;
void Start()
{
    GetComponent<Rigidbody>().velocity = transform.right * speed; 
    ...
}
void OnTriggerEnter2D(Collider2D coll)
{
    transform.parent = pool.transform;
    gameObject.SetActive(false);//子弹入池
    ...//其他操作，如敌人扣血（当然也可以放在敌人的代码里）
}
``` 
---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)