## 1、__str__作用与用法
当实例对象被print调用时会打印__str__的返回值
- 如下：
```python
In [57]: class Person(object):
    ...:     def __init__(self, name):
    ...:         self.name = name
    ...:
    ...:     def __str__(self):
    ...:         return self.name
    ...:

In [58]: xiaoMing = Person("xiaoMing")
    ...: print(xiaoMing)
xiaoMing
```

## 2、__repr__作用与用法
与__str__作用差不多，用于交互模式下显示信息，区别在于不用print调用
```python
In [59]: class Person(object):
    ...:     def __init__(self, name):
    ...:         self.name = name
    ...:
    ...:     def __repr__(self):
    ...:         return self.name
    ...:
    ...: xiaoMing = Person("xiaoMing")
    ...:

In [60]: xiaoMing
Out[60]: xiaoMing
```