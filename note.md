# <center>python笔记 </center>

1. python中可变类型和不可变类型  
不可变类型：int,string,float,tuple  
可变类型：dict,list
2. python 中路径操作相关的函数  
import os  
3. python 新式类和经典类区别  
    + 新式类对象可以直接通过__class__属性获取自身类型:type
    + 继承搜索的顺序发生了改变，经典类多继承数学搜索顺序：
        + 深度优先-->先深入继承树左侧，再返回，开始找右侧；
        + 新式类多继承属性搜索顺序：广度优先遍历：先水平搜索，然后再向上移动
    + 新式类增加了__slots__内置属性，可以把实例属性的种类锁定到__slots__规定的范围之中
        + eg：
            ```python
            class A(object):
                __slots__ = ('name','age')
            class A1():
                __slots__ = ('name','age')
            a1 = A1()
            a = A()
            a1.name1 = 'a1'
            a.name1 = 'a'
            ```
            + A是新式类添加了__slots__属性，所以只允许添加name age
            + A1是经典类__slots__属性没用
        + 通常每一个实例都会有一个__dict__属性，用来记录实例中所有的属性和方法，也是通过这个字典，可以实例绑定任意的属性 
        + __slots__属性作用就是，当类C有比较少的变量，而且拥有__slots__属性时，类C的实例就没有__dict__属性，而是把变量的值存在一个固定的地方。如果试图访问一个__slots__中没有的属性，实例就会报错
        + 好处: __slots__属性令实例失去了绑定任意属性的遍历，但是因为每一个实例没有__dict__属性，却能有效 **_节省_** 每一个实例的 **_内存消耗_**，有利于生成小而精干的实例
    + 新式类增加了**__getattribute__**方法
        + ```python
            class A(object):    
                def __getattribute__(self, *args, **kwargs):    
                    print "A.__getattribute__"  
                    
                
            class A1():    
                def __getattribute__(self, *args, **kwargs):    
                    print "A1.__getattribute__"  
                    
                
            a1 = A1()  
            a = A()  
            
            a.test  
            print "========="  
            a1.test
            ```
            + ```python
                A.__getattribute__  
                =========  
                Traceback (most recent call last):  
                File "t.py", line 18, in <module>  
                    a1.test  
                AttributeError: A1 instance has no attribute 'test'
                ```
    + Python 2.x 中默认都是经典类，只有显式继承了object才是新式类
    + Python 3.x中默认都是新式类，不必显式的继承object