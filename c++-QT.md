# QT学习笔记



## 01_QT简介

在 Qt 中，我们将窗口和控件统称为部件（Widget）

窗口是指程序的整体界面，可以包含标题栏、菜单栏、工具栏、关闭按钮、最小化按钮、最大化按钮等。
 控件是指按钮、复选框、文本框、表格、进度条等这些组成程序的基本元素。一个程序可以有多个窗口，一个窗口也可以有多个控件。

+ QWidget 是所有用户界面元素的基类，窗口和控件都是直接或间接继承自 QWidget

+ QMainWindow、QWidget、QDialog 三个类就是用来创建窗口的，可以直接使用也可以继承后再使用。
+ QMainWindow 窗口可以包含菜单栏、工具栏、状态栏、标题栏等，是最常见的窗口形式，可以作为GUI程序的主窗口。
+  QDialog 是对话框窗口的基类。对话框主要用来执行短期任务，或与用户进行互动，它可以是模态的也可以是非模态的。QDialog 没有菜单栏、工具栏、状态栏等。



## 02_创建文件



### 2.1 三大基类

1. QWidget:  
2. QMainDows
3. QDialog:    对话框 新窗口



### 2.2 基础文件

1. 项目名.pro

   ```python
   # QT包含的模块   core和gui
   QT       += core gui
   
   # QT版本大于4后 包含widgets模块
   greaterThan(QT_MAJOR_VERSION, 4): QT += widgets
   
   CONFIG += c++11
   
   DEFINES += QT_DEPRECATED_WARNINGS
   
   # 原文件
   SOURCES += \
       infobtn.cpp \
       main.cpp \
       mywidget.cpp \
       printmsg.cpp
   
   # 头文件
   HEADERS += \
       infobtn.h \
       mywidget.h \
       printmsg.h
   
   # 语言
   TRANSLATIONS += \
       MyFirstQt_zh_CN.ts
   
   # Default rules for deployment.
   qnx: target.path = /tmp/$${TARGET}/bin
   else: unix:!android: target.path = /opt/$${TARGET}/bin
   !isEmpty(target.path): INSTALLS += target
   
   ```

2. main.cpp

   ```c++
   #include "mywidget.h"
   #include <QApplication>         // 包含一个应用程序头文件
    // 入口    args命令行变量的数量  argv命令行变量的数量
   int main(int argc, char *argv[])
   {
       // a是应用程序对象   在QT中, 应用程序对象有且仅有一个
       QApplication a(argc, argv);
       //  w是窗口对象 有点类似Jframe
       MyWidget w;
       //  窗口对象默认不会显示   需要调用show方法
       w.show();
       //  应用程序对象进入消息循环机制    相当于代码阻塞 后面的代码不会运行
       return a.exec();
   }
   
   ```

3. MyWidget.h

   ```c++
   #ifndef MYWIDGET_H
   #define MYWIDGET_H
   #include "infobtn.h"
   #include "printmsg.h"
   #include <QWidget>
   
   class MyWidget : public QWidget
   {
       // 宏 允许类中使用信号和槽的机制
       Q_OBJECT
   
   public:
       MyWidget(QWidget *parent = nullptr);
       ~MyWidget();
   
   private:
       InfoBtn *infoBtn;
       PrintMsg *printMsg;
   
       void afterCliect();
   
   };
   #endif // MYWIDGET_H
   ```

4. MyWidget.cpp

   ```c++
   #include "mywidget.h"
   #include <QPushButton>
   MyWidget::MyWidget(QWidget *parent)
       : QWidget(parent)
   {   
   }
   MyWidget::~MyWidget()
   {}
   ```



## 03_常用组件

+ 按钮:  QPushButton

+ 



## 04_继承树图





## 05_信号和槽

信号和槽是用于对象之间的通信，它是Qt的核心机制。

再举一个例子，比如在一个主窗口内有一个关闭按钮，如果点击这个按钮窗口就会关闭，那么关闭按钮是发送信号的对象，它发送的信号是点击，接收信号的对象是窗口，响应信号的槽是关闭窗口。

 <img src = "https://img2020.cnblogs.com/blog/442200/202008/442200-20200815205318043-1961895942.png" style=zoom:30%>

