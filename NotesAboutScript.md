### Get GameObject & Component（获取游戏物件及其组件） 

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

P.S.尽量不要在`Update()`函数中使用，一般写在`Start()`函数中 

2、`游戏物件.Getcomponent<T>()`获取某个游戏物件的组件 

首先，辨明`gameObject`与`GameObject`的区别： 

~显然，在于开头的大小写~ `gameObject`在脚本中的意思就是**此脚本所挂载的游戏物件**；而`GameObject`是游戏物件的类名。 

![`gameObject`与`GameObject`的区别]()

### Function（函数） 


### Input（输入） 


### Player_Move（角色移动）
