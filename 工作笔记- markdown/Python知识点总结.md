[toc]

### 一、知识点

##### 1.for循环时候添加index

```python
# 使用enumerate()实现
ints = [8, 23, 45, 12, 78]
for idx, val in enumerate(ints):
    print(idx, val)

ints = [8, 23, 45, 12, 78]
for index, item in enumerate(ints, start=0):   # 默认是从 0 开始
    print(index, item)

ints = [8, 23, 45, 12, 78]
for index, item in enumerate(ints, start=1):   # 索引修成从 1 开始
    print(index, item)
```



```python
# 使用range()实现
ints = [8, 23, 45, 12, 78]
for i in range(len(ints)):
   print(i, ints[i])
```



```python
# 仅使用for和in实现
ints = [8, 23, 45, 12, 78]
index = 0
for value in ints:
    index +=1
    print(index, value)


ints = [8, 23, 45, 12, 78]
index = 0
for value in (ints):
    index +=1
    print index, value
    if index >= len(ints)-1: # 重置index索引
        index = 0  

```



1. python中typing.Union 用法

   - 通常一个参数和返回值只能是一个类型，在c/c++,java,[golang](https://so.csdn.net/so/search?q=golang&spm=1001.2101.3001.7020)这种静态语言里，也不可能返回两种类型，或者传参使用两种类型，但是在python里可以。

     ```python
     def mytest(a:str or int)->str or int:
     　　return a*2
     
     
     from typing import Union
     def mytest(a:Union[str,int])->Union[str,int]:
     　　return a*2
     
     ```

2. python partial函数

   Python 提供了一个 functools 的模块，该模块为高阶函数提供支持，partial 就是其中的一个函数，该函数的形式如下：

   ```python
   functools.partial(func[,*args][, **kwargs])
   ```

   假设有如下函数：

   ```python
   def multiply(x, y):
       return x * y
   ```

   现在，我们想返回某个数的双倍，即：

   ```python
   >>> multiply(3, y=2)
   6
   >>> multiply(4, y=2)
   8
   >>> multiply(5, y=2)
   10
   ```

   上面的调用有点繁琐，每次都要传入 `y=2`，我们想到可以定义一个新的函数，把 `y=2` 作为默认值，即：

   ```python
   def double(x, y=2):
       return multiply(x, y)
   ```

   现在，我们可以这样调用了：

   ```python
   >>> double(3)
   6
   >>> double(4)
   8
   >>> double(5)
   10
   ```

   事实上，我们可以不用自己定义 double，利用 partial，我们可以这样：

   ```python
   double = partial(multiply, 2)
   
   ```

   ```python
   from functools import partial
    
   def subtraction(x, y):
       return x - y
    
   f = partial(subtraction, 4)  # 4 赋给了 x
   >>> f(10)   # 4 - 10
   -6
   ```

#### 2.将代码写到init文件中

peach/timer/store/__init__.py

```python
_engine = None


def get_engine():
    return _engine


def set_engine(engine):
    global _engine
    _engine = engine

```



### 二、Django

#### 1.ORM

##### 1.get_or_create、update_or_create

```markdown
Task.objects.update_or_create(
            biz_code=biz_code,
            biz_num=biz_num,
            defaults=dict(when=when, biz_ext=biz_ext),
        )

大致意思是创建或者更新，当过滤条件匹配到查询结果则将更新defaults字典中的字段，
` 否则创建一条记录，创建的字段内容为defaults + **kwargs【也就是传入的键值对】
```

##### 2.Potman如何设置header，在django中获取

```markdown
如果header 的key为auth-token，即headers={‘auth-token’:‘1234’}
应该使用request.META.get(“HTTP_AUTH_TOKEN”)获取

总结： header key中的小写转为大写，横线“-”转为下划线“”,并且加上前缀HTTP
尤其注意header key中不应该包含 HTTP前缀，以及符号"",否则会取不到对应的值
```



