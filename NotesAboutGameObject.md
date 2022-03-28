**粘贴：Instantiate** 

`Instantiate(游戏物件, 位置矢量, 旋转欧拉角);` 

**销毁：Destroy** 

`Destroy(游戏物件, 时间（float）);`，时间默认为0（然而销毁总是发生在*当前Update结束后*） 

示例： 

```C#
//实现按下空格时玩家射击
...
public GameObject bulletprefab;//Unity中拖入子弹预制体
private Transform fp;
void Start()
{
    fp = transform.Find("firepoint");//firepoint是一个空物件，作为玩家子物件来表示开火位置
    ...
}
void Update()
{
    if(Input.GetKeyDown(KeyCode.Space))
    { 
        Instantiate(bulletprefab, fp.position, fp.rotation);//粘贴子弹
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
    Destroy(gameObject);//撞上东西，子弹自毁，见关于碰撞触发的笔记
    ...//其他操作，如敌人扣血（当然也可以放在敌人的代码里）
}
``` 

[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)