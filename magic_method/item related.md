## __getitem__（self,key）
定义获取容器中指定元素的行为，相当于 self[key]

## __setitem__(self, key, value)
定义设置容器中指定元素的行为，相当于 self[key] = value

## __delitem__(self, key)
定义删除容器中指定元素的行为，相当于 del self[key]

**例子**

```python
In [32]:class DictDemo:
    ...:    def __init__(self,key,value):
    ...:        self.dict = {}
    ...:        self.dict[key] = value
    ...:    def __getitem__(self,key):
    ...:        return self.dict[key]
    ...:    def __setitem__(self,key,value):
    ...:        self.dict[key] = value
    ...: dictDemo = DictDemo('key0','value0')
    ...:

In [33]: dictDemo['key0']
Out[33]: 'value0'

In [34]: dictDemo['key1'] = 'value1'

In [35]: dictDemo['key1']
Out[35]: 'value1'


# 一下是类一些其他的魔法方法
In [36]: dictDemo.__dict__
Out[36]: {'dict': {'key0': 'value0', 'key1': 'value1'}}

In [37]: dictDemo.__class__
Out[37]: __main__.DictDemo

In [38]: dictDemo.__module__
Out[38]: '__main__'

In [39]: dictDemo.__dir__
Out[39]: <function DictDemo.__dir__>

In [40]: dictDemo.__dir__()
Out[40]:
['dict',
 '__module__',
 '__init__',
 '__getitem__',
 '__setitem__',
 '__dict__',
 '__weakref__',
 '__doc__',
 '__repr__',
 '__hash__',
 '__str__',
 '__getattribute__',
 '__setattr__',
 '__delattr__',
 '__lt__',
 '__le__',
 '__eq__',
 '__ne__',
 '__gt__',
 '__ge__',
 '__new__',
 '__reduce_ex__',
 '__reduce__',
 '__subclasshook__',
 '__init_subclass__',
 '__format__',
 '__sizeof__',
 '__dir__',
 '__class__']

```