### 5.1 connect函数(qt5)

Qt 5 推出了新的 connect 函数，不需要使用 SIGNAL() 和 SLOT() 宏，可以在编译时做类型检查：

connect函数的声明如下：

`[static] QMetaObject::Connection QObject::connect(const QObject *sender, PointerToMemberFunction signal, const QObject *context, Functor functor, Qt::ConnectionType type = Qt::AutoConnection)`

简单讲:

`connect(信号的发出者, 发送的具体信号, 信号的接收者, 信号的处理又称为槽函数)`

值得一提:

信号函数和槽函数需要的是函数地址



### 5.2 简单示例

1. 使用系统定义好的信号和槽函数

   ```c++
   // 关闭窗口    btn是按钮对象
   connect(btn,&QPushButton::clicked,this,&MyWidget::close);
   ```

2. 点击后实现打印信息

   + 定义接收信息对象 printMsg.h

     ```c++
     #ifndef PRINTMSG_H
     #define PRINTMSG_H
     #include <QObject>
     #include <QDebug>
     class PrintMsg:public QObject
     {
         Q_OBJECT
     public:
         explicit PrintMsg(QObject *parent = nullptr);
         // 需要定义和实现
         void msg();
     };
     #endif // PRINTMSG_H
     ```

   + printMsg的实现 printMsg.cpp

     ```c++
     #include "printmsg.h"
     PrintMsg::PrintMsg(QObject *paraent):QObject(paraent)
     {}
     void PrintMsg::msg(){
         qDebug() << "提示信息" ;
     }
     ```

   + 窗口类头需要声明两个对象  myWidget.h

     ```c++
     #ifndef MYWIDGET_H
     #define MYWIDGET_H
     #include "infobtn.h"
     #include "printmsg.h"
     #include <QWidget>
     class MyWidget : public QWidget
     {
     public:
         MyWidget(QWidget *parent = nullptr);
         ~MyWidget();
     private:
         InfoBtn *infoBtn;
         PrintMsg *printMsg;
     
         void afterCliect();
     
     };
     #endif // MYWIDGET_H
     ```

   + 窗口类的实现  myWidget.cpp

     ```c++
     #include "mywidget.h"
     #include <QPushButton>
     MyWidget::MyWidget(QWidget *parent)
         : QWidget(parent)
     {
        this->infoBtn = new InfoBtn(this);
        infoBtn->setText("提示信息按钮");
        this->printMsg = new PrintMsg(this);
         // 信号和槽 做连接    信号函数和槽函数需要的是函数地址
         connect(infoBtn, &InfoBtn::clicked,printMsg,&PrintMsg::msg);
     }
     MyWidget::~MyWidget()
     {
     }
     ```



### 5.3 自定义信号和槽

#### 5.3.1 无参

1. 示例:

   下课后:

   ​	老师触发一个信号 -> 饿了

   ​	学生响应信号    -> 请客吃饭

2. 步骤:

   1. 新建teacher和student类(.h和.cpp)
   2. 发信号方定义信号:
      1. 在`signals:`作用域下自定义信号(返回void, 可重载, 只需要声明不需要实现)
   3. 接收信号方定义槽函数:
      1. 在`public slots:`作用域下自定义槽函数(返回void, 可重载 ,需要声明也需要实现)
      2. 在`.cpp`中实现槽函数
   4. widget函数中做连接
      1. 在`.h`下声明信号发出和接收对象指针(`public`或`private`作用域都可以)
      2. 在`.cpp`下实例化对象(如果发生类和对象重名使用class关键词区分)
      3. 编写连接     将两者进行绑定  如此一来触发前者联动后者
      4. `connect(信号发出者, 发出信号函数的地址, 信号接收者, 槽函数地址);`
   5. 编写触发信号函数
      1. widget的`.h`下声明
      2. widget的`.cpp`下实现   
      3. 信号函数前需要使用`emit`关键字
   6. 在`widget.cpp`中调用触发函数

