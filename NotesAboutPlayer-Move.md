###移动方法
1、translate transform 

2、rigidbody2d addforce velocity 

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.Windows;
public class player_move : MonoBehaviour
{
    private Rigidbody2D rig;
    private Animator anim;
    private CapsuleCollider2D coll;
    private Transform trans;

    public float jumpforce;
    public float deltaforce;
    public float speed;
    public float speedInTheAir;
    public LayerMask ground;
    public int jumptimemax;
    
    private int jumptime;   
    public bool isRight;
    private bool isGround;
    private bool isRun;

    // Start is called before the first frame update
    void Start()
    {
        rig = GetComponent<Rigidbody2D>();
        anim = GetComponent<Animator>();
        coll = GetComponent<CapsuleCollider2D>();
        trans = GetComponent<Transform>();
        isRight = true;
        isRun = false;
        rig.velocity = new Vector3(8.8f, 0f, 0f);
    }

    // Update is called once per frame
    void Update()
    {
        if (coll.IsTouchingLayers(ground))
        {
            isGround = true;
        }
        else
        {
            isGround = false;
        }
        if (isGround)
        {
            jumptime = 0;
            anim.SetBool("fall", false);
            if (Input.GetKey(KeyCode.D))
            {
                if (!isRight)
                {
                    trans.Rotate(new Vector3(0f, -180f, 0f));
                    isRight = true;
                }
                anim.SetBool("idle", false);
                anim.SetBool("move", true);
                rig.velocity = new Vector3(speed, rig.velocity.y, 0f);
            }
            else if (Input.GetKey(KeyCode.A))
            {
                if (isRight)
                {
                    trans.Rotate(new Vector3(0f, 180f, 0f));
                    isRight = false;
                }
                anim.SetBool("idle", false);
                anim.SetBool("move", true);
                rig.velocity = new Vector3(-speed, rig.velocity.y, 0f);
            }
            if (rig.velocity.magnitude < 1f)
            {
                anim.SetBool("move", false);
                anim.SetBool("idle", true);
            }
        }
        else 
        {
            anim.SetBool("idle", false);
            anim.SetBool("move", false);
            if (Input.GetKey(KeyCode.D))
            {
                if (!isRight)
                {
                    trans.Rotate(new Vector3(0f, -180f, 0f));
                    isRight = true;
                }
                rig.velocity = new Vector3(speedInTheAir, rig.velocity.y, 0f);
            }
            else if (Input.GetKey(KeyCode.A))
            {
                if (isRight)
                {
                    trans.Rotate(new Vector3(0f, 180f, 0f));
                    isRight = false;
                }
                rig.velocity = new Vector3(-speedInTheAir, rig.velocity.y, 0f);
            }
            if (rig.velocity.y > 0.00001f)
            {
                anim.SetBool("fall", false);
                anim.SetBool("jump", true);
            }
            else
            {
                anim.SetBool("jump", false);
                anim.SetBool("fall", true);
            }
        }
        if (Input.GetKeyDown(KeyCode.K))
        {   
            jumptime++;
            if (jumptime < jumptimemax)
            {
                rig.AddForce(new Vector3(0f, 1f, 0f) * (jumpforce - jumptime * deltaforce));
            }
        }
    }
}

//左右动画不相同时
 void Update()
    {
        if (coll.IsTouchingLayers(ground))
        {
            isGround = true;
        }
        else
        {
            isGround = false;
        }
        if (isGround)
        {
            jumptime = 0;
            anim.SetBool("fallLeft", false);
            anim.SetBool("fallRight", false);
            if (Input.GetKey(KeyCode.D))
            {
                if (!isRight)
                {
                    isRight = true;
                }
                anim.SetBool("idleLeft", false);
                anim.SetBool("idleRight", false);
                anim.SetBool("moveLeft", false);
                anim.SetBool("moveRight", true);
                rig.velocity = new Vector3(speed, rig.velocity.y, 0f);
            }
            else if (Input.GetKey(KeyCode.A))
            {
                if (isRight)
                {
                    isRight = false;
                }
                anim.SetBool("idleLeft", false);
                anim.SetBool("idleRight", false);
                anim.SetBool("moveRight", false);
                anim.SetBool("moveLeft", true);
                rig.velocity = new Vector3(-speed, rig.velocity.y, 0f);
            }
            if (rig.velocity.magnitude < 1f)
            {
                anim.SetBool("moveRight", false);
                anim.SetBool("moveLeft", false);
                if (isRight)
                {
                    anim.SetBool("fallRight", false);
                    anim.SetBool("idleLeft", false);
                    anim.SetBool("idleRight", true);
                }
                else
                {
                    anim.SetBool("fallLeft", false);
                    anim.SetBool("idleRight", false);
                    anim.SetBool("idleLeft", true);
                }
            }
        }

        else 
        {
            anim.SetBool("idleLeft", false);
            anim.SetBool("idleRight", false);
            anim.SetBool("moveLeft", false);
            anim.SetBool("moveRight", false);
            if (Input.GetKey(KeyCode.D))
            {
                if (!isRight)
                {
                    isRight = true;
                }
                rig.velocity = new Vector3(speed, rig.velocity.y, 0f);
            }
            else if (Input.GetKey(KeyCode.A))
            {
                if (isRight)
                {
                    isRight = false;
                }
                rig.velocity = new Vector3(-speed, rig.velocity.y, 0f);
            }
            if (isRight)
            {
                if (rig.velocity.y > 0.00001f)
                {
                    anim.SetBool("fallLeft", false);
                    anim.SetBool("fallRight", false);
                    anim.SetBool("jumpLeft", false);
                    anim.SetBool("jumpRight", true);
                }
                else
                {
                    anim.SetBool("jumpLeft", false);
                    anim.SetBool("jumpRight", false);
                    anim.SetBool("fallLeft", false);
                    anim.SetBool("fallRight", true);
                }
            }
            else if (!isRight)
            {
                if (rig.velocity.y > 0.00001f)
                {
                    anim.SetBool("fallLeft", false);
                    anim.SetBool("fallRight", false);
                    anim.SetBool("jumpRight", false);
                    anim.SetBool("jumpLeft", true);
                }
                else
                {
                    anim.SetBool("jumpLeft", false);
                    anim.SetBool("jumpRight", false);
                    anim.SetBool("fallRight", false);
                    anim.SetBool("fallLeft", true);
                }
            }            
        }
        if (Input.GetKeyDown(KeyCode.W))
        {   
            jumptime++;
            if (jumptime < jumptimemax)
            {
                rig.AddForce(new Vector3(0f, 1f, 0f) * jumpforce);
            }
        }
    }
 */
```
