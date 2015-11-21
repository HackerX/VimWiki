# PyQt(PySide)

PyQt是基于GPL协议的QT库，另一个QT库是PySide，基于LGPL协议，推荐。

## 控件


### lineEdit、textEdit控件

	setText()#设置控件文本

	lineEdit.text()#获取lineEdit控件文本

	textEdit.toText()#获取textEdit控件文本

### 设置窗口透明度

    self.setWindowOpacity(0.85)

## 信号槽

## 线程

* PYQT线程

```python
class Worker(QThread):
    def __init__(self,parent=None):
        super(Worker,self).__init__(parent)
        self.working=True
    def __del__(self):
        self.working=False
        self.wait()
    def run(self):
```

## 模块化设计

使用QT Designer设计UI时，将UI代码作为模块导入，能避免因修改UI导致实际代码被覆盖。

### 例子

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from PyQt4.QtGui import QMainWindow
from PyQt4.QtCore import pyqtSignature
from PyQt4.QtCore import *
from PyQt4.QtGui import *

from Ui_Main import Ui_MainWindow

class MainWindow(QMainWindow, Ui_MainWindow):

    def __init__(self, parent = None):
        QMainWindow.__init__(self, parent)
        self.setupUi(self)
        self.setAcceptDrops(True)

        self.setWindowTitle('Title')
        #self.setWindowOpacity(0.85)

if __name__ == "__main__":
    import sys
    from PyQt4 import QtCore, QtGui
    app = QtGui.QApplication(sys.argv)
    MainWindow = MainWindow()
    MainWindow.show()
    sys.exit(app.exec_())
```

## 实时界面

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from PyQt4.QtCore import *
from PyQt4.QtGui import *
import sys, os, time

class Test(QDialog):
    def __init__(self,parent=None):
        super(Test,self).__init__(parent)
        self.thread=Worker()
        self.listFile=QListWidget()
        self.btnStart=QPushButton('Start')
        layout=QGridLayout(self)
        layout.addWidget(self.listFile,0,0,1,2)
        layout.addWidget(self.btnStart,1,1)
        self.connect(self.btnStart,SIGNAL('clicked()'),self.slotStart)
        self.connect(self.thread,SIGNAL('output(QString)'),self.slotAdd)#接收线程返回字符，传递给soltAdd函数
    def slotAdd(self,file_inf):#接收一个字符串参数
        self.listFile.addItem(file_inf)#list添加一项
    def slotStart(self):
        self.btnStart.setEnabled(False)#按钮设置为灰色
        self.thread.start()#线程开始

class Worker(QThread):
    def __init__(self,parent=None):
        super(Worker,self).__init__(parent)
        self.working=True
        self.num=0
    def __del__(self):
        self.working=False
        self.wait()
    def run(self):
        while self.working==True:
            file_str='File index {0}'.format(self.num)
            self.num+=1
            self.emit(SIGNAL('output(QString)'),file_str)
            self.sleep(3)

app=QApplication(sys.argv)
dlg=Test()
dlg.show()
sys.exit(app.exec_())
```
