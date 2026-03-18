# Python3 开发八股
## 一、Python 设计哲学与基础语法
*TODO*

## 二、面对对象 (OOP, Object-Oriented Programming) 相关：

### （一）封装继承多态
**Q: 讲一下封装。**<br>
A: <br>
封装是将属于同一业务的数据和方法组合在一起，隐藏内部实现细节，只暴露必要的接口。

**Q: 讲一下继承。**<br>
A: <br>
继承是子类可以继承父类的属性和方法，从而实现代码的复用。

**Q: 讲一下多态。**<br>
A: <br>
多态是指同一个接口可以被不同的对象以不同的方式实现，具体形式是不同类的实例对象调用同名方法时执行动作不同，提高了代码的灵活性和可扩展性。

### （二）`__init__`和`__new__`
**Q: 讲一下`__init__`和`__new__`的区别。**<br>
A: <br>
- `__new__`是用于创建对象实例的方法，负责在内存中申请空间，接收类作为第一个参数，并传递参数；而`__init__`是用于初始化对象实例的方法，接收实例作为第一个参数，将传入的参数绑定到对象实例上。

- `__new__`在对象创建时调用，而`__init__`在对象初始化时调用。

- `__new__`必须返回一个对象实例，而`__init__`不需要返回值。

- 通常情况下，我们只需要重写`__init__`方法来初始化对象实例，只有在需要控制对象创建过程（实现单例模式）、继承修改不可变类型 (tuple, str) 时才需要重写`__new__`方法。


### （三）设计模式

**Q: 讲一下设计模式。**<br>
A: <br>
面向对象提供了对象、封装、继承、多态等机制，设计模式则是面向对象的补充。

设计模式是如何利用这些机制设计构建一套能被反复使用的代码的最佳实践经验总结。

使用设计模式可以提高代码的可维护性、可扩展性和复用性。

**Q: 讲一下单例模式，以及 Python 中其实现方式。**<br>
A: 单例模式是一种设计模式，能确保一个类在项目运行的全生命周期中只有一个实例，并提供一个全局访问点。

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
**Q: 讲一下元类。**<br>
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
**Q: 讲一下装饰器。**<br>
A: <br>
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

### （七）可迭代对象、迭代器、生成器




### 其他



## 三、多进程、多线程相关

### （一）进程、线程概念

Q: 真并行与伪并行？解决方案

A:


### （二）锁

gil：全局解释器锁


### （三）异步

Q: 讲一下异步的概念。

A:





### （四）全局变量







### （四）并行

上下文管理器




## 七、深浅拷贝



## 九、垃圾回收机制



## 十一、异常处理

## 十一、测试框架


## 十一、日志管理


## 其他问题：
**Q: 讲一下魔术方法。**

A:

**Q: `is`和`==`的区别**

A:


**Q: 如何定义全局变量？**

A: 函数外声明的变量即为全局变量，函数如果要访问全局变量，需要在函数内先用`global`声明。

