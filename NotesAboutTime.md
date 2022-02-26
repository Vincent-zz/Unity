###延时 
**简陋计时器** 

**Invoke** 

`Involk("函数（无参数）名", 延时开始时间);`（\*延时开始时间为0f时并不是立即执行，而是等当前帧过了之后才执行） 
 
停止：`CancelInvoke();` 

**协程** 

`StartCoroutine(协程函数名(参数))`或`StartCoroutine("协程函数名", 参数)`（\*也并不是马上执行的，等当前帧过了才执行） 

```C#
IEnumerator 协程函数名(参数)
{
    ...
    yield return null;//下一帧再调用后面的
    ...
}
``` 

`yield return new WaitForSeconds(时间)`（\*类似，时间填0f等价于yield return null，要等下一帧） 

停止：`StopCoroutine("协程函数名");` 

\* 已经启动的invoke与coroutine不会因SetActive(false)而停止 

###“子弹时间”