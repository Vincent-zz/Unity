## 设计模式之状态模式 

状态接口： 

```C#
public interface state
{
    void OnEnter();
    void OnUpdate();
    void OnExit();
}
``` 

具体的状态类： 

```C#
public class idleState : state
{
    private FSM manager;

    public idleState(FSM manager_)
    {
        manager = manager_
    }

    public void OnEnter()
    {
        //...
    }
    public void OnUpdate()
    {
        //...
    }
    public void OnExit()
    {
        //...
    }
}
``` 

状态机脚本： 

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public enum stateType
{
    idle,
    jump,
    fall,
    attack,
    hurt,
    dodge
}

public class FSM : MonoBehaviour
{
    private state currentState;
    private Dictionary<stateType, state> states = new Dictionary<stateType, state>();

    void Start()
    {
        states.Add(stateType.idle, new idleState(this));
        //...
        Transition(stateType.idle);//set initial state
    }

    void Update()
    {
        currentState.OnUpdate();
    }

    public void Transition(stateType type)
    {
        if(currentState != NULL)
        {
            currentState.OnExit();
        }
        currentState = states[type];
        currentState.OnEnter();
    }
}
```