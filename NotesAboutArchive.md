## PlayerPrefs 

`PlayerPrefs`是unity内置的用于储存玩家偏好（player preference）的类，可以实现少量简单数据的存储 

```C#
//数据存储各三种静态函数
PlayerPrefs.SetString("keyName1", value1);
PlayerPrefs.SetInt("keyName2", value2);
PlayerPrefs.SetFloat("keyName3", value3);
PlayerPrefs.GetString("keyName1");
PlayerPrefs.GetInt("keyName2");
PlayerPrefs.GetFloat("keyName3");
//读取时可以传入“默认值”，当keyName并没有值时返回"Nothing"
PlayerPrefs.GetString("keyName","Nothing");
//判断是否存在某个Key
bool hasKey = PlayerPrefs.HasKey("keyName");
``` 

数据以注册表（Registry）存在指定路径下：计算机\HKEY_CURRENT_USER\SOFTWARE\Unity\UnityEditor\Default\Company\项目文件名 

因此极易遭魔改 


## JsonUtility  

JavaScript Object Notation：是特殊的字符串，可在不同语言间转换 

```C#
string messageInJson = JsonUtility.ToJson(可序列化类);//将传入类中的可序列化字段转化为json；*判断是否可序列化：在inspector窗口中是否显示
object message = JsonUtility.FromJson(json字符串);//
```  

将Json字符串保存到本地文件（`using System.IO`） 

```C#
string path = Path.Combine(Application.persistentDataPath, 文件名);//Application.persistentDataPath自动对应各平台的储存路径，文件名（包括后缀）可以任意取
File.WriteAllText(path, json字符串);//创建并写入（已存在则会覆盖）
string messageInJson = File.ReadAllText(path);//读取
``` 

---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)