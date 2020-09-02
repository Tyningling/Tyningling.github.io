---
title: Java Swing
date: 2020-04-12
description: Java Swing 基于AWT的纯JAVA图形化增强工具包,介绍如何使用Swing.
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: lin
authorEmoji: 🤣
type: theme
tags:
- Java
#类别
categories:
- Java
#系列
image: https://y.gtimg.cn/music/photo_new/T002R300x300M0000004IR7y0pw4ky_1.jpg
---

## Swing是什么

Swing是Java主要的图形界面技术[曾经]。

> 它提供了跨平台的界面风格，用户可以自定义Swing的界面风格。Swing提供了比AWT更加完整的组件，引入了许多新的特性。SwingAPI是围绕着实现`AWT`各个部分的API构筑，由100%的纯JAVA实现，不存在本地代码，所以不依赖操作系统支持，这是Swing与AWT的最大区别。

## 组成结构

### 类层次结构

容器与组件构成了Swing的主要内容。

Swing容器类主要有JWindow、JFrame和JDialog，其他的不带J开头的都属于AWT提供的类。

Swing的所有组件都继承自JComponent，而JComponent又间接继承自AWT的java.awt.Component类。



![1](https://ae01.alicdn.com/kf/H0d3b8d7b2c314f18afa1811d7ae37e77Z.png)

![2](https://ae03.alicdn.com/kf/Hfbfae1df99d04b74a499a0fcbd3b1316i.png)



## 程序结构

我们知道一个图形界面应该由窗体及窗体内容器的组件组成，编写Swing程序就是创建窗体和添加组件的过程。

在Swing中，创建窗体主要使用JFrame，那么如何构建一个Swing程序呢。

### 通过创建JFrame

```Java
import java.awt.*;
import javax.swing.*;

public class Demo {
	public static void main (String[] args){
		JFrame form = new JFrame("MyFrame"); //创建JFrame
		JLabel label = new JLabel("Hello String!");//创建Label标签
		Container contentPage = form.getContentPane();//获取容器
		contentPage.add(label);//添加Label
		form.setSize(200,200);//设置窗体
		form.setVisible(true);
	}
}
```



### 通过继承JFrame

```java
import java.awt.*;
import javax.swing.*;

public class Main {
    public static void main(String[] args) {
        new form1("风陵"); //创建设置好的Jframe
    }
    public static class form1 extends JFrame {
        public form1(String title) { //通过构造传参并打开窗体
            JLabel Label1 = new JLabel("你好世界"); //生成一个Label标签
            Container container = this.getContentPane();//获得容器
            container.add(Label1);//放进去
            super.setLayout(new FlowLayout());//切换布局
            super.setTitle(title);// 设置窗体标题
            //设置窗体背景颜色与Label字体
            container.setBackground(new Color(0x050505));
            Label1.setForeground(new Color(0xBEBEBE));
            Label1.setFont(new Font("微软雅黑", Font.PLAIN, 36));
            super.setSize(400, 200);//窗体大小
            Label1.setBounds(400, 100, Label1.getWidth(), Label1.getHeight());//位置
            super.setVisible(true);//可见性 建议放置到最末尾
        }
    }
}
```
相信看完这两个DEMO一定会疑惑，Container是什么，为什么是由它来添加组件。

> 在Swing中添加到JFrame上的可见组件，除了菜单栏以外，全部都应该添加到内容面板，而不要直接添加到JFrame，这是Swing绘制系统所要求的。
> 
几乎所有的图形用户界面技术在构建界面时都采用层级结构。顶级容器JFrame是所有容器的根，子容器有内容面板和菜单栏，然后其他的组件添加到内容面板容器上。所有的Swing组件都拥有add方法，通过调用该方法，可以将其他组件添加到容器当中，作为当前容器的子组件。

界面构建层次图：

![3](https://ae06.alicdn.com/kf/Ha9638d196f0741d79ad2e3abaffdc3b0y.png)

## 事件处理

### 什么是事件处理？

图像界面的组件{比如按钮}，要响应用户操作，就必须添加事件处理机制，{假设你玩过类似的图形编程，例如VB，WPF，应该会很清楚。}，Swing采用了AWT的事件处理模型进行事件处理。

在事件处理的过程中，涉及到三个要素。

1. 事件： 用户对界面的操作，在JAVA中事件被封装成为事件类java.awt.AWTEvent及其子类，例如按钮的单机事件类为java.awt.event.ActionEvent。
2. 事件源：是事件发生的场所，就是各个组件，例如按钮单击事件的事件源是按钮。
3. 事件处理者：是事件处理程序，在 Java 中事件处理者是实现特定接口的事件对象。

在事件处理模型中最重要的是事件处理者，它根据事件的不同会实现不同的接口，这些接口命名为XXXXLisitener，所以事件处理者也被称为事件监听器。最后事件源通过addXXXListener()方法添加事件监听。

> 事件和对应的监听器接口如表所示：
```mark
   : 事件类型   :    :监听器接口中的方法:                                                 :相应监听器接口:           
| :-----------: | :-------------------------------------------------------------|:-----------------------------:|
|    Action     | actionPerformed(ActionEvent)                                  |    ActionListener             |
|     Item      | itemStateChanged(ItemEvent)                                   |    ItemListener               |
|               | mousePressed(MouseEvent)      mouseEntered(MouseEvent)        |                               |
|     Mouse     | mouseReleased(MouseEvent)     mouseExited(MouseEvent)         |    MouseListener              |
|               | mouseClicked(MouseEvent)                                      |                               |
| Mouse Montion | mouseDragged(MouseEvent) mouseMoved(MouseEvent)               |    MouseMotionListener        |
|      Key      | keyPressed(KeyEvent) keyReleased(KeyEvent) keyTyped(KeyEvent) |    KeyListener                |
|     Focus     | focusGained(FocusEvent)  focusLost(FocusEvent)                |    FocusListener              |
|  Adjustment   | adjustmentValueChanged(AdjustmentEvent)                       |    AdjustmentListener         |
|   Component   | componentMoved(ComponentEvent)                                |                               |
|               | componentHidden(ComponentEvent)                               |    ComponentListener          |
|               | componentResized(ComponentEvent)                              |                               |
|               | componentShown(ComponentEvent)                                |                               |
|    Window     | windowClosing(WindowEvent)   windowOpened(WindowEvent)        |                               |
|               | windowlconified(WindowEvent) windowDeiconified(Window Event)  |    Window Listener            |
|               | windowClosed(WindowEvent)    windowActivated(WindowEvent)     |                               |
|               | windowDeactivated(WindowEvent)                                |                               |
|   Container   | componentAdded(ContainerEvent)componentRemoved(ContainerEvent)|    ContainerListener          |
|     Text      | textValueChanged(TextEvent)                                   |    TextListener               |
```


### 通过内部类

```Java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

//  使用事件监听器 响应用户操作
public class start extends JFrame {
    //标签A
    JLabel labelA;

    public start(String title) {
        super(title); //初始化
//创建标签
        labelA = new JLabel("label");
//创建按钮
        JButton button = new JButton("Button1");
        JButton button2 = new JButton("Button2");
        JButton button3 = new JButton("Button3");
//添加到窗体容器
        super.getContentPane().add(labelA);
        super.getContentPane().add(button);
        super.getContentPane().add(button2);
        super.getContentPane().add(button3);
//添加按钮监听器
        //内部类实现监听
        button3.addActionListener(new ABC());
        button2.addActionListener(new ActionEventHandler());
        //匿名内部类实现监听
        button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent actionEvent) {
                labelA.setText("HelloSwing");
            }
        });
// 窗体设置
        super.setLayout(new FlowLayout());//切换布局
        super.setSize(550, 200);
        super.setVisible(true);
    }

    //内部类 -- 监听按钮2单击
    class ActionEventHandler implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent actionEvent) {
            // labelA.setText("Hello World!");
            labelA.setText("Hello World!");
        }
    }

   // 内部类 -- 监听按钮3单击
    class ABC implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent actionEvent) {
            labelA.setText("yes!");
        }
    }
}
```

### 通过Lambda

```java
import java.awt.*;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;
import javax.swing.*;

public class start extends JFrame implements ActionListener {
    JLabel label;
    public start(String title){
        super(title);
//创建标签
        label = new JLabel("label");
//创建按钮
        JButton button = new JButton("Button1");
        JButton button2 = new JButton("Button2");
     //   JButton button3 = new JButton("Button3");
        super.getContentPane().add(label);
        super.getContentPane().add(button);
        super.getContentPane().add(button2);
    //    super.getContentPane().add(button3);
        super.setSize(350,120);
        button2.addActionListener(this); //抽象方法
        button.addActionListener((event) -> { //Lambda {简洁}
                label.setText("Hello World"); });
        // 窗体设置
        super.setLayout(new FlowLayout()); //切换布局
        super.setSize(550, 200);
        super.setVisible(true);
    }
    @Override
    public void actionPerformed(ActionEvent event)
        {
            label.setText("HellosSwing");
        }
}
```

### 事件适配器

​	所有事件监听器都是接口类型，在JAVA接口中定义的抽象方法都必须实现，像下面给出的这个例子，我们只希望在退出窗体时清理一下内存，这只需要使用WindowListener监听器，并对WindowClosing事件重写。

```java
         //事件监听器
        this.addWindowListener(new WindowListener() {
            @Override
            public void windowOpened(WindowEvent windowEvent) { }
            @Override
            public void windowClosing(WindowEvent windowEvent) {
               //弹出关闭选项对话框
               int oldValue;
               int now  = JOptionPane.showConfirmDialog(
             null, "确定要离开么", "询问", JOptionPane.YES_NO_OPTION
               ); 
               if(now == 0){
                               System.exit(0);
               }else{                         //阻止窗体关闭
                    if (getDefaultCloseOperation() != 0) {
                        oldValue = getDefaultCloseOperation();
                        setDefaultCloseOperation(0);  //阻止窗体关闭
                    }
               }
            }
            @Override
            public void windowClosed(WindowEvent windowEvent) { }
            @Override
            public void windowIconified(WindowEvent windowEvent) { }
            @Override
            public void windowDeiconified(WindowEvent windowEvent) { }
            @Override
            public void windowActivated(WindowEvent windowEvent) { }
            @Override
            public void windowDeactivated(WindowEvent windowEvent) { }
        });
```

我们对Window监听的其他事件并不关心，但是实现监听器必须写出它其余6个接口的实现，这就使代码看起来十分臃肿，为此JAVA提供了与监听器配套的适配器类。使用**通过继承**事件所对应的适配器类，只需要重写所需的方法应用的事件，不再需要关心其余的事件。

```java
        //事件适配器
        this.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                //弹出关闭选项对话框
                int oldValue;
                int now = JOptionPane.showConfirmDialog(
                        null, "确定要离开么", "询问", JOptionPane.YES_NO_OPTION
                );
                if (now == 0) {
                    //setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  //清理窗体
                    System.exit(0);

                } else {
                    if (getDefaultCloseOperation() != 0) {
                        oldValue = getDefaultCloseOperation();
                        setDefaultCloseOperation(0);
                    }
                }
            }
        }); 
```

需要注意一点，事件适配器提供了一种简单的实现监听器的手段，可以缩短程序代码。但是，**由于Java并不支持多继承，当需要多种监听器或此类已有父类时**，就无法采用事件适配器。

> 而且，并非所有的监听器接口都有对应的适配器类，一般定义了多个方法的监听器接口，例如WIndow，才需要配套的适配器。主要的适配器如下：
>
> ComponentAdapter：组件适配器
>
> ContainerAdapter：容器适配器
>
> FocusAdapter：焦点适配器
>
> KeyAdapter：键盘适配器
>
> MouseAdapter：鼠标适配器
>
> MouseMotionAdapter：鼠标运动适配器
>
> WindowAdapter：窗口适配器

## 关于布局管理器与其他组件

关于布局管理器与其他组件的详解，想了解的去看看[C语言中文网](http://c.biancheng.net/view/1212.html)的教程，JAVA之所以提供布局管理，是因为在早期的编程环境中，布局管理能使效率上升，毕竟光设置代码就够累了{参照CSS}，但到现在我觉得没有任何必要，现代的IDE一般都自带有可视化设计功能或相关方面的插件，你写代码设计的时间完全可以省下一大笔，虽然说代码不好看，但是也没人在意。

再者，Swing是一个很老旧的产物，目前也就是只是停留在还能用的级别，如今已经是2020年，仍在使用Swing的开发者并不多。

