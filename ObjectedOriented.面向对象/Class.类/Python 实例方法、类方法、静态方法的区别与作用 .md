
# [Python 实例方法、类方法、静态方法的区别与作用](https://www.cnblogs.com/wcwnina/p/8644892.html)

Python中至少有三种比较常见的方法类型，即实例方法，类方法、静态方法。它们是如何定义的呢？如何调用的呢？它们又有何区别和作用呢？且看下文。

首先，这三种方法都定义在类中。下面我先简单说一下怎么定义和调用的。（PS：实例对象的权限最大。）

1. 实例方法
    定义：第一个参数必须是实例对象，该参数名一般约定为“self”，通过它来传递实例的属性和方法（也可以传类的属性和方法）；  
    调用：只能由实例对象调用。  

2. 类方法
    定义：使用装饰器@classmethod。第一个参数必须是当前类对象，该参数名一般约定为“cls”，通过它来传递类的属性和方法（不能传实例的属性和方法）；  
    调用：实例对象和类对象都可以调用。  

3. 静态方法
    定义：使用装饰器@staticmethod。参数随意，没有“self”和“cls”参数，但是方法体中不能使用类或实例的任何属性和方法；  
    调用：实例对象和类对象都可以调用。  


## 实例方法

简而言之，实例方法就是类的实例能够使用的方法。这里不做过多解释。


## 类方法

使用装饰器@classmethod。

原则上，类方法是将类本身作为对象进行操作的方法。假设有个方法，且这个方法在逻辑上采用类本身作为对象来调用更合理，那么这个方法就可以定义为类方法。另外，如果需要继承，也可以定义为类方法。

如下场景：

假设我有一个学生类和一个班级类，想要实现的功能为：
    执行班级人数增加的操作、获得班级的总人数；
    学生类继承自班级类，每实例化一个学生，班级人数都能增加；
    最后，我想定义一些学生，获得班级中的总人数。

思考：这个问题用类方法做比较合适，为什么？因为我实例化的是学生，但是如果我从学生这一个实例中获得班级总人数，在逻辑上显然是不合理的。同时，如果想要获得班级总人数，如果生成一个班级的实例也是没有必要的。
复制代码
```py
class ClassTest(object):
    __num = 0

    @classmethod
    def addNum(cls):
        cls.__num += 1

    @classmethod
    def getNum(cls):
        return cls.__num

    # 这里我用到魔术方法__new__，主要是为了在创建实例的时候调用累加方法。
    def __new__(self):
        ClassTest.addNum()
        return super(ClassTest, self).__new__(self)


class Student(ClassTest):
    def __init__(self):
        self.name = ''

a = Student()
b = Student()
print(ClassTest.getNum())
```
复制代码


## 静态方法

使用装饰器@staticmethod。

静态方法是类中的函数，不需要实例。静态方法主要是用来存放逻辑性的代码，逻辑上属于类，但是和类本身没有关系，也就是说在静态方法中，不会涉及到类中的属性和方法的操作。可以理解为，静态方法是个独立的、单纯的函数，它仅仅托管于某个类的名称空间中，便于使用和维护。

譬如，我想定义一个关于时间操作的类，其中有一个获取当前时间的函数。
复制代码
```py
import time

class TimeTest(object):
    def __init__(self, hour, minute, second):
        self.hour = hour
        self.minute = minute
        self.second = second

    @staticmethod
    def showTime():
        return time.strftime("%H:%M:%S", time.localtime())


print(TimeTest.showTime())
t = TimeTest(2, 10, 10)
nowTime = t.showTime()
print(nowTime)
```
复制代码

如上，使用了静态方法（函数），然而方法体中并没使用（也不能使用）类或实例的属性（或方法）。若要获得当前时间的字符串时，并不一定需要实例化对象，此时对于静态方法而言，所在类更像是一种名称空间。

其实，我们也可以在类外面写一个同样的函数来做这些事，但是这样做就打乱了逻辑关系，也会导致以后代码维护困难。

以上就是我对Python的实例方法，类方法和静态方法之间的区别和作用的简要阐述。

 

　　至此，转载请注明出处。

