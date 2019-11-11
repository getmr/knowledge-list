## __call__的作用与用法
__call__是一个非常常用的魔法方法，如果该类实现了__call__方法，那么该类的实例对象就可以像函数一样被调用，常被用作协议的借口，如python的http框架wsgi、asgi协议

**例如**：
```python
class Person(object):
    def __init__(self, name):
        self.name = name
    
    def __call__(self, age: int):
        print(self.name, age)

```
输出：
```python
In [64]: xiaoMing = Person("xiaoMing")

In [65]: xiaoMing(18)
xiaoMing 18
```

以下是falsk的wsgi协议的接口：
```python
 def __call__(self, environ, start_response):
        """The WSGI server calls the Flask application object as the
        WSGI application. This calls :meth:`wsgi_app` which can be
        wrapped to applying middleware."""
        return self.wsgi_app(environ, start_response)
```
服务器uwsgi、或者gunicorn运行时会通过调用框架的app实例而调用__call__,从而实现服务器与框架的互通
