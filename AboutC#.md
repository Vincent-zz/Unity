简易遍历： 

```C#
int[] a = new int(10);
foreach(int _a in a)
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
    public int b{get;set;}//自动实现
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

序列化（将对象转化为二进制数据的过程）： 

Unity由C++编写，与C#脚本交互需要序列化与反序列化 

例：Inspector窗口中的值被序列化，加载脚本后进行反序列化，实现通过Inspector赋值；每一个prefab都是序列化过后的数据，在场景中粘贴时将进行反序列化 

```C#
class A
{
    public int a;//public成员默认可序列化
    [SerializeField] private int a_;//private、protected成员通过[SerializeField]实现可序列化（在Inspector窗口中显示及修改）
}
``` 

```C#
class A
{
    [HideInInspector] public int a;//Inspector窗口中不显示
}
``` 

字典类 

```C#
Dictionary<type1, type2> myDic = new Dictionary<type1, type2>();
myDic.Add(key1, value1);
```