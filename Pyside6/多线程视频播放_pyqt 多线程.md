
#### 文章目录

*   [OpenCV-PyQT 项目实战（12）项目案例 08：多线程视频播放](#OpenCVPyQT1208_17)
*   *   [1. 线程处理](#1__23)
    *   *   [1.1 线程与进程](#11__25)
        *   [1.2 为什么使用多线程](#12__41)
        *   [1.3 单线程与多线程的基本例程](#13__69)
    *   [2. Python 自定义线程](#2_Python_162)
    *   *   [2.1 线程的创建步骤](#21__164)
        *   [2.2 通过线程类 threading.Thread() 创建线程对象](#22__threadingThread__177)
        *   [2.4 继承 threading.Thread 来自定义线程类](#24__threadingThread__242)
        *   [2.5 使用 moveToThread 方法](#25__moveToThread__269)
    *   [3. 多线程视频解码和视频播放](#3__385)
    *   *   [3.1 自定义线程类](#31__387)
        *   [3.2 视频解码子程序](#32__429)
        *   [3.3 视频播放子程序](#33__463)
        *   [3.4 主程序](#34__487)
        *   [3.5 UI 界面 uiDemo13.ui](#35_UI__uiDemo13ui_516)
    *   [4. 项目实战：PyQt 多线程视频处理](#4_PyQt_531)
    *   *   [4.1 程序说明](#41__533)
        *   [4.2 完整例程](#42__577)

### 1. 线程处理

#### 1.1 线程与进程

进程（Process）是操作系统进行资源分配和调度运行的基本单位，可以理解为操作系统中正在执行的程序。每个应用程序都有一个自己的进程。

线程是一个基本的 CPU 执行单元，是程序执行的最小单元。 线程自己不拥有系统资源，必须依托于进程存活。一个线程是一个 execution context，即一个 CPU 执行时所需要的一串指令。

每一个进程启动时都会最先产生一个线程，即主线程。然后主线程会再创建其他的子线程。

*   线程与进程的区别：
    *   线程必须在某个进程中执行。
    *   一个进程可包含多个线程，其中有且只有一个主线程。
    *   多线程共享同个地址空间、打开的文件以及其他资源。
    *   多进程共享物理内存、磁盘、打印机以及其他资源。

#### 1.2 为什么使用多线程

线程在程序中是独立的、并发的执行流。与分隔的进程相比，进程中线程之间的隔离程度要小，它们共享内存、文件句柄和其他进程应有的状态。

因为线程的划分尺度小于进程，使得多线程程序的并发性高。进程在执行过程之中拥有独立的内存单元，而多个线程共享内存，从而极大的提升了程序的运行效率。

线程比进程具有更高的性能，这是由于同一个进程中的线程都有共性，多个线程共享一个进程的虚拟空间。线程的共享环境包括进程代码段、进程的共有数据等，利用这些共享的数据，线程之间很容易实现通信。

操作系统在创建进程时，必须为改进程分配独立的内存空间，并分配大量的相关资源，但创建线程则简单得多。因此，使用多线程来实现并发比使用多进程的性能高得要多。

多线程（multithreading）是指从软件或者硬件上实现多个线程并发执行的技术。具有多线程能力的计算机因有硬件支持而能够在同一时间执行多于一个线程，进而提升整体处理性能。

线程从创建到消亡的过程为：

1.  new（新建）：新创建的线程经过初始化后，进入 Runnable 状态。
2.  Runnable（就绪）：等待线程调度。调度后进入运行状态。
3.  Running（运行）。
4.  Blocked（阻塞）：暂停运行，解除阻塞后进入 Runnable 状态重新等待调度。
5.  Dead（消亡）：线程方法执行完毕返回或者异常终止。

从运行状态（Running）进入阻塞状态（Blocked）有三种情况：

*   同步：线程中获取同步锁，但是资源已经被其他线程锁定时，进入 Locked 状态，直到该资源可获取（获取的顺序由 Lock 队列控制）
*   睡眠：线程运行 sleep()或 join()方法后，线程进入 Sleeping 状态。区别在于 sleep 等待固定的时间，而 join 是等待子线程执行完。当然 join 也可以指定一个 “超时时间”。从语义上来说，如果两个线程 a,b, 在 a 中调用 b.join()，相当于合并(join) 成一个线程。最常见的情况是在主线程中 join 所有的子线程。
*   等待：线程中执行 wait() 方法后，线程进入 Waiting 状态，等待其他线程的通知 (notify）。

#### 1.3 单线程与多线程的基本例程

**例程 1：单线程**

```
import time
 
def sayHello():
    print("Hello")
    time.sleep(1) 
 
if __name__ == '__main__':
    for i in range(10):
        sayHello()
```

运行时间为：10 秒。

**例程 2：多线程**

通过多线程可以完成多任务。

```
import time,threading
 
def sayHello():
    print("Hello")
    time.sleep(1)
 
 
if __name__ == '__main__':
    for i in range(10):
        # 创建子线程对象
        thread_obj = threading.Thread(target=sayHello)
 
        # 启动子线程对象
        thread_obj.start()
```

运行时间为：1 秒。

**例程 3：线程的执行顺序**

```
import time,threading
 
def sing():
    for i in range(3):
        print("is singing... %d" % i)
        time.sleep(1)
 
def dance():
    for i in range(3):
        print("is dancing... %d" % i)
        time.sleep(1)
 
 
if __name__ == '__main__':
    print("The main thread starts to execute")
    
    t1 = threading.Thread(target=sing)
    t2 = threading.Thread(target=dance)
 
    t1.start()
    t2.start()
    
    print("Main thread execution completed")
```

运行结果：

```
The main thread starts to execute
is singing... 0
is dancing... 0
Main thread execution completed
is dancing... 1is singing... 1

is singing... 2
is dancing... 2
Process finished with exit code 0
```

主线程执行完毕后，还要等待所有子线程执行完毕后才结束程序运行。

### 2. Python 自定义线程

#### 2.1 线程的创建步骤

```
(1) 导入线程模块
    import threading
(2) 通过线程类创建进程对象
    线程对象 = threading.Thread(target = 任务名) 
(3) 启动线程执行任务
    线程对象.start()
```

#### 2.2 通过线程类 threading.Thread() 创建线程对象

**语法：**

thread.Thread(group=Nore,targt=None,args=(),kwargs={},*,daemon=None)

**参数说明：**

*   group：必须为 None，于 ThreadGroup 类相关，一般不使用。
*   target：线程调用的对象，就是目标函数。
*   name：为线程起这个名字。默认是 Tread-x，x 是序号，由 1 开始，第一个创建的线程名字就是 Tread-1。
*   args：为目标函数传递关键字参数，元组。
*   kwargs：为目标函数传递关键字参数，字典。
*   daemon：用来设置线程是否随主线程退出而退出。

**例程 4：子线程的创建与开启**

线程对象 = threading.Thread(target = 任务名)

```
import threading
 
def run1(n):
    print("current task：run1")
 
def run2(n):
    print("current task：run2")
    
if __name__ == "__main__":
    t1 = threading.Thread(target=run1)  # 创建子线程1
    t2 = threading.Thread(target=run2)  # 创建子线程2
    t1.start()  # 开启线程 t1
    t2.start()  # 开启线程 t2
```

**例程 5：给线程执行的任务传递参数**

线程可以以元组 args 的方式或字典 kwargs 的方式给执行的任务传递参数。

线程对象 = threading.Thread(target = 任务名, args=(arg1,…))  
线程对象 = threading.Thread(target = 任务名, kwargs={k1:num1,…})

```
import threading
 
def run1(n):
    print("current task：", n)
 
def run2(n):
    print("current task：", n)
    
if __name__ == "__main__":
    t1 = threading.Thread(target=run1, args=("thread 1",))  # 创建子线程1
    t2 = threading.Thread(target=run2, args=("thread 2",))  # 创建子线程2
    t1.start()  # 开启线程 t1
    t2.start()  # 开启线程 t2
    print("end")
```

#### 2.4 继承 threading.Thread 来自定义线程类

**例程 6：自定义线程类 MyThread**

```
import threading
import time

class MyThread(threading.Thread):
    def __init__(self, num):
        threading.Thread.__init__(self)
        self.num = num

    def run(self):
        print("current task：", self.num)
        time.sleep(self.num)

if __name__ == "__main__":
    t1 = MyThread(100)  # 创建子线程1
    t2 = MyThread(200)  # 创建子线程2
    t1.start()  # 开启线程 t1
    t2.start()  # 开启线程 t2
    print("end")
```

#### 2.5 使用 moveToThread 方法

moveToThread 函数给多个任务（如显示多个界面）各分配一个线程去执行，避免自定义多个类继承自 QThread 类，从而可以避免冗余。

使用 moveToThread 函数的流程：

1.  创建一个类继承自 QObject 类或其子类，在其中定义所要执行的多个任务。
2.  任务通过 moveToThread 指定所要执行的线程。
3.  线程通过 start 启动。
4.  通过信号与槽机制触发线程的执行。

该方法通过创建一个线程，将创建的线程与类方法进行绑定来实现，相当于在线程中操作类方法。

**例程 6：自定义线程类 MyThread**

```
# coding:UTF-8
from PyQt5 import QtWidgets, QtCore
import sys
from PyQt5.QtCore import *
import time
 
# 继承 QObject
class Runthread(QtCore.QObject):
    #  通过类成员对象定义信号对象
    signal = pyqtSignal(str)
 
    def __init__(self):
        super(Runthread, self).__init__()
        self.flag = True
 
    def __del__(self):
        print ">>> __del__"
 
    def run(self):
        i = 0
        while self.flag:
            time.sleep(1)
            if i <= 100:
                self.signal.emit(str(i))  # 注意这里与_signal = pyqtSignal(str)中的类型相同
                i += 1
        print ">>> run end: "
 
 
class Example(QtWidgets.QWidget):
    #  通过类成员对象定义信号对象
    _startThread = pyqtSignal()
 
    def __init__(self):
        super(Example, self).__init__()
        # 按钮初始化
        self.button_start = QtWidgets.QPushButton('开始', self)
        self.button_stop = QtWidgets.QPushButton('停止', self)
        self.button_start.move(60, 80)
        self.button_stop.move(160, 80)
        self.button_start.clicked.connect(self.start)  # 绑定多线程触发事件
        self.button_stop.clicked.connect(self.stop)  # 绑定多线程触发事件
 
        # 进度条设置
        self.pbar = QtWidgets.QProgressBar(self)
        self.pbar.setGeometry(50, 50, 210, 25)
        self.pbar.setValue(0)
 
        # 窗口初始化
        self.setGeometry(300, 300, 300, 200)
        self.show()
 
        self.myT = Runthread()          # 创建线程对象
        self.thread = QThread(self)     # 初始化QThread子线程
 
        # 把自定义线程加入到QThread子线程中
        self.myT.moveToThread(self.thread)
 
        self._startThread.connect(self.myT.run)     # 只能通过信号-槽启动线程处理函数
        self.myT.signal.connect(self.call_backlog)
 
    def start(self):
        if self.thread.isRunning():     # 如果该线程正在运行，则不再重新启动
            return
 
        # 先启动QThread子线程
        self.myT.flag = True
        self.thread.start()
        # 发送信号，启动线程处理函数
        # 不能直接调用，否则会导致线程处理函数和主线程是在同一个线程，同样操作不了主界面
        self._startThread.emit()
 
    def stop(self):
        if not self.thread.isRunning():     # 如果该线程已经结束，则不再重新关闭
            return
        self.myT.flag = False
        self.stop_thread()
 
    def call_backlog(self, msg):
        self.pbar.setValue(int(msg))  # 将线程的参数传入进度条
 
    def stop_thread(self):
        print ">>> stop_thread... "
        if not self.thread.isRunning():
            return
        self.thread.quit()      # 退出
        self.thread.wait()      # 回收资源
        print ">>> stop_thread end... "
 
 
if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    myshow = Example()
    myshow.show()
    sys.exit(app.exec_())
```

### 3. 多线程视频解码和视频播放

#### 3.1 自定义线程类

```
def openVideo(self):  # 导入视频文件，点击 btn_1 触发
        if self.btn_1.text() == "打开视频":
            # 打开视频文件
            self.videoPath, _ = QFileDialog.getOpenFileName(self, "Open Video", "../images/", "*.mp4 *.avi *.flv")
            print("Open Video: ", self.videoPath)

            # 实例化 cvDecode 类
            self.decodework = cvDecode()
            self.decodework.start()
            self.decodework.threadFlag = True
            self.decodework.changeFlag = True
            self.decodework.videoPath = r"" + self.videoPath

            # 实例化 playVedio 类
            self.playwork = playVedio()
            self.playwork.playLabel = self.label_1  # 设置显示控件 label_1
            self.playwork.threadFlag = True
            self.playwork.playFlag = True  # 控制标识：播放

            # 创建视频播放线程
            self.playthread = QThread()
            self.playwork.moveToThread(self.playthread)
            self.playthread.started.connect(self.playwork.play)  # 线程与类方法进行绑定
            self.playthread.start()  # 启动视频播放线程

            # 视频/摄像头，准备播放
            self.btn_1.setText("关闭视频")
            self.btn_2.setEnabled(True)  # "播放"按钮 可用
            self.btn_3.setEnabled(True)  # "抓拍"按钮 可用
        else:
            self.closeEvent(self.close)  # 关闭线程
            self.btn_1.setText("打开视频")
            self.btn_2.setText("播放视频")
            self.btn_2.setEnabled(False)  # "播放"按钮 不可用
            self.btn_3.setEnabled(False)  # "抓拍"按钮 不可用
```

#### 3.2 视频解码子程序

```
class cvDecode(QThread):  # 视频解码
    def __init__(self):
        super(cvDecode, self).__init__()
        self.threadFlag = False  # 控制线程退出
        self.videoPath = ""      # 视频文件路径P
        self.changeFlag = 0      # 判断视频文件路径是否更改
        self.cap = cv.VideoCapture()

    def run(self):
        while self.threadFlag:  # 线程开启状态
            if self.changeFlag == 1 and self.videoPath !="":
                self.changeFlag = 0
                self.cap = cv.VideoCapture(r""+self.videoPath)

            if self.videoPath !="":
                if self.cap.isOpened():
                    ret, frame = self.cap.read()
                    time.sleep(0.01)   # 读取时间控制，读取视频文件取 0.01，读取实时摄像取 0.001
                    if frame is None:  # 控制循环播放
                        self.cap = cv.VideoCapture(r"" + self.videoPath)
                    if ret:
                        Decode2Play.put(frame)  # 解码后的数据放到队列中
                    del frame  # 释放资源
                else:
                    #   控制重连
                    self.cap = cv.VideoCapture(r"" + self.videoPath)
                    time.sleep(0.01)
```

#### 3.3 视频播放子程序

```
class playVedio(QObject):  # 视频播放类
    def __init__(self):
        super(playVedio, self).__init__()
        self.playLabel = QLabel()   # 初始化QLabel对象
        self.threadFlag = False     # 控制线程退出
        self.playFlag = False       # 控制播放/暂停标识

    def play(self):
        while self.threadFlag:  # 线程开启状态
            if not Decode2Play.empty():
                self.frame = Decode2Play.get()  # 从队列中读取视频帧
                if self.playFlag:  # 当前状态播放
                    image = cv.resize(self.frame, (400, 320))  # 调整为显示尺寸
                    # qImg = self.cvToQImage(frame)  # OpenCV 转为 PyQt 图像格式
                    qImg = QImage(image, image.shape[1], image.shape[0], QImage.Format_RGB888).rgbSwapped()
                    self.playLabel.setPixmap(QPixmap.fromImage(qImg))   #   图像在QLabel上展示
            time.sleep(0.001)
```

#### 3.4 主程序

```
class MyMainWindow(QMainWindow, Ui_MainWindow):  # 继承 QMainWindow 类和 Ui_MainWindow 界面类
    def __init__(self, parent=None):
        super(MyMainWindow, self).__init__(parent)  # 初始化父类
        self.setupUi(self)  # 继承 Ui_MainWindow 界面类

        # 菜单栏
        self.actionOpen.triggered.connect(self.openVideo)  # 连接并执行 openSlot 子程序
        self.actionHelp.triggered.connect(self.trigger_actHelp)  # 连接并执行 trigger_actHelp 子程序
        self.actionQuit.triggered.connect(self.close)  # 连接并执行 trigger_actHelp 子程序
        #
        # # 通过 connect 建立信号/槽连接，点击按钮事件发射 triggered 信号，执行相应的子程序 click_pushButton
        self.btn_1.clicked.connect(self.openVideo)  # 打开视频文件
        self.btn_2.clicked.connect(self.playPause)  # 播放/暂停视频
        self.btn_3.clicked.connect(self.captureFrame)  # 抓拍视频图像
        self.btn_4.clicked.connect(self.imageProcess)  # 图像处理程序
        self.btn_5.clicked.connect(self.close)  # 点击关闭按钮触发：关闭程序
        # self.timerCam.timeout.connect(self.refreshFrame)  # 计时器结束时调用槽函数刷新当前帧

        # 初始化
        self.btn_2.setEnabled(False)  # "播放"按钮 不可用
        self.btn_3.setEnabled(False)  # "抓拍"按钮 不可用
        self.btn_4.setEnabled(False)  # "处理"按钮 不可用
```

#### 3.5 UI 界面 uiDemo13.ui

本例的 UI 继承自 uiDemo4.ui ，并进行修改如下：

![](https://i-blog.csdnimg.cn/blog_migrate/aeb2f1be74789bf36428005b1ade2f44.png#pic_center)

完成了本项目的图形界面设计，将其保存为 uiDemo13.ui 文件。

在 PyCharm 中，使用 PyUIC 将选中的 uiDemo13.ui 文件转换为 .py 文件，就得到了 uiDemo13.py 文件。

### 4. 项目实战：PyQt 多线程视频处理

#### 4.1 程序说明

（1）“打开视频” 按钮用于从文件夹选择播放的视频文件。

![](https://i-blog.csdnimg.cn/blog_migrate/8d1f6d61438432bdb0bc9f1ec34baa1c.png#pic_center)

导入视频前，“暂停播放”、”抓拍图像 “、” 处理图像“ 按钮都不可用。

（2）“播放” 按钮用于播放打开的视频文件，播放结束后自动关闭。

导入视频后开始播放，“暂停播放”、”抓拍图像 “按钮可用，” 处理图像“不可用。

![](https://i-blog.csdnimg.cn/blog_migrate/a0cdef224c2f42db40234434634f81d9.png#pic_center)

（3）“暂停 / 播放”按钮用于暂停 / 播放视频文件。按钮初始显示为 “暂停播放”，按下“暂停” 按钮后暂停播放，按钮显示切换为 “播放视频”；再次按下“播放” 按钮后继续播放，按钮显示切换为“暂停播放”。

（4）” 抓拍图像 “按钮用于抓拍图像，并显示在窗口右侧的显示控件 Label_2。左侧窗口的视频播放不受影响。

![](https://i-blog.csdnimg.cn/blog_migrate/af0aed11e0be99b9fa780e1ba529d489.png#pic_center)

抓拍图像完成后，“图像处理” 按钮可用。

（5）” 图像处理 “按钮用于处理所抓拍图像，并将处理后图像显示在窗口右侧的显示控件 Label_2。

![](https://i-blog.csdnimg.cn/blog_migrate/04010b2f9c326393e411bfb382cc1f7f.png#pic_center)

为了简化例程，本例中的图像处理仅将抓拍图像转换为灰度图像进行显示。在实际应用中，也可以根据需要编写图像处理程序。

由于采用多线程处理机制，不论图像处理程序耗时如何，都不会影响左侧窗口中的视频播放。

#### 4.2 完整例程

```
# OpenCVPyqt13.py
# Demo05 of GUI by PyQt5
# Copyright 2023 Youcans, XUPT
# Crated：2023-03-02

import sys, time
from PyQt5.QtWidgets import *
from PyQt5.QtCore import QObject, QThread, Qt
from PyQt5.QtGui import *
import cv2 as cv
from queue import Queue
from uiDemo13 import Ui_MainWindow  # 导入 uiDemo10.py 中的 Ui_MainWindow 界面类
Decode2Play = Queue()

class cvDecode(QThread):  # 视频解码
    def __init__(self):
        super(cvDecode, self).__init__()
        self.threadFlag = False  # 控制线程退出
        self.videoPath = ""      # 视频文件路径P
        self.changeFlag = 0      # 判断视频文件路径是否更改
        self.cap = cv.VideoCapture()

    def run(self):
        while self.threadFlag:  # 线程开启状态
            if self.changeFlag == 1 and self.videoPath !="":
                self.changeFlag = 0
                self.cap = cv.VideoCapture(r""+self.videoPath)

            if self.videoPath !="":
                if self.cap.isOpened():
                    ret, frame = self.cap.read()
                    time.sleep(0.01)   # 读取时间控制，读取视频文件取 0.01，读取实时摄像取 0.001
                    if frame is None:  # 控制循环播放
                        self.cap = cv.VideoCapture(r"" + self.videoPath)
                    if ret:
                        Decode2Play.put(frame)  # 解码后的数据放到队列中
                    del frame  # 释放资源
                else:
                    #   控制重连
                    self.cap = cv.VideoCapture(r"" + self.videoPath)
                    time.sleep(0.01)

class playVedio(QObject):  # 视频播放类
    def __init__(self):
        super(playVedio, self).__init__()
        self.playLabel = QLabel()   # 初始化QLabel对象
        self.threadFlag = False     # 控制线程退出
        self.playFlag = False       # 控制播放/暂停标识

    def play(self):
        while self.threadFlag:  # 线程开启状态
            if not Decode2Play.empty():
                self.frame = Decode2Play.get()  # 从队列中读取视频帧
                if self.playFlag:  # 当前状态播放
                    image = cv.resize(self.frame, (400, 320))  # 调整为显示尺寸
                    # qImg = self.cvToQImage(frame)  # OpenCV 转为 PyQt 图像格式
                    qImg = QImage(image, image.shape[1], image.shape[0], QImage.Format_RGB888).rgbSwapped()
                    self.playLabel.setPixmap(QPixmap.fromImage(qImg))   #   图像在QLabel上展示
            time.sleep(0.001)

class MyMainWindow(QMainWindow, Ui_MainWindow):  # 继承 QMainWindow 类和 Ui_MainWindow 界面类
    def __init__(self, parent=None):
        super(MyMainWindow, self).__init__(parent)  # 初始化父类
        self.setupUi(self)  # 继承 Ui_MainWindow 界面类

        # 菜单栏
        self.actionOpen.triggered.connect(self.openVideo)  # 连接并执行 openSlot 子程序
        self.actionHelp.triggered.connect(self.trigger_actHelp)  # 连接并执行 trigger_actHelp 子程序
        self.actionQuit.triggered.connect(self.close)  # 连接并执行 trigger_actHelp 子程序
        #
        # # 通过 connect 建立信号/槽连接，点击按钮事件发射 triggered 信号，执行相应的子程序 click_pushButton
        self.btn_1.clicked.connect(self.openVideo)  # 打开视频文件
        self.btn_2.clicked.connect(self.playPause)  # 播放/暂停视频
        self.btn_3.clicked.connect(self.captureFrame)  # 抓拍视频图像
        self.btn_4.clicked.connect(self.imageProcess)  # 图像处理程序
        self.btn_5.clicked.connect(self.close)  # 点击关闭按钮触发：关闭程序
        # self.timerCam.timeout.connect(self.refreshFrame)  # 计时器结束时调用槽函数刷新当前帧

        # 初始化
        self.btn_2.setEnabled(False)  # "播放"按钮 不可用
        self.btn_3.setEnabled(False)  # "抓拍"按钮 不可用
        self.btn_4.setEnabled(False)  # "处理"按钮 不可用

    def openVideo(self):  # 导入视频文件，点击 btn_1 触发
        if self.btn_1.text() == "打开视频":
            # 打开视频文件
            self.videoPath, _ = QFileDialog.getOpenFileName(self, "Open Video", "../images/", "*.mp4 *.avi *.mov")
            print("Open Video: ", self.videoPath)

            # 实例化 cvDecode 类
            self.decodework = cvDecode()
            self.decodework.start()
            self.decodework.threadFlag = True
            self.decodework.changeFlag = True
            self.decodework.videoPath = r"" + self.videoPath

            # 实例化 playVedio 类
            self.playwork = playVedio()
            self.playwork.playLabel = self.label_1  # 设置显示控件 label_1
            self.playwork.threadFlag = True
            self.playwork.playFlag = True  # 控制标识：播放

            # 创建视频播放线程
            self.playthread = QThread()
            self.playwork.moveToThread(self.playthread)
            self.playthread.started.connect(self.playwork.play)  # 线程与类方法进行绑定
            self.playthread.start()  # 启动视频播放线程

            # 视频/摄像头，准备播放
            self.btn_1.setText("关闭视频")
            self.btn_2.setEnabled(True)  # "播放"按钮 可用
            self.btn_3.setEnabled(True)  # "抓拍"按钮 可用
        else:
            self.closeEvent(self.close)  # 关闭线程
            self.btn_1.setText("打开视频")
            self.btn_2.setText("播放视频")
            self.btn_2.setEnabled(False)  # "播放"按钮 不可用
            self.btn_3.setEnabled(False)  # "抓拍"按钮 不可用

    def playPause(self):  # 暂停/播放控制，点击 btn_2 触发
        if self.btn_2.text() == "暂停播放":
            self.playwork.playFlag = False  # 控制标识：暂停
            self.btn_2.setText("播放视频")
        else:
            self.btn_2.setText("暂停播放")
            self.playwork.playFlag = True  # 控制标识：播放

    def captureFrame(self):  # 抓拍视频图像，点击 btn_3 触发
        wLabel, hLabel = 400, 320
        self.image = cv.resize(self.playwork.frame, (wLabel, hLabel))  # 调整为显示尺寸
        qImg = QImage(self.image, wLabel, hLabel, QImage.Format_RGB888).rgbSwapped()  # OpenCV 转为 PyQt 图像格式
        self.label_2.setPixmap((QPixmap.fromImage(qImg)))  # 加载 PyQt 图像
        self.btn_4.setEnabled(True)  # "处理"按钮 可用

    def imageProcess(self):  # 抓拍视频图像，点击 btn_4 触发
        gray = cv.cvtColor(self.image, cv.COLOR_BGR2GRAY)  # 转为灰度图像
        row, col, pix = gray.shape[0], gray.shape[1], gray.strides[0]
        qImg = QImage(gray.data, col, row, pix, QImage.Format_Indexed8)
        # qImg = QImage(self.image, row, col, QImage.Format_RGB888)  # OpenCV 转为 PyQt 图像格式
        self.label_2.setPixmap((QPixmap.fromImage(qImg)))  # 加载 PyQt 图像

    def closeEvent(self, event):  # 关闭线程
        print("关闭线程")  # 先退出循环才能关闭线程
        if self.decodework.isRunning():  # 关闭解码
            self.decodework.threadFlag = False
            self.decodework.quit()
        if self.playthread.isRunning():  # 关闭播放线程
            self.playwork.threadFlag = False
            self.playthread.quit()

    def refreshShow(self, img, label):  # 刷新显示图像
        qImg = self.cvToQImage(img)  # OpenCV 转为 PyQt 图像格式
        label.setPixmap((QPixmap.fromImage(qImg)))  # 加载 PyQt 图像
        return

    def trigger_actHelp(self):  # 动作 actHelp 触发
        QMessageBox.about(self, "About",
                          """多线程视频播放器 v1.0\nCopyright YouCans, XUPT 2023""")
        return

if __name__ == '__main__':
    app = QApplication(sys.argv)  # 在 QApplication 方法中使用，创建应用程序对象
    myWin = MyMainWindow()  # 实例化 MyMainWindow 类，创建主窗口
    myWin.show()  # 在桌面显示控件 myWin
    sys.exit(app.exec_())  # 结束进程，退出程序
```

**【本节完】**

Copyright 2023 youcans, XUPT

Crated：2023-03-02