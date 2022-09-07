## Collision & Trigger

(碰撞或触发的条件详见先前的[Physics2D](https://github.com/Vincent-zz/Unity/blob/main/NotesAboutPhysics2D.md)) 

### Layer, Sorting Layer, Tag三者的区别 

- Sorting Layer：位于Sprite Renderer组件中，管理的是渲染的显示层级，同一个Sorting Layer的游戏物体的显示层级由Order in Layer的大小决定  

- Layer：位于inspector窗口右上角，可应用于碰撞检测与射线检测，是LayerMask类的实例 

获取：`游戏物体.layer`

- Tag： 用于标识游戏物体，可以通过tag查找游戏物体以及在碰撞时标识物体 

获取：`此游戏物体的任意组件.tag`或`游戏物体.tag` 

### 碰撞检测方法 

**1、碰撞发生时调用的函数** 

触发（更加常用）为例： 

`OnTriggerEnter2D(Collider2D 变量名)` 

`OnTriggerStay2D(Collider2D 变量名)` 

`OnTriggerExit2D(Collider2D 变量名)` 

很显然，分别代表触发开始、持续中、结束时调用的函数 

Trigger替换为Collision即为碰撞 

\*由于碰撞（触发）必然涉及两个游戏物体，也就是说两个物体各自脚本中的碰撞（触发）函数都会被调用，所以要合理地将碰撞（触发）时应当执行的代码分配给参与碰撞（触发）的两个物体。例如：子弹与敌人触发，将执行两个操作——子弹销毁与敌人扣血。子弹销毁应当写在子弹的代码中（实际上子弹不管碰上什么都要执行销毁）；敌人扣血若写在子弹上则需要if(coll.tag == "Enemy"){coll.gameObjcet.GetComponent<敌人生命管理脚本>()...}类似的操作，而写在敌人生命管理脚本中只需要一个if(coll.name == "bullet(Clone)"){...}即可，还能直接使用private变量，显然更胜一筹。 

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

**3、2D射线检测** 

2D射线检测示例： 

```C#
RaycastHit2D hitInfo = Physics2D.Raycast(起始position, 方向向量, 距离（float）, 指定图层罩); 
``` 

距离(默认Mathf.Infinity，无限长)，指定图层罩（默认所有图层） 

示例： 

```C#
    //运用Raycast进行触地检测（向下发射一段很短的射线）
    ...
    public LayerMask ground;//在Unity中选择ground对应的图层
    private bool grounded;

    void Update()
    {
        RaycastHit2D hitInfo = Physics2D.Raycast(transform.Find("foot").position, -Vector3.up, 0.01f, Mathf.Infinity, ground);//foot为空子物体来指示位置
        if(hitInfo != null)
        {
            grounded = true;
        }
        else
        {
            grounded = false;
        }
        ...
    }
    ...
``` 

以上射线检测方法只能检测射线碰上的第一个游戏物体，若要检测射线这一路上全部游戏物体可以用RaycastAll函数： 

```C#
RaycastHit2D[] hitInfo = Physics2D.RaycastAll(起始position, 方向向量, 距离（float）, 指定图层罩);
``` 

示例：

```C#
    //敌人用射线检测玩家，检测到就开火
    ...
    private bool shoot;

    void Update()
    {
        RaycastHit2D[] hitInfo = Physics2D.RaycastAll(transform.Find("detect").position, transform.right);//detect为空子物体用于表示检测射线其实位置
        if(hitInfo != null)
        {
            foreach(info in hitInfo)
            {
                if(info.tag == "player")
                {
                    shoot = true;//可以射击了
                }
            }
        }            
    }
``` 

---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)