3. 实现:

   + teacher

     ``` c++
     #ifndef TEACHER_H
     #define TEACHER_H
     #include <QObject>
     class teacher : public QObject
     {
         Q_OBJECT
     public:
         explicit teacher(QObject *parent = nullptr);
     signals:
         //在signals:作用域下自定义信号(返回void, 可重载, 只需要声明不需要实现)
         void hungry();
     };
     #endif // TEACHER_H
     
     #include "teacher.h"
     teacher::teacher(QObject *parent) : QObject(parent)
     {
     }
     ```

   + student

     ``` c++
     #ifndef STUDENT_H
     #define STUDENT_H
     #include <QDebug>
     #include <QObject>
     class student : public QObject
     {
         Q_OBJECT
     public:
         explicit student(QObject *parent = nullptr);
     signals:
     public slots:
         // 在public slots:作用域下自定义槽函数(返回void, 可重载 ,需要声明也需要实现)
         void treat();
     };
     #endif // STUDENT_H
     
     
     #include "student.h"
     student::student(QObject *parent) : QObject(parent)
     {
     }
     void student::treat(){
         qDebug() << "请客吃饭" ;
     }
     ```

   + widget

     ``` c++
     #ifndef MYWIDGET_H
     #define MYWIDGET_H
     #include "teacher.h"
     #include "student.h"
     #include <QWidget>
     class MyWidget : public QWidget
     {
     public:
         MyWidget(QWidget *parent = nullptr);
         void classIsOver();
         ~MyWidget();
     private:
         teacher *teacher;
         student *student;
     };
     #endif // MYWIDGET_H
     
     
     #include "mywidget.h"
     #include <QPushButton>
     //
     /* teacher类
      * student类
      * 下课后:
      *      老师触发一个信号 -> 饿了
      *      学生响应信号    -> 请客吃饭
      */
     MyWidget::MyWidget(QWidget *parent)
         : QWidget(parent)
     {
         // 调用带参数的构造  父类销毁时子类也会销毁
         // 创建老师对象   teacher对象和teacher类重名  编译器懵逼了
         this->teacher = new class teacher(this);
         // 创建学生对象
         this->student = new class student(this);
         // 连接   将两者进行绑定  如此一来触发前者联动后者
         connect(teacher, &teacher::hungry, student, &student::treat);
         // 编写信号触发函数
         classIsOver();
     }
     void MyWidget::classIsOver(){
         emit teacher->hungry();
     }
     MyWidget::~MyWidget()
     {
     }
     ```

     

#### 5.3.2 有参

1. 示例:

   下课后:

   ​	老师触发一个信号 -> 饿了  并通过参数传入想吃什么

   ​	学生响应信号    -> 请客吃饭    接收参数

2. 步骤:

   1. 新建teacher和student类(.h和.cpp)
   2. 发信号方定义信号:
      1. 在`signals:`作用域下自定义信号(返回void, 可重载, 只需要声明不需要实现)
   3. 接收信号方定义槽函数:
      1. 在`public slots:`作用域下自定义槽函数(返回void, 可重载 ,需要声明也需要实现)
      2. 在`.cpp`中实现槽函数
   4. widget函数中做连接
      1. 在`.h`下声明信号发出和接收对象指针(`public`或`private`作用域都可以)
      2. 在`.cpp`下实例化对象(如果发生类和对象重名使用class关键词区分)
      3. 编写连接     将两者进行绑定  如此一来触发前者联动后者
      4. 编写信号和槽函数指针
      5. `connect(信号发出者, 发出信号函数的地址, 信号接收者, 槽函数地址);`
   5. 编写触发信号函数
      1. widget的`.h`下声明
      2. widget的`.cpp`下实现   
      3. 信号函数前需要使用`emit`关键字
   6. 在`widget.cpp`中调用触发函数

