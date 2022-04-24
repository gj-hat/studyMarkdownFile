# 监听器

## 01_什么是监听器

​	监听器，字面上的理解就是监听观察某个事件（程序）的发生情况，当被监听的事件真的发生了的时候，事件发生者（事件源） 就会给注册该事件的监听者（监听器）发送消息，告诉监听者某些信息，同时监听者也可以获得一份事件对象，根据这个对象可以获得相关属性和执行相关操作。

​	实现了特定接口的java类,这个java类用于监听另一个类的改变(调用或者属性变化等等),当另一个类发生改变时监听器内的方法就会立即执行。

## 02_监听器的术语

1. 事件源:汽车

2. 监听器:报警器

3. 事件源和监听器绑定:报警器安装在汽车上

4. 事件:触发报警器(踹了一jio)

5. UI编码示例:

   ```java
    public class MyJframe extends JFrame {
       public static void main(String[] args) {
           //1.创建小窗口(事件源)
           MyJframe myJframe = new MyJframe();
           //2. 设置窗口显示
           myJframe.setVisible(true);
           //3.设置窗口宽高
           myJframe.setBounds(400, 180, 600, 500);
           //绑定
           myJframe.addWindowListener(new MyWindowListener());
       }
   }
   class MyWindowListener implements WindowListener {
   
       @Override
       public void windowOpened(WindowEvent e) {
       }
       //监听关闭窗口(监听器)
       @Override
       public void windowClosing(WindowEvent e) {
           //事件
           System.out.println("窗口关闭");
           //关闭虚拟机
           System.exit(0);
       }
       @Override
       public void windowClosed(WindowEvent e) {
       }
       @Override
       public void windowIconified(WindowEvent e) {
       }
       @Override
       public void windowDeiconified(WindowEvent e) {
       }
       @Override
       public void windowActivated(WindowEvent e) {
       }
       @Override
       public void windowDeactivated(WindowEvent e) {
       }
   }
   ```

   