
一、基本环境准备

1.安装 python3 的 pip3       

```
sudo apt update
sudo apt install python3-pip
```

        之后可以使用 pip3 指令安装 python3 的包

二、安装 pyside6

2.1 安装

```
 sudo pip3 install pyside6 -i https://pypi.tuna.tsinghua.edu.cn/simple 
```

        -i 后面使用了清华源，默认的太慢了

        使用 sudo，安装路径就是系统目录，不用 sudo 会安装再. local 内。我用了 sudo，安装后有警告提示，后续有问题再重装。

![](https://i-blog.csdnimg.cn/blog_migrate/5f3b9ca731f2e39a5557dbc41f3ea482.png)

2.2 打开 pyside6-designer

        终端 pyside6-designer，不能直接 designer 指令。打开后如下图，安装正确。

```
pyside6-designer
```

![](https://i-blog.csdnimg.cn/blog_migrate/82ce669d1dea73463d3d85132b41c6ea.png)

三、安装 vscode

3.1 安装 vscode

        官网下载 deb 包，安装

3.2 安装 3 个插件

        Python，Python debugger，都是 python 编程用的

![](https://i-blog.csdnimg.cn/blog_migrate/ecdb0c1606fc09926da652eadffd7079.png)

        然后是 PYQT Integration 插件

![](https://i-blog.csdnimg.cn/blog_migrate/0f44fea299c86621c5b6f1d5737449a7.png)

3.2 配置 PYQT Integration 插件 

![](https://i-blog.csdnimg.cn/blog_migrate/b445c8311ed05355b964eab9ea0509a9.png)

        需要配置 4 个，如图：        

![](https://i-blog.csdnimg.cn/blog_migrate/14bb91462706ca62e62934df29f9df57.png)

![](https://i-blog.csdnimg.cn/blog_migrate/ad34e95e60eaa1c44b014c6dd17b259b.png)

        具体配置信息都是具体文件的全路径，除了最后的 Path，其他都有默认的，都是 pyqt5 的，pyside6 都换了名字，具体是：  
                - pylupdate：pyside6-lupdate  
                - pyrcc：pyside6-rcc   
                - pyuic：pyside6-uic  
                - qtdesigner：pyside6-designer

        【使用 whereis 指令查找真实路径后填入，使用 Ctrl+Shift+C 复制终端里的文本】        

![](https://i-blog.csdnimg.cn/blog_migrate/0fba1fb39b17bed3120f78d70b47a2e4.png)

填入后

![](https://i-blog.csdnimg.cn/blog_migrate/5b776b693b0be1fd3edf014892ecbca3.png)

       - pyside6-rcc  // 编译资源文件的，可以改后面的 Filepath 配置来修改转换后的文件名，我没改  
       - pyside6-uic  // 将 UI 界面编译成 python 代码的，改界面后必须编译一次更新的，可以修改后面的 Filpath 配置修改转换后的文件名，我没改，默认编译后明明为 Ui_<ui 文件名>.py

四、安装 python3 打包程序

        清华源，速度飞起，国外源太慢了

```
sudo pip3 install pyinstaller -i https://pypi.tuna.tsinghua.edu.cn/simple
```

        打包后，将源码打包成一个完整的可执行文件，有 2 中方式：        

            - pyinstaller xxx.py，生成执行文件，附带一个文件夹  
            - pyinstaller -F xxx.py， 合成一个，单个文件在其他机器上可运行

        pyinstaller 只是将代码打包成本平台的执行文件，不会交叉编译成其他平台程序的。有的 blog 写的 ubuntu 打包成 exe，但这就是一个说法，不是真的打包成 windows 平台的. exe 文件。

五、测试例程

5.1 基本测试        

        vscode 打开文件夹，空白处右键，菜单里出现 PYQT（插件通用的）的选项，空白文件夹只有新建，有 UI 后会有新建、修改和编译。

![](https://i-blog.csdnimg.cn/blog_migrate/4e78ec87a43eb50c629c597baab631b5.png)

        点击后，打开了 Qt designer，创建一个 widget，随便填几个控件

![](https://i-blog.csdnimg.cn/blog_migrate/aec176341898a29ad6b8b74e9dc46aa4.png)

![](https://i-blog.csdnimg.cn/blog_migrate/990fb1cea1ff89c929b2d79bc9d7ebd3.png)

保存到 ui 文件（例如明明 ui.ui）以后，右键 ui.ui，编译，会生成 Ui_ui.py，里面创建了窗口类。

![](https://i-blog.csdnimg.cn/blog_migrate/41a06bedf3603b293aad6d9a6589892d.png)

![](https://i-blog.csdnimg.cn/blog_migrate/3456a0619089de7cdac6e558f4c80e37.png)

        新建一个 py 文件，例如 testwidget.py，python 语言语法。pyside6 的基本调用结构，与 pyqt 是一样的。类 Ui_Form 就是之前绘制的窗口类。

```
'''
 testwidget.py
'''
 
from Ui_ui import Ui_Form
from PySide6.QtWidgets import QApplication, QWidget
 
 
class MyWindow(QWidget, Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)
 
 
if __name__ == '__main__':
    app = QApplication([])
    win = MyWindow()
    win.show()
    app.exec()
```

        运行 testwidget.py，效果如下，成功运行~~

![](https://i-blog.csdnimg.cn/blog_migrate/f66909cee3f5ccbd5d3eca261dc7f56e.png)

        5.2 试试信号和槽功能

        窗口内控件名称都是默认的，修改 testwidget.py

```
'''
 testwidget.py
'''
 
from Ui_ui import Ui_Form
from PySide6.QtWidgets import QApplication, QWidget
 
 
class MyWindow(QWidget, Ui_Form):
    def __init__(self):
        super().__init__()
        self.setupUi(self)
        self.count = 0
        self.bind()
    
    def bind(self):
        self.pushButton.clicked.connect(self.setshowtext)
        self.pushButton.clicked.connect(self.increasecount)
        self.dial.valueChanged.connect(self.setshowvalue)
    
 
    def setshowtext(self):
        self.label.setText('Hello pyside6! ' + str(self.count))
 
    def increasecount(self):
        self.count = self.count + 1
    
    def setshowvalue(self):
        self.lcdNumber.display(self.dial.value())
 
 
if __name__ == '__main__':
    app = QApplication([])
    win = MyWindow()
    win.show()
    app.exec()
```

运行之后，点击按钮，右边 label 出现文本及点击次数；转动旋钮，右边 LCD 显示对应数字。

![](https://i-blog.csdnimg.cn/blog_migrate/e980994f73b35404ec3660760798d5f3.png)

        功能完善~~

5.3 测试打包

        testwidgete.py 所在文件夹内打开终端：

```
pyinstaller -F testwidget.py
```

        结果再 dist 文件夹内，双击运行：

![](https://i-blog.csdnimg.cn/blog_migrate/27553a4f0d9177b924ab7af18c12e164.png)

![](https://i-blog.csdnimg.cn/blog_migrate/105cf2a2f99c05ba3fc24066b4666fe7.png)