3. 实现:

   + teacher

     ``` c++
     #ifndef TEACHER_H
     #define TEACHER_H
     #include <QObject>
     class teacher : public QObject
     {
         Q_OBJECT
     public:
         explicit teacher(QObject *parent = nullptr);
     signals:
         //在signals:作用域下自定义信号(返回void, 可重载, 只需要声明不需要实现)
         void hungry();
     };
     #endif // TEACHER_H
     
     #include "teacher.h"
     teacher::teacher(QObject *parent) : QObject(parent)
     {
     }
     ```

   + student

     ``` c++
     #ifndef STUDENT_H
     #define STUDENT_H
     #include <QDebug>
     #include <QObject>
     class student : public QObject
     {
         Q_OBJECT
     public:
         explicit student(QObject *parent = nullptr);
     signals:
     public slots:
         // 在public slots:作用域下自定义槽函数(返回void, 可重载 ,需要声明也需要实现)
         void treat();
     };
     #endif // STUDENT_H
     
     
     #include "student.h"
     student::student(QObject *parent) : QObject(parent)
     {
     }
     void student::treat(){
         qDebug() << "请客吃饭" ;
     }
     ```

   + widget

     ``` c++
     #ifndef MYWIDGET_H
     #define MYWIDGET_H
     #include "teacher.h"
     #include "student.h"
     #include <QWidget>
     class MyWidget : public QWidget
     {
     public:
         MyWidget(QWidget *parent = nullptr);
         void classIsOver();
         ~MyWidget();
     private:
         teacher *teacher;
         student *student;
     };
     #endif // MYWIDGET_H
     
     
     #include "mywidget.h"
     #include <QPushButton>
     
     //
     /* teacher类
      * student类
      * 下课后:
      *      老师触发一个信号 -> 饿了   并说想吃什么
      *      学生响应信号    -> 请客吃饭
      */
     MyWidget::MyWidget(QWidget *parent)
         : QWidget(parent)
     {
         this->teacher = new class teacher(this);
         this->student = new class student(this);
     
         void (teacher:: *teacherSignal)(QString) = &teacher::hungry;
         void (student:: *studentSlot)(QString) = &student::treat;
         connect(teacher, teacherSignal, student, studentSlot);
         classIsOver();
     }
     void MyWidget::classIsOver(){
         emit teacher->hungry("满汉全席");
     }
     MyWidget::~MyWidget()
     {
     }
     ```


#### 5.3.3 使用按钮联动触发

1. 其他都不变 只更改widget.cpp

   ```c++
   #include "mywidget.h"
   #include <QPushButton>
   //
   /* teacher类
    * student类
    * 点击按钮后:
    *      老师触发一个信号 -> 饿了   并说想吃什么
    *      学生响应信号    -> 请客吃饭
    */
   MyWidget::MyWidget(QWidget *parent)
       : QWidget(parent)
   {
       QPushButton *btn = new QPushButton("触发下课", this);
       this->resize(300,400);
   
       this->teacher = new class teacher(this);
       this->student = new class student(this);
   
       void (teacher:: *teacherSignal)(void) = &teacher::hungry;
       void (student:: *studentSlot)(void) = &student::treat;
       connect(teacher, teacherSignal, student, studentSlot);
       connect(btn, &QPushButton::clicked, teacher, teacherSignal);
   }
   MyWidget::~MyWidget()
   {
   }
   ```



### 5.4 断开

`disconnect(里面参数和连接的一样)`

### 5.5 使用lambda表达式完成带参连接

```c++
#include "mywidget.h"
#include <QPushButton>

//
/* teacher类
 * student类
 * 下课后:
 *      老师触发一个信号 -> 饿了   并说想吃什么
 *      学生响应信号    -> 请客吃饭
 */

MyWidget::MyWidget(QWidget *parent)
    : QWidget(parent)
{
    QPushButton *btn = new QPushButton("触发下课", this);
    this->resize(300,400);

    this->teacher = new class teacher(this);
    this->student = new class student(this);

    void (teacher:: *teacherSignal)(QString) = &teacher::hungry;
    void (student:: *studentSlot)(QString) = &student::treat;
    connect(teacher, teacherSignal, student, studentSlot);

    connect(btn, &QPushButton::clicked, this, [=](){
        emit teacher->hungry("满汉全席");
        this->close();
    });
}
MyWidget::~MyWidget()
{
}
```



