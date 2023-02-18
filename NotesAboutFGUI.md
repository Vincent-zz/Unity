# FGUI

**FGUI的核心优势** 

将GUI设计中程序和美术的工作（近乎彻底地）分离开来 

美术制作时无需学习引擎、代码，单纯设计即可 

程序制作时无需频繁与美术对接，单纯编写代码即可 

## FGUI中UI界面的设计（美术部分）

**FGUI的层次结构**

- 组件（Component）类比unity中的Canvas、Panel
- 组
  - 普通组：仅FGUI内编辑时有效，导出后无效
  - 高级组
    - 导出后类比unity中的Panel
    - 布局：“无”的情况下默认锚点类比unity中小花瓣在上一级的四个角
- 元件，类比Unity中的按钮、文字等UI
  - 基础元件：文本、图片、动画等
  - 组合型原件：按钮、下拉框、滚动条等
  - 特殊原件：列表

**导出FGUI、导入Unity**

- Unity资源商店import免费的FairyGUI package
- 将要导出的Component设置为导出，右键包选择导出，直接导出至Unity项目文件
- Unity中新建一个UIPanel（StageCamera伴随着生成），在Inspector窗口中设置包名、组件名
- 为UIPanel添加UI Content Scaler组件，设置为Scale With Screen Size以及分辨率
- 对原相机的culling mask进行设置，使之不渲染UI（否则会渲染两次）

**关于图片**

- 九宫格缩放：对话框的缩放方式
- 填充：血条、技能CD等

FGUI会在导出时生成纹理集，将所有用于制作Component的图片纹理放在几张纹理集图片里，可以单独设置每个图片、动画所在的纹理集编号，但过大的动画可能会出错（不得不将帧放在不同的纹理集中，导致出错） 

**关于文本**

- 勾选输入后作为输入框（勾上密码，则*代替）
- 输入限制，使用Unity正则表达式
- 文本编辑支持UBB语法
- 字体
  - 资源->新建位图字体
- 关于富文本
  - 支持HTML
  - 可交互（如作为按钮使用）


**关于各种条**

- 进度条：不可拖动，可做血条、蓝条等
- 滑动条：鼠标拖动
- 滚动条：可拖可滚，和列表搭配使用

**关联系统**（Unity中的关联系统为锚点系统）
- UGUI中锚点系统是只能与父物体关联的FGUI中则可以与任意物体关联
- 例子
  - 左->右:左到被关联物体右距离不变
  - 左右居中：左右到被关联物体中线距离不变
  - 加%后：会有缩放...

**动效**

- 可创建补间动画，类似Unity中的制作
- 组件、元件都能建立各自的动效（图片似乎不行，因为图片元件没有单独的编辑窗口），动效作用的对象是该组件/元件显示列表中的物体（子物体）
- 组件、元件的动效在Unity中对应UI物体（FGUI中组件、元件本质一样，到了Unity中都是UI物体）上挂载的Animation
- 标签：类似Unity中帧事件，可以理解为帧事件的一处标记，而该标记对应的程序代码则在Unity中由程序员实现

**控制器** 

## 脚本API（程序部分） 

[在官网中完整详细的FGUI API](https://fairygui.com/api/html/9b3868b6-73f2-a7b9-0f13-8b1eb3441ecd.htm) 

**关于GComponent与GObject的基本操作** 

``` C#
//此脚本挂载在UIPanel上，以下为组件的取用与生成、元件的取用等基本操作
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

using FairyGUI;//

class fguiAPI
{
  private GComponent comp1;//FGUI组件的类型
  private GComponent comp2;
  void Start()
  {
    comp1 = GetComponent<UIPanel>().ui;
    //获取该UIPanel名为UIPanel的脚本组件中的名为ui（类型为GComponent）的成员，即该UIPanel所显示的FGUI组件


    comp2 = UIPackage.CreateObject("包名", "组件名").asCom;
    //使用UIPackage的静态函数生成指定FGUI组件（不可见）
    //CreateObject(string, string)函数返回的是GObject类型的组件，所以还需后面的类型转换（asCom是静态只读自动属性）


    GButton buttom1 = comp1.GetChild("Comp1组件中的某按钮名").asButton;
    GObject movieClip1 = comp1.GetChild("Comp1组件中的某动画名");
    //获取组件中指定元件的两个例子(GetChild方法最终获取到的类型为GObject)
  }

  void Comp2ShowUp
  {
    GRoot.inst.AddChild(Comp2);
    //Comp2显示（inst是静态只读自动属性）
  }

  void Comp2GetRemoved
  {
    GRoot.inst.RemoveChild(Comp2);
    //Comp2被去除
  }
}
``` 

Unity中开始运行后

- 与UIPanel关联的FGUI组件会作为UIPanel的子物体生成
- 组件内的元件作为该组件的子物体生成
- 一个名为Stage的DontDestroyOnLoad物体会生成，它会有一个名为GRoot的子物体

`UIPackage.CreateObject`生成指定FGUI组件后

- 该组件作为DontDestroyOnLoad物体生成
- 该组件为不可见状态

显示组件的过程

- 将该组件作为GRoot的子物体
- 该组件（自动变成）可见

\* 组件（GComponent）、元件（GButtom、GImage、GGroup等）二者虽然在FGUI中处于不同层级，但都是从GObject类继承而来的，有很多通用的属性（类似于把Canvas以及一部分Panel从UGUI中拿出来单独做一个叫组件的层级，而本质上还是与其他UI一样的） 

**关于动效** 

```C#
//此脚本挂载在UIPanel上
class transitionAPI
{
  private GComponent comp1;
  private Transition t1, t2;

  void Start()
  {
    comp1 = GetComponent<UIPanel>().ui;//获取组件

    t1 = comp1.GetTransition("动效1名");
    t2 = comp1.GetTransition("动效2名");
    //从组件中获取动效

    t1.SetHook("标签名", ()=>{HookFunction();});
    //设置标签对应的帧事件
  }

  void Play()
  {
    t1.play();//播放动效

    t2.play(()=>{AfterTheEnd();});
    //播放动效，并在播放完后执行AfterTheEnd函数
  }

  void HookFunction()
  {
    //标签处执行的代码（帧事件）
  }

  void AfterTheEnd()
  {
    //动效播放完后执行的代码
  }
}
``` 

**关于按钮**  

```C#
//此脚本挂载在UIPanel上
class buttonAPI
{
  private GComponent comp1;
  private GButton b1;

  void Start()
  {
    comp1 = GetComponent<UIPanel>().ui;
    b1 = comp1.GetChild("按钮名"),asButton;//获取，返回GObject类

    b1.onClick.Add(()=>{Clicked();});
    //为按钮添加按下后要执行的函数
    //onClick是EventListener类型的静态只读自动属性
    //onClick与Add()是从GObject继承而来的，所以即使没有类型转换也可以直接使用.onClick.Add()
  }

  void Clicked()
  {
    //按下后要执行的代码
  }
}
``` 

---
[Back to Notes](https://github.com/Vincent-zz/Unity/blob/main/UnityNotes.md)