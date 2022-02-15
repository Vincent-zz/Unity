## Component

### Rigidbody2D（刚体） 

**Body Type** 

`Dynamic`：能由代码控制运动，也会受到力的作用（重力，其他物件撞击），是最符合物理规律的也是开销最大的，主角一般都会使用Dynamic 

`Kinematic`：只能由代码控制运动，不受其他力的作用，例:在固定两点间巡逻的敌人，不会被其他物件撞出指定路线 

`Static`：确定静止不动的刚体，如地图，障碍物等（开销最小） 

**Collision Detection** 

**Constraints** 

### Collider2D（碰撞体） 

## Collision & Trigger

碰撞的条件：首先前提是参与碰撞的两个物体都有collider（都不为触发）。然后其中至少一个是dynamic刚体（所以说kinematic刚体不会碰上静止碰撞体）。 

触发的条件：参与碰撞的两个物体都有collider且任意一个为触发 

[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)
