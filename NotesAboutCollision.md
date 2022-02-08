## Layer, Sorting Layer, Tag三者的区别 

- Sorting Layer：位于Sprite Renderer组件中，管理的是渲染的显示层级，同一个Sorting Layer的游戏物件的显示层级由Order in Layer的大小决定  

- Layer：位于inspector窗口右上角，可应用于碰撞检测与射线检测，是LayerMask类的实例 

获取：`游戏物件.layer`

- Tag： 用于标识游戏物件，可以通过tag查找游戏物件以及在碰撞时标识物体 

获取：`此游戏物件的任意组件.tag`或`游戏物件.tag` 

## 碰撞检测方法 

OnCollisionEnter Physics2D.Linecast 

LayerMask 
