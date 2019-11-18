## 1、__enter__的作用
- 1. 定义当实例对象被with调用时，`___enter__`会被调用
- 2.  `__enter__` 的返回值被 with 语句的目标或者 as 后的名字绑定

## 2、__exit__的作用
- 1. 当上下文管理器执行完毕或者因异常终止应该做什么操作

**分析**
```python
In [80]: class Person(object):
    ...:     def __init__(self, name):
    ...:         print("init...")
    ...:         self.name = name
    ...:
    ...:     def __enter__(self):
    ...:         return self
    ...:
    ...:     def __exit__(self, exc_type, exc_value, traceback):
    ...:         print("exit...")
    ...:
    ...: xiaoMing = Person("xiaoMing") # 实例化完全可以在with语句中做
    ...: print("inited...")
    ...:
    ...: with xiaoMing as a:
    ...:     print(a.name)
    ...:
    ...: print("finished")
init...
inited...
xiaoMing
exit...
finished
```
可以看出 `__enter__` 实在with语句调用实例对象时被调用；
`__exit__` 是在with语句结束时被调用（实例对象并未被gc回收），这与 `__del__`是有区别的

## 应用
`__enter__` 和 `__exit__`用于下文管理器
例如：
```python
# pymyql 的连接的上下文管理器
def __enter__(self):
    """Context manager that returns a Cursor"""
    warnings.warn(
        "Context manager API of Connection object is deprecated; Use conn.begin()",
        DeprecationWarning)
    return self.cursor()

def __exit__(self, exc, value, traceback):
    """On successful exit, commit. On exception, rollback"""
    if exc:
        self.rollback()
    else:
        self.commit()
```