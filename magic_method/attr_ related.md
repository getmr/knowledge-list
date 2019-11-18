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
