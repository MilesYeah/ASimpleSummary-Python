# Python 的特点和优点

Python 是一门开源的解释性语言，相比 Java C++ 等语言，Python 具有动态特性，非常灵活。


## Python 运行速度慢的原因
1. Python 不是强类型的语言，所以解释器运行时遇到变量以及数据类型转换、比较操作、引用变量时都需要检查其数据类型。
2. Python 的编译器启动速度比 JAVA 快，但几乎每次都要启动编译。
3. Python 的对象模型会导致访问内存效率变低。Numpy 的指针指向缓存区数据的值，而 Python 的指针指向缓存对象，再通过缓存对象指向数据

## Python 慢的问题，有什么解决办法?
1. 可以使用其他的解释器，比如 PyPy 和 Jython 等。
2. 如果对性能要求较高且静态类型变量较多的应用程序，可以使用 CPython。
3. 对于 IO 操作多的应用程序，Python 提供 asyncio 模块提高异步能力。

