## 1、__new__方法的作用

用于类实例化时创建一个对象实例，但是这个实例没有任何属性，并返回这个实例， __init__会对这个实例进行赋予相应的实例属性
## 2、__init__方法的作用

将__new__方法的返回实例对象（没有属性的实例对象）赋予相应的属性

如何说明这一点呢？
## 3、分析
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

## 4、用途与实例
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
