# Python3 开发八股
## Python 设计哲学与理念
*TODO*


## 面对对象 (OOP, Object-Oriented Programming) 相关：

### （一）封装继承多态


### （二）`__init__`和`__new__`



### （三）设计模式

**Q: 讲一下设计模式。**

**Q: 讲一下单例模式，以及 Python 中其实现方式。**
A: 单例模式是一种设计模式，确保一个类只有一个实例，并提供一个全局访问点。

Python 中实现单例模式的方式有多种，常见的有以下几种：
1. 使用模块：Python 模块本身就是单例的，可以直接使用模块来实现单例模式。
2. 使用`__new__`类变量方法：通过在类中定义一个类变量来存储实例，并在 `__new__` 方法中检查是否已经存在实例，如果存在则返回该实例，否则创建一个新的实例。

```python
class Singleton:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kwargs)
        return cls._instance
```

3. 使用装饰器：通过定义一个装饰器来实现单例模式。
```python
def singleton(cls):
    instances = {}

    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]

    return get_instance

@singleton
class MyClass:
    pass
```

4. 使用元类重写`__call__`方法：通过定义一个元类来实现单例模式。
```python
class SingletonMeta(type):
    _instance = None

    def __call__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super(SingletonMeta, cls).__call__(*args, **kwargs)
        return cls._instance

class Singleton(metaclass=SingletonMeta):
    pass
```



### （四）元类
**Q: 讲一下元类。**

A:<br>
Python 中一切皆对象，类也是对象，元类就是用来创建类的对象的，类似于“机床”。

类默认继承`type`元类，我们可以通过自定义元类，控制类的创建过程，动态地修改类的属性和方法，实现一些特殊的功能。

默认由`type`创建类，定义元类需要继承`type`，重写`__new__`或`__init__`方法来实现自定义的类创建逻辑。继承自定义元类时需要指定`metaclass`参数。

举一个简单的例子，定义一个元类来自动为类添加一个`greet`方法：

```python
class GreetMeta(type):
    def __new__(cls, name, bases, attrs):
        attrs['greet'] = lambda self: f"Hello, I am {self.__class__.__name__}!"
        return super().__new__(cls, name, bases, attrs)

class MyClass(metaclass=GreetMeta):
    pass
```

### （五）面向切面编程 (AOP, Aspect-Oriented Programming)
为多个毫无关联的类，添加相同的通用功能。

例如：日志记录（Logging）、权限校验（Authentication）、性能监控（Monitoring）、数据库事务管理（Transaction Management）等通用逻辑，抽象封装成一个个独立的‘切面’。


### （六）装饰器 (decorator)
装饰器的本质是一个高阶函数，Python 中函数本身可以被当作变量传递和返回，装饰器实际上也就是一个*接收函数作为参数，并返回一个新函数的可调用对象*。

实际开发中的作用是无需修改函数具体的内部实现即可添加功能，提高了代码模块化程度，方便开发和后续维护。

具体形式：
```python
from functools import wraps

def time_logger(func):

    @wraps(func)
    # 命名规范：wrapper
    def wrapper(*args, **kwargs):
        #
        #

    return wrapper

@time_logger
def download_file(filename):
    return

```


### （五）其他



## 一、多进程、多线程相关

### （一）进程、线程概念

Q: 真并行与伪并行？解决方案

A:


### （二）锁

gil：全局解释器锁


### （三）异步

Q: 讲一下异步的概念。

A:





### （四）全局变量

**Q: 如何定义全局变量？**

A: 函数外声明的变量即为全局变量，函数如果要访问全局变量，需要在函数内先用`global`声明。





### （五）并行





## 四、生成器和迭代器的区别



## 五、



## 六、上下文管理器



## 七、深浅拷贝



## 八、元类



## 九、垃圾回收机制



## 十、异步编程


## 十一、异常处理


## 其他问题：
**Q: 讲一下魔术方法。**

A:

`is`和`==`的区别

