示例： 

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
//玩家生命设置
public class player_health : MonoBehaviour
{
    public GameObject bloodEffect;//受伤掉血的粒子特效
    public GameObject deathMenu;//死亡时巨大的“DEATH”提示以及UI菜单栏
    private float health;//当前血量
    public float healthmax;//血量上限
    public GameObject messageprefab;//一个DontDestroyOnLoad的物体，用于在不同场景间传递信息（比如当前关卡的初始血量应当为上一关结束时剩下的）
    
    void Start()//关卡开始时的血量初始化
    {
        if (GameObject.Find("message(Clone)") == null)//没有前一关的信息（也就是第一关）
        {
            SetHealth(healthmax);
            Instantiate(messageprefab, transform.position, transform.rotation);
        }
        else//有前一关的message
        {
            health = GameObject.Find("message(Clone)").GetComponent<message>().GetPreHealth();//获取
        }
    }
    public void Damage(float damage)//扣血的函数，在敌人子弹的OnTriggerEnter里调用（当然，扣血的操作也可以写在当前类的OnTriggerEnter里，如下面的enemy_health）
    {
        health -= damage;
        Instantiate(bloodEffect, transform.position, transform.rotation);//掉血粒子特效
        if (health < 0.1f)//死亡
        {
            //死亡动画设置，粒子特效设置等等
            deathMenu.SetActive(true);//死亡时打开死亡场景菜单
            enemy_health.SetEnemyNum();//这个很坑，见下面enemy_health的SetEnemyNum函数
        }
    }
    public float GetHealth()//获取当前血量的函数，关卡结束时message(Clone)将通过它获取下一关的初始血量
    {
        return health;
    }
}
//敌人生命设置
public class enemy_health : MonoBehaviour
{
    public GameObject bloodeffect;
    private float health;
    public float healthmax;
    static int enemy_num = 0;//用来记录现存敌人数量的静态成员（所有敌人死亡后才能触发下一关）

    void Start()
    {
        health = healthmax;
        enemy_num++;//每生成一个敌人，总数加一
    }
    void OnTriggerEnter2D(Collider2D coll)//写在角色处的扣血操作
    {
        if (coll.name == "bullet(Clone)")
        {
            health -= coll.gameObject.GetComponent<bullet>().damage;
            Instantiate(bloodeffect, transform.position, transform.rotation);
            if (health < 0.1f)
            {
                //敌人死亡的种种效果
                enemy_num--;//每死一个敌人，总数减一
            }
        }
    }
    public static int GetEnemyNum()//获取现存敌人数量
    {
        return enemy_num;
    }
    public static void SetEnemyNum()//用于归零现存敌人数量：在player死时，场景中还有敌人活着，这时候直接通过deathMenu跳转到主菜单重开一局，那么player死的那关就有几个敌人的数量始终没有被减掉，那么第二局第一关就会出现杀光了敌人却触发不了下一关的情况
    {
        enemy_num = 0;
    }
}
```

---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)