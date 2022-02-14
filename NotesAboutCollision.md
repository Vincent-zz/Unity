## Collision & Trigger

(碰撞或触发的条件详见先前的[Physics2D](https://github.com/Vincent-zz/Unity/blob/main/NotesAboutPhysics2D.md)) 

### Layer, Sorting Layer, Tag三者的区别 

- Sorting Layer：位于Sprite Renderer组件中，管理的是渲染的显示层级，同一个Sorting Layer的游戏物件的显示层级由Order in Layer的大小决定  

- Layer：位于inspector窗口右上角，可应用于碰撞检测与射线检测，是LayerMask类的实例 

获取：`游戏物件.layer`

- Tag： 用于标识游戏物件，可以通过tag查找游戏物件以及在碰撞时标识物体 

获取：`此游戏物件的任意组件.tag`或`游戏物件.tag` 

### 碰撞检测方法 

**1、碰撞发生时调用的函数** 

 
**2、使用LayerMask**（不妨乡土翻译为“图层罩”） 

检测某个碰撞体与指定图层中所有碰撞体的碰撞：`碰撞体.IsTouchingLayers(图层罩)`或`Physics2D.IsTouchingLayers(碰撞体, 图层罩)`返回bool值  

\* （无论碰撞或触发都算is touching layer，都会返回true） 

示例： 

```C#
    //运用LayerMask进行触地检测
    ...
    private BoxCollider2D foot;
    public LayerMask ground;//在Unity中选择ground对应的图层
    private bool grounded;
    
    void Start()
    {
        foot = GetComponent<BoxCollider2D>();
        ...
    }
    void Update()
    {
        grounded = foot.IsTouchingLayers(ground));//等价于Physics2D.IsTouchingLayers(foot, ground)
        ...
    }
``` 

\* 另外还有`刚体.IsTouchingLayers(图层罩)`可以检测此刚体上所有碰撞体与指定图层中所有碰撞体的碰撞，代码类似 

**3、射线检测** 

射线定义：`Ray example = new Ray(指定position, 方向向量)` 

射线检测：返回？ 

```C#
RaycastHit hitInfo;//射线碰撞信息
Physics2D.Linecast(射线, out hitInfo , 距离（float）, 指定图层罩);//其中`射线`可由`指定position, 方向向量`代替 
``` 

射线碰撞信息可以没有（重载）；距离，指定图层罩也可以没有（默认） 

示例： 

```C#
    //运用Raycast进行触地检测（向下发射一段很短的射线）
    ...
    public LayerMask ground;//在Unity中选择ground对应的图层
    private bool grounded;

    void Update()
    {
        grounded = Physics2D.Raycast(transform.Find("foot").position, -Vector3.up, 0.01f, ground);//挂载了一个叫foot的空子物件来指示位置
        ...
    }
    ...
```
