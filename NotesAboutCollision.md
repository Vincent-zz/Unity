## Collision & Trigger

(碰撞或触发的条件详见先前的[Physics2D]()) 

### Layer, Sorting Layer, Tag三者的区别 

- Sorting Layer：位于Sprite Renderer组件中，管理的是渲染的显示层级，同一个Sorting Layer的游戏物件的显示层级由Order in Layer的大小决定  

- Layer：位于inspector窗口右上角，可应用于碰撞检测与射线检测，是LayerMask类的实例 

获取：`游戏物件.layer`

- Tag： 用于标识游戏物件，可以通过tag查找游戏物件以及在碰撞时标识物体 

获取：`此游戏物件的任意组件.tag`或`游戏物件.tag` 

### 碰撞检测方法 

**1、碰撞发生时调用的函数** 

 
**2、使用LayerMask**（不妨称它“图层罩”，原谅我的乡土翻译） 

检测某个碰撞体与指定图层中所有碰撞体的碰撞：`碰撞体.IsTouchingLayers(图层罩)`或`Physics2D.IsTouchingLayers(碰撞体, 图层罩)`返回bool值  

\* （无论碰撞或触发都算is touching layer，都会返回true） 

示例： 

```C#
    ...
    private CapsuleCollider2D coll;
    public LayerMask ground;//在Unity中选择ground对应的图层
    private bool isGround;
    
    void Start()
    {
        coll = GetComponent<CapsuleCollider2D>();
        ...
    }
    void Update()
    {
        if (coll.IsTouchingLayers(ground))//等价于if(Physics2D.IsTouchingLayers(coll, ground))
        {
            isGround = true;
        }
        else
        {
            isGround = false;
        }
        ...
    }
``` 

\* 另外还有`刚体.IsTouchingLayers(图层罩)`可以检测此刚体上所有碰撞体与指定图层中所有碰撞体的碰撞，代码类似 

**3、射线检测** 

Physics2D.Linecast 
