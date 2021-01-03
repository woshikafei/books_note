实际逻辑：

1. 先以`classname`、`bases`、`dicts`为参数，去call这个指定的metaclass
2. 返回的实际class为metaclass call之后的返回结果。


```python
class MyType(type):
    def __init__(self,*args,**kwargs):
 
        print("Mytype __init__",*args,**kwargs)
 
    def __call__(self, *args, **kwargs):
        print("Mytype __call__", *args, **kwargs)
        obj = self.__new__(self)
        print("obj ",obj,*args, **kwargs)
        print(self)
        self.__init__(obj,*args, **kwargs)
        return obj
 
    def __new__(cls, *args, **kwargs):
        print("Mytype __new__",*args,**kwargs)
        return type.__new__(cls, *args, **kwargs)
 
class Foo(object,metaclass=MyType):  #python3统一用这种
    #__metaclass__ = MyType  #python2.7中的写法
 
    def __init__(self,name):
        self.name = name
 
        print("Foo __init__")
 
    def __new__(cls, *args, **kwargs):
        print("Foo __new__",cls, *args, **kwargs)
        return object.__new__(cls)
 
f = Foo("shuaigaogao")
print("f",f)
print("fname",f.name)
 
#输出
Mytype __new__ Foo (<class 'object'>,) {'__new__': <function Foo.__new__ at 0x0000025EF0EFD6A8>,
'__init__': <function Foo.__init__ at 0x0000025EF0EFD620>, '__qualname__': 'Foo', '__module__': '__main__'}
Mytype __init__ Foo (<class 'object'>,) {'__new__': <function Foo.__new__ at 0x0000025EF0EFD6A8>,
 '__init__': <function Foo.__init__ at 0x0000025EF0EFD620>, '__qualname__': 'Foo', '__module__': '__main__'}
Mytype __call__ shuaigaogao
Foo __new__ <class '__main__.Foo'>
obj  <__main__.Foo object at 0x0000025EF0F05048> shuaigaogao
<class '__main__.Foo'>
Foo __init__
f <__main__.Foo object at 0x0000025EF0F05048>
fname shuaigaogao
```

metaclass实现单例：

```python
def signten(classname,bases,dicts):
    # 设置标志，判断是否初始化
    _instance = None
 
    # 返回的是类对象
    s = None
    def get_instance(name,age):
        nonlocal s
        if s == None:
            # 实例化，元类的使用
            # classmate，bases,dicts为元类type的三个参数
            _instance = type(classname,bases,dicts)
            # s为类_instance的对象
            s = _instance(name,age)
        return s
    # 闭包必须返回参数，参数为方法名，不可返回方法名（）
    # 引用函数，并非调用函数,返回的是函数对象
    return get_instance
 
# 此时的User为方法，并非类，User()这时是在调用函数，并非类
class User(metaclass=signten):
    def __init__(self,name,age):
        self.name = name
        self.age = age
        
    def __str__(self):
        return f"name:{self.name},age:{self.age}"
 
# print(User)
# 上面语句的结果如下，返回的是一个函数，而类
# <function signten.<locals>.get_instance at 0x0000027FB0920268>
#  通过函数Uesr()，并返回get_instance()函数的返回值
u = User("王",23)
u1 = User("郭",15)
print(u is u1)
```





