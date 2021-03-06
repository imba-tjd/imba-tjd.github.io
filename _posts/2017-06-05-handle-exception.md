---
title: 异常处理
---

StackOverflowException无法被catch。

如果异常发生时不在try块内或该try语句没有匹配的catch子句，CLR将搜索调用栈，寻找带匹配的catch子句的封装try语句。如果找到，回到调用栈的顶端，沿着调用栈，执行任何封装的try语句的finally子句，并从栈中弹出每个栈帧。然后执行匹配的catch子句，如果有finally子句，执行。然后在try语句之后继续执行。

finally中不能有return。

Best Practice
-------------

作为库的作者

* 违反约定时抛出异常；可以提供一些API，让调用库的人用普通的方法控制程序的流程，比如Exists这样的测试方法或者Try...方法
* 一般如果发生了严重影响程序的错误，比如数据库发生了有关完整性的错误，就应该抛出异常，否则问题会越来越严重
* 确实有必要分开处理的异常才表示成不同的异常类，否则可以继承同一个异常类

### 使用者

* 只在catch中进行错误的处理、恢复，不要控制程序流
* 只catch能处理的异常（比如连接超时），不能处理的（比如权限问题）就在main里catch，备份好用户数据，调用API生成dump，发回公司的服务器，然后让程序结束
* 避免catch中在引发新的异常，比如记录日志的时候出现IO异常
* 7.0后，throw变为表达式，可以使用null ?? throw new Exception()这样的语句了
* 不要catch总的Exception类然后switch(e.TargetSite.Name)等可能发生变化的东西
* 如果catch后只是做记录，想继续网上抛要用`throw`而不是`throw e`，后者是重新引发异常，导致调用栈丢失

异常的强保证
------------

* 如果某操作抛出异常，那么应用程序的状态必须和执行该操作之前相同。即要么不做，要么做完
* 会有一定的性能损失
* 对于值类型，可以先把数据copy一份，操作完了再替换
* 对于引用类型，比如List，如果执行前，别的地方获得了引用，单纯的替换无法影响原引用。必须把原来的内容删掉后再添加进来，一般clear后foreach添加即可
* 也可以用信封信纸模式。别的地方获得的是信封的引用，信封中包含真实的集合；关键是信封需要实现IList\<元素\>，直接return真实集合对应的方法；
* No throw保证：析构器，Dispose，exception的when，多播委托和事件处理程序（异常会直接中断，后续不会调用）

异常筛选器：catch后的when
-------------------------

* 传统的catch只能判断类型，用when可以进行其他因素的判断，避免了捕获后判断再抛出的过程。例如http错误码，HResult
* 合理利用when的副作用：因为是顺序判断的，可以写一个永远返回false的函数，里面只做log，添加到所有catch之前；或者可以改变输出文字的颜色，向控制台输出信息
* 还可以产生只非在debug时catch，否则不catch的效果，一遍排查故障：!System.Diagnostics.Debugger.IsAttached

## System.Diagnostics.Debug/Trace

* Trace.Listeners是一个静态列表，程序运行后自动产生一个DefaultTraceListener，可取出来指定LogFileName，之后Trace.Print()既可以记录日志了
* Debug.Assert不会导致程序结束。WinForm下会产生对话框，可直接点ignore，可使用listener.AssertUiEnabled=False关掉
* System.Diagnostics.Debugger.Break()相当于加断点
