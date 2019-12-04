# 属性相关
## 1、 `__getattr__（self, name）` 的作用
获取对象不存在的属性时的行为，通俗的讲就是当使用点号获取实例属性时，如果属性不存在就自动调用`__getattr__`方法
## 2、__getattribute__(self, name)的作用
获取对象属性时会调用该方法获取其返回值，如果获取不到会调用`__getattr__`方法
## 3、`__setattr__`的作用
定义一个对象属相时被调用
## 4、`__delattr__ `的作用
删除一个对象的属性时被调用

**例如**
```python
class Person(object):
    def __init__(self, name, age):
        print('调用init...')
        self.name = name
        self.age = age
        print("in init set atrr finished")
    
    def __getattr__(self, name):
        # 当__getattribute__方法获取不到时，会调用__getattr__方法
        return None
    
    def get_attr(self, key):
        return self.__dict__[key]
    
    def __getattribute__(self, key):
        # 通过.获取属性或方法，会先走这个方法
        # 所以在这个方法里面不要用self.，否则会造成死循环
        print("调用__getattribute__....")
        return object.__getattribute__(self, key)
    
    def __setattr__(self, key, value):
        print("调用__setattr__")
        self.__dict__[key] = value
        print(f"set attr {key}: {value}")
```
## 5.对应的内置函数hasattr、getattr、setattr
- hasattr(ojt, "attr")

    用于判断某一对象是否拥有某属性或方法, 如果有就返回True, 否则返回False
```python
In [5]: class Person(object):
   ...:     def __init__(self, name, age):
   ...:         print('调用init...')
   ...:         self.name = name
   ...:         self.age = age
   ...:         print("in init set atrr finished")
   ...:
   ...:     def __getattr__(self, name):
   ...:         # 当__getattribute__方法获取不到时，会调用__getattr__方法
   ...:         return None
   ...:
   ...:     def get_attr(self, key):
   ...:         return self.__dict__[key]
   ...:
   ...:     def __getattribute__(self, key):
   ...:         # 通过.获取属性或方法，会先走这个方法
   ...:         # 所以在这个方法里面不要用self.，否则会造成死循环
   ...:         print("调用__getattribute__....")
   ...:         return object.__getattribute__(self, key)
   ...:
   ...:     def __setattr__(self, key, value):
   ...:         print("调用__setattr__")
   ...:         self.__dict__[key] = value
   ...:         print(f"set attr {key}: {value}")
In [6]: xiaoWang = Person("xiaoWang", 19)
In [12]: hasattr(xiaoWang, "name")
调用__getattribute__....
Out[12]: True
In [15]: hasattr(xiaoWang, "have") # 为什么会返回True
调用__getattribute__....
Out[15]: True

In [16]: hasattr(xiaoWang, "has") # 为什么会返回True
调用__getattribute__....
Out[16]: True

In [17]: hasattr(xiaoWang, "haha") # 为什么会返回True
调用__getattribute__....
Out[17]: True
```
**为什么上面没有的属性会返回True?**

因为上面的类实现了__getattr__魔法方法，获取不到的属性会调用该方法，该方法默认有返回值，所以无论怎么样都会有返回值，故hasattr总是会返回True

```python
In [19]: class Person(object):
    ...:     def __init__(self, name, age):
    ...:         print('调用init...')
    ...:         self.name = name
    ...:         self.age = age
    ...:         print("in init set atrr finished")
    ...:
    ...:     #def __getattr__(self, name):
    ...:         # 当__getattribute__方法获取不到时，会调用__getattr__方法
    ...:         #return None
    ...:
    ...:     def get_attr(self, key):
    ...:         return self.__dict__[key]
    ...:
    ...:     def __getattribute__(self, key):
    ...:         # 通过.获取属性或方法，会先走这个方法
    ...:         # 所以在这个方法里面不要用self.，否则会造成死循环
    ...:         print("调用__getattribute__....")
    ...:         return object.__getattribute__(self, key)
    ...:
    ...:     def __setattr__(self, key, value):
    ...:         print("调用__setattr__")
    ...:         self.__dict__[key] = value
    ...:         print(f"set attr {key}: {value}")
    ...:

In [20]: xiaoWang = Person("xiaoWang", 19)
In [21]: hasattr(xiaoWang, "get")
调用__getattribute__....
Out[21]: False
```
将__getattr__注释掉，没有的属性就会返回False

- getattr

    相当于obj.attr
```python
In [26]: get_attr = getattr(xiaoWang, "get_attr")
调用__getattribute__....

In [27]: get_attr("name")
调用__getattribute__....
Out[27]: 'xiaoWang'

In [28]: get_attr = getattr(xiaoWang, "get_attr")("name")
调用__getattribute__....
调用__getattribute__....

In [29]: get_attr
Out[29]: 'xiaoWang'
```

- setattr
    相当于__setattr__

```python
In [30]: setattr(xiaoWang, "weight", 130)
调用__setattr__
调用__getattribute__....
set attr weight: 130

In [31]: xiaoWang.weight
调用__getattribute__....
Out[31]: 130

```