## 06_QMainWindow

普通的桌面应用程序有个共同的特性，有菜单栏、工具栏、状态栏、中央窗口等部件。菜单栏其实可以看成是一个窗口，菜单栏中的每一个菜单也可以看成一个窗口，每个部件基本都可以认为是一个窗口。那么这些典型的桌面应用可以认为是一些QWidget的组合，通过QWidget派生的方式也确实能够完成这样的窗口。

 但是如果每次都自己去设计，重复这些工作，想想都有些麻烦，于是Qt帮我们实现这样的窗口，叫做QMainWindow，QMainWindow已经布置好菜单栏、工具栏、状态栏等窗口，我们只需要懂得如何去应用就行了。
<img src="https://img-blog.csdn.net/20180224002659221" style=zoom:90%>

*注：MenuBar是菜单栏，toolbars是工具栏，Dock Widgets是停口窗口，Central Widget是中央窗口，Status Bar是状态栏*

### 6.1 菜单,工具栏, 状态栏, 铆接, 中心部件

```c++
#include "mainwindow.h"

#include <QMenuBar>
#include <QToolBar>
#include <QString>
#include <QPushButton>
#include <QStatusBar>
#include <QLabel>
#include <QDockWidget>
#include <QTextEdit>
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
{

    this->resize(500, 600);

    /*
        菜单栏之多只能有一个

    */

    // 菜单栏创建
    QMenuBar *bar = menuBar();
    // 将菜单栏放入this中
    this->setMenuBar(bar);

    // 创建菜单栏内容
    QMenu *fileMenu = bar->addMenu("文件");
    QMenu *editMenu = bar->addMenu("编辑");

    // 给文件栏添加子目录
    QAction * newFile = fileMenu->addAction("新建");
    // 添加分隔符
    fileMenu->addSeparator();
    QAction * openFile = fileMenu->addAction("打开");
    QAction * saveFile = fileMenu->addAction("保存");
        
    /*
     * 工具栏 可以有多个
    */
    // 创建工具栏
    QToolBar * toolBar = new QToolBar(this);
    // 添加进窗口并设置窗口默认位置
    this->addToolBar(Qt::TopToolBarArea,toolBar);
    // 后期设置只允许 左右停靠
    toolBar->setAllowedAreas(Qt::BottomToolBarArea | Qt::RightToolBarArea);
    // 设置浮动
    toolBar->setFloatable(false);
    // 浮动的总开关  false则不允许移动
    toolBar->setMovable(false);
    // 添加内容
    toolBar->addAction(openFile);
    toolBar->addSeparator();
    QAction * saveFileTool = toolBar->addAction("关闭");
    // 添加组件  按钮
    QPushButton * btnTool = new QPushButton("按钮组件", this);
    toolBar->addWidget(btnTool);

    /*
     * 状态栏 至多有一个
    */
    QStatusBar * stBar = new QStatusBar(this);
    this->setStatusBar(stBar);
    // 放标签组件
    QLabel * label = new QLabel("提示信息",this);
    stBar->addWidget(label);
    QLabel * labelRight = new QLabel("右侧提示", this);
    stBar->addPermanentWidget(labelRight);

    /*
     * 铆接部件 (浮动窗口) 可以有多个
    */
    QDockWidget * dockEle = new QDockWidget("铆接部件", this);
    this->addDockWidget(Qt::LeftDockWidgetArea,dockEle);
    // 设置后期运行停靠的范围
    dockEle->setAllowedAreas(Qt::LeftDockWidgetArea | Qt::NoDockWidgetArea);

    /*
     * 中心部件  只能有一个
    */
    QTextEdit * editEle = new QTextEdit(this);
    this->setCentralWidget(editEle);
}
MainWindow::~MainWindow()
{
}
```

### 6.2 点击按钮弹出对话框

对话框大致分为:

+ 模态对话框:  不可以对原窗口进行操作

+ 非模态对话框:   可以对原窗口进行操作

