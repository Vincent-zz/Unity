简易遍历： 

```C#
int[] a = new int(10);
foreach(_a in a)
{
    _a++;
}
``` 

属性：（读写权限分离） 

```C#
public class A
{
    private int a;
    public int _a
    {
        get
        {
            return a;
        }
        set
        {
            _a = value;
        }
    }
}
class Program
{
    static void Main
    {
        A classA = new A();
        classA._a = 1;//将1赋给a
        Console.WriteLine(classA._a);//输出a的值
    }
}
```