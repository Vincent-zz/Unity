###延时 
**简陋计时器** 

**Invoke** 

`Involk("函数名", 延时时间);`，`Involk("函数名", 延时开始时间,执行间隔时间);` 

**协程** 

`StartCoroutine("协程函数名")` 

```C#
IEnumerator 函数名()
{
    ...
    yield return null;
}
``` 

`yield return WaitForSeconde(时间)` 

###“子弹时间”