+ 示例:

  ``` c++
  #include "mainwindow.h"
  #include "ui_mainwindow.h"
  #include <QIcon>
  #include <QDialog>
  #include <QDebug>
  MainWindow::MainWindow(QWidget *parent)
      : QMainWindow(parent)
      , ui(new Ui::MainWindow)
  {
      ui->setupUi(this);
      // 添加qt资源方法 :+前缀名+文件名
      ui->actionnewFile->setIcon(QIcon(":/img/4.jpg"));
      ui->actionopenFile->setIcon(QIcon(":/img/5.jpg"));
  
      /*
       * 对话框分类:  模态对话框(可以对其他窗口进行操作)    非模态对话框(不可以对其他窗口进行操作)
      */
      // 点击新建按钮弹出模态对话框
      connect(ui->actionnewFile,&QAction::triggered,[=](){
          // 模态
          QDialog dlg(this);
          dlg.resize(200,300);
          // 设置模态对话框  阻塞
          dlg.exec();
         qDebug() << "hahahah";
      });
  
      // 非模态 非模态是不阻塞的点击按钮结束后对话框就消失了 所以需要new对象
      // 但是有一些二傻子没事就点开关闭内存不会主动释放  就需要设置属性一次后销毁
      connect(ui->actionopenFile,&QAction::triggered,[=](){
  
         // QDialog dlg1(this);
          QDialog * dlg1 = new QDialog(this);
          dlg1->resize(200,300);
          dlg1->setAttribute(Qt::WA_DeleteOnClose);
          dlg1->show();
          qDebug() << "aaaaa";
      });
  }
  
  MainWindow::~MainWindow()
  {
      delete ui;
  }
  ```

  

**标准对话框具体分类:**

| 对话框                  | 常用静态函数名称                                             | 函数功能                                                     |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| QFileDialog 文件对话框  | QString getOpenFileName() QStringList getOpenFileNames() QString getSaveFileName() QString getExistingDirectory() QUrl getOpenFileUrl() | 选择打开一个文件 选择打开多个文件 选择保存一个文件 选择一个己有的目录 选择打幵一个文件，可选择远程网络文件 |
| QcolorDialog 颜色对话框 | QColor getColor()                                            | 选择颜色                                                     |
| QFontDialog 字体对话框  | QFont getFont()                                              | 选择字体                                                     |
| QinputDialog 输入对话框 | QString getText() int getlnt() double getDouble() QString getltem() QString getMultiLineText() | 输入单行文字 输入整数 输入浮点数 从一个下拉列表框中选择输入 输入多行字符串 |
| QMessageBox 消息框      | S[tan](http://c.biancheng.net/ref/tan.html)dardButton information() StandardButton question() StandardButton waming() StandardButton critical() void about() void aboutQt() | 信息提示对话框 询问并获取是否确认的对话框 警告信息提示对话框 错误信息提示对话框 设置自定义信息的关于对话框 关于Qt的对话框 |



**QMessageBox东翻开;对话框参数:**

+ 参数1:   父亲
+ 参数2:    标题
+ 参数3:    显示内容
+ 参数4:    按键类型
+ 参数5:     默认即就是与回车关联

## 07_添加资源文件

> 例如 图片等等  

**需求: ** 给组件添加图标Icon

**步骤:**

1. 将img等资源文件放至项目根目录
2. qt项目中右键`add new`->`qt`->`Qt Resource File`输入名称(随意)
3. 项目中会自动生成`.qrc`文件
4. 右键`.qrc`文件 -> `open in edit`  在右边界面点击`add prefix`添加前缀(随意 但是尽量有理可循)
5.  `open in edit`  在右边界面点击`add file添加文件 添加img文件下的所有内容

 **实现:**

```c++
#include "mainwindow.h"
#include "ui_mainwindow.h"
#include <QIcon>
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    // 添加qt资源方法 :+前缀名+文件名
    ui->actionnewFile->setIcon(QIcon(":/img/4.jpg"));
    ui->actionopenFile->setIcon(QIcon(":/img/5.jpg"));
}
MainWindow::~MainWindow()
{
    delete ui;
}
```







