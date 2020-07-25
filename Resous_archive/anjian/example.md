# 官方案例

## Delay
Delay 的意思是延时，我们做的任何操作，都是在时间轴上的，第一秒做了什么，第二秒又做了什么，在代码中，就是用延时来给手机反应的时间,Delay 后面的参数为时间，单位为毫秒，即Delay 1000，是为延时1000毫秒（也就是1秒）的意思。

```
Delay 1000 //延时1000毫秒，也就是1秒再往下执行
```

## Touch、Tap 与 KeyPress 三者间的恩怨
游戏或者应用中，最基础的操作方式就是点击，一般情况下一个点击操作究竟有这几个步骤：
* 1、触摸屏幕中的一个位置；
* 2、经过一小段时间的延时；
* 3、放手；

用代码表示即为：

```
TouchDown x, y, 1 //x,y为坐标，也就是点击屏幕的位置
Delay 50
TouchUp 1
```

一个点击操作就如此麻烦，有没有简便的操作呢？答案是有的，我们可以用 Tap 替换上面的3行代码。

```
Tap x, y
```

而 KeyPress，作为PC端最常见的输入手段，在安卓中也是有的，常用的安卓三键（主菜单，主页，返回）就是使用 KeyPress 来点击。分别为：

```
KeyPress "Menu"
KeyPress "Home"
KeyPress "Back"
```

## Swipe 和 Touch 也有一腿
游戏或者应用中常出现的操作——划屏，简单粗暴的使用 Swipe 进行。

```
Swipe 10, 10, 100, 100, 300 //从坐标10,10划动到坐标100,100，历时300毫秒
```

然而很多情况下，我们希望能够长按一段时间再进行移动（比如拖动软件位置时），又或者希望能滑动后停顿一段时间再抬起，防止列表由于惯性又滑动了一段距离（很多游戏都这样），Swipe 并不适合这种情况，那么我们就需要使用 Touch 命令来解决这个问题，直接贴代码：

```
TouchDown 10, 10, 1 // 在坐标10，10处按下
Delay 1000
TouchMove 100, 100, 1, 300 // 拖动到坐标100，100处，历史300毫秒
Delay 1000
TouchUp 1
```

当然，这样的一段代码我们可以封装成一个函数，方便调用。

```
Function SwipeByDzc(StartX, startY, EndX, EndY, LastTime1, LastTime2, MoveTime)
TouchDown StartX, StartY, 1
Delay LastTime1
TouchMove EndX, EndY, 1, MoveTime
Delay LastTime2
TouchUp 1
End Function
```

那下次再出现类似场景时便可以直接使用函数调用的方式执行类似操作：

```
SwipeByDzc 10, 10, 100, 100, 1000, 1000, 300
```

## ShowMessage 和 TracePrint 异同
官方例子中，我们除了看到各种操作外，还看到了每一个操作附带的提示信息，这种短暂的提示，对于使用者来说相当友好，也方便自己开发时用来显示调试内容。当然，如果用来做调试，ShowMessage 并不是一个好选择，我们来看看他的兄弟--TracePrint。作为开发脚本的作者，游戏中的逻辑相当之多，一旦代码量上来了，运行的时候就需要在各种分支或者操作加上 TracePrint 的调试信息，方便我们知道脚本”跑”到哪了。

```
KeyPress "Menu"
TracePrint "点击了菜单键"

KeyPress "Home"
TracePrint "点击了主界面键"

KeyPress "Back"
TracePrint "点击了返回键"
```

而且，我们还可以在输出调试信息时输出变量值：

```
Dim 循环次数 = 0
Do
Delay 1000
循环次数 = 循环次数+1
TracePrint “循环次数：”&循环次数&“次”
Loop
```
