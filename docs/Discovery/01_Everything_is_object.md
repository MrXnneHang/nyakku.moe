# Everything is object

## 1 初识

面向对象编程可以说是 Python 语法体系中最难的了，这个就算我有 c 的基础也是一点用都没，因为 c 还没有面向对象的编程思想，第一遍看书时候基本都没看懂，这次再看教程结合之前看的 turtle 库源码，对 class 的理解渐渐有了点眉目。

### 1.1 先做一个简单的 class

```Python
    '''首先，我们定义一个类，它就是一个东西的集合，比如人类、学生、动物……
括号里的是继承的父类，我们新建的这个类就是它的子集，继承了父类已有方法，但又有自己的独有方法'''
class Student(object):
    '''这个类是有一些属性的，比如学生有身高体重等等，不同的人身高体重是不同的，
我们用这些对它们进行区分，以便之后实例化他的时候调用。'''
    def __init__(self,name,height=175,weight=50):
        self.name=name
        self.height=height
        self.weight=weight
    '''作为一个类，不仅有属性，还有方法，其实就是他们所拥有的能力，比如学生会学习、玩、睡'''
    def study(self):
        print('{} is studying'.format(self.name))
    def play(self):
        print('{} is playing'.format(self.name))
    def sleep(self):
        print('zzZ')

'''我们建立几个学生的实例，比如学生小李，学生小王，可以在定义他们时候便输入他们的属性，
这样会通过__init__方法给他们的属性赋初值，当然身高体重已经有默认值了，不赋也不会出错'''
li=Student('xiaoli')
wang=Student('xiaowang',172,80)
'''现在，两个活生生的实例便诞生了，我们可以通过<实例>.<方法>(参数)调用方法，让他们使用自己作为学生所拥有的能力，
（因为现在他们只是学生类，只会学生所具备的能力），也可以通过<实例>.<属性>调出属性'''
li.study()
wang.sleep()
print('his name is {}'.format(li.name))
# 大部分注释都用两句话，第一句是刚接触类的感悟，第二句是之后深入理解类时候的想法
```

### 1.2 class and data

好了，现在已经可以写一个简单的 class 了，但这只是最最基本的 class，我们可以通过这个例子看出，这和我们直接使用函数没什么不同嘛，只是每次调用都针对着不同对象，不过仔细想想，不同的对象……这不就是我们操作的数据吗，每一个对象都是一个数据，那么反过来，哪个数据又不是对象呢？我们的 float、int、str、list、dict 不都是操作的对象吗，特别是 list，很早就见过 list.<方法>的形式对它进行操作了，这些不都是一样的嘛？冥冥中他们似乎有着某种联系，难道……有预先设定好叫做 list、int、str 等等的 class？我们使用函数都是调用他们所在 class 下的方法？后面看到 len(str)其实是在调用 str.**len**()方法，我就更加确信这一点了，再往后看，我渐渐明白了 class 的原理，或者说……Python 对数据的操作的原理。

### 1.3 Set? Extend?

首先，我们大概用数学的集合概念表征一下类的关系，我们最大的集合就是 object，它有着很多很多的子集，包括 int、float、dict、list、tuple、module 等等，这些是没有显式定义的 class，事实上一定有那么一个地方是定义着这些 class 的，除此之外，我们可以显式地定义一些 class，就像我之前写的 Student，很明显这也是一个 object 的子集，同时，我们也可以用继承关系创建无数的 class，我们可以很容易的从集合的角度理解他们之间的关系（子类继承父类，这就像子集拥有父集的一般属性一样），最后就是实例化了，我们可以将它理解为元素，因为它不能再包容其他元素了，所以不能再叫他集合了（不再是 class 了，他只能属于某个 class）。这时候我们把上文中的集合全换成 class，子集换子类，父集换父类，元素换实例，仔细想一下，是不是就可以理解了呢？

当然我们的子类不能一味地继承父类，子类自然也有自己的“个性”，也就是说子类可以定义自己的方法属性，实例也是一样的。在集合里很容易理解元素与子集之间的关系，实例与子类也是一样的，不严谨的说，实例就是一个最小的子类。正是因为他已经很具体到实例了，所以我们才会对他进行具体的操作，就像我们只会操作 0、1、2 这样具体的数据，不会操作自然数这样一个泛泛的集合一样。

我们理解 class 层次关系时完全可以这么想，实例继承了上层父类的所有可继承的能力。这里提到了可继承，是出于之前对**xxx 与\_xxx 的一点疑问，**xxx 是只有所在类可以调用，连子类（包括实例）都不能用，而\_xxx 是所在类及其子类（包括实例）均可调用。之前提到 module 也是一个 class，有着未显式定义的方法，但是我们为什么可以直接操作 turtle 这样的库名呢？因为 turtle 就是一个 module 类的实例啊！我们保存在 module 文件里的语句函数都是绑定在该实例上的方法属性！之前有看到实例不可以调用所在类的\_\_xxx 方法，这个很容易理解，后来看到了一个库拒绝调用的方法属性是用\_xxx 就可以，现在再想想，当然会拒绝了，我们写的文件里怎么可能有 module 实例下的子类呢？实例下已经不可能有子类了呀。

![01_Everything_is_object.png](../Images/01_Everything_is_object.png)

_（这里的最底层都是实例，虽然是使用集合形式去理解继承的关系，但由于只在多重继承才会出现与层次图的矛盾，所以只用层次图搭建了一个主框架）_

### 1.4 Call Method

继承关系是自上而下逐层寻找的，而调用方法属性的过程恰恰相反，Python 根据实例调用方法或属性的格式，首先在实例绑定的方法属性中找，找不到会接着找上层的类是否有对应属性方法，以此类推，一直找到 object（所以大概这里有对最基本的 class 的定义）。这种按照调用格式寻找是指（先不讨论后期绑定的属性和方法），print(a.age)这样就很自然地就去找 a 所在 class 的**init**初始化属性，a.eat()就去找 eat()方法，特殊的格式会去找 Magic 方法，比如 len(a)会去找**len**方法，for i in a 会去找**iter**()和对应的**next**()方法，等等。Python 所操作的一切数据都是被实例化的 class，因此我们完全可以用 class 写出对基本数据类型的操作方法。

### 1.5 Module? File?

最后，我想了下我们平时写的代码又是在什么环境呢？很明显，我们写的文件就是一个个的 module 实例，我们随时可以在其他文件中用 import 进行调用。emmmmm 居然连码的代码都是对象诶……Python 还真是……一切皆对象。

# Reference

1. [中国大学 MOOC Python 语言程序设计](https://www.icourse163.org/course/BIT-268001#/info)
2. [廖雪峰 Python 教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)