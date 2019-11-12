## 1、__new__方法的作用
1. __new__ 是在一个对象实例化的时候所调用的第一个方法
2. 它的第一个参数是这个类，其他的参数是用来直接传递给 __init__ 方法
3. __new__ 决定是否要使用该 __init__ 方法，因为 __new__ 可以调用其他类的构造方法或者直接返回别的实例对象来作为本类的实例，如果 __new__ 没有返回实例对象，则 __init__ 不会被调用
4. __new__ 创建一个对象实例，但是这个实例没有任何属性，并返回这个实例， __init__会对这个实例进行赋予相应的实例属性

## 2、__init__方法的作用

将__new__方法的返回实例对象（没有属性的实例对象）赋予相应的属性

如何说明这一点呢？
### 分析
```python
class Person(object):

    def __new__(cls,age, name):
        new_obj = object.__new__(cls)
        print(dir(new_obj))   # 注意new出来的实例与最终的实例化对象dir()的区别
        return new_obj

    def __init__(self,age, name):
        self.age = age
        self.name = name
    
    def get_name(self):
        return self.name
```
运行结果如下：
```python
In [30]: xiaoming = Person(18, 'xiaoming')
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'get_name']

In [31]: print(dir(xiaoming))
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'age', 'get_name', 'name']
```
- 可以看出__new__创建出实例是，该实例已经拥有自身的方法（包括用户自定义的’get_name‘）
- 通过__init__之后，创建的实例对象具备了 age与name属性

### 用途与实例
- 通过__new__与__init__可以构建单例（一个进程中只会创建一个实例对象）

**思路**：让__new__的返回值始终是唯一的实例对象，然后如果该实例已经有了对用属性就不在让__init__对其进行赋予属性

**验证**：对单例进行实例化两次，查看两个实例对象的id是否一致，如果一致查看两实例对象的属性值是否一致

```python
class Person(object):

    def __new__(cls,age, name):
        if not hasattr(Person, "new_obj"):
            Person.new_obj = object.__new__(cls)
        return Person.new_obj

    def __init__(self,age, name):
        if not hasattr(self, "age"):
            self.age = age
        if not hasattr(self, "name"):
            self.name = name
    
    def get_name(self):
        return self.name
```
运行结果：
```python 
In [46]: xiaoMing = Person(18, "xiaoMing")

In [47]: xiaoHua = Person(20, "xiaoHua")

In [48]: id(xiaoMing)
Out[48]: 4559714232

In [49]: id(xiaoHua)
Out[49]: 4559714232

In [50]: xiaoMing == xiaoHua
Out[50]: True

In [51]: xiaoMing is xiaoHua
Out[51]: True

In [52]: xiaoHua.name
Out[52]: 'xiaoMing'

In [53]: xiaoHua.age
Out[53]: 18
```
## 3、__del__方法的作用
当实例对象被销毁时会被调用
### 分析
```python 
In [68]: class A:
    ...:     def __del__(self):
    ...:         print("__del__被执行...")
    ...:

In [69]: a = A()

In [70]: del a
__del__被执行...
```
再看下面例子
```python
In [71]: def test():
    ...:     s = A()
    ...:     print("in test...")
    ...:

In [72]: test()
in test...
__del__被执行...
```
对比两个例子，前一个是用户手动删除实例对象，后一个是python gc回收实例对象，都会调用 __del__

### 应用
当一个对象被销毁时都需要执行某操作时，可以在 __del__ 中实现，而不用每次都要手动操作
如：
```python
# 以下是pymysql连接的代码，当对象销毁时确保socket被关闭
def _force_close(self):
    """Close connection without QUIT message"""
    if self._sock:
        try:
            self._sock.close()
        except:  # noqa
            pass
    self._sock = None
    self._rfile = None

__del__ = _force_close
```