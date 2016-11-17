---
layout: post
title: ［译］给Javascript开发者的Python指南
excerpt: <p>javascript,python</p>
---

| 原文：[Python for JavaScript Developers](https://mariopabon.com/2016/11/01/python-for-javascript-developers.html)


特别感谢[Brett Langdon](https://brett.is/)的认真检查

最近我开始在纽约一家创业公司工作，公司叫[Underdog.io](https://underdog.io/)，在那我发现他们后端主要是用Python写的，而这是我之前很少接触到的语言。

我是被雇来写JavaScript喝React的，这些在我的团队中只占了很少一部分，这意味着我经常得为了找到一些特性而钻进各个代码库中。所以我必须很快地了解Python。

不幸的是，我之前想找一些给写过代码的人看的Python教程找的好艰难。我已经知道怎么写代码了而且也熟悉其他语言，我只是需要学习Python这一特定语言的语法和规范。

这是我写这篇blog的原因。给Javascript开发者一个指南，可以快速地上手Python，而不用去学习声明一个变量的意思和方法是什么。

这篇log假设你在用Python 3.0.1，所以其中一些例子可能不能在旧版本的Python上跑。

<h2>内容列表</h2>

- [语法](#syntax)
- [类型](#type)
- [函数](#function)
- [模块](#module)
- [面向对象编程](#object-oriented)
- [资源](#resource)

---

<h2>语法</h2>

<h3>声明变量</h3>

在Python中声明一个变量非常简单。跟Javascript一样，你声明的时候也不用设置变量类型。还有你不用设置变量的作用域：

```
x = 5
```

你可以改变变量的类型，只要给它赋值一个不同类型的值

```
x = 5       # x has a type of Integer
x = ‘Hewwo’ # x is now a String
```

不像JavaScript，Python的变量一般都是块级作用域。

<h3>块级作用域</h3>

Python在不同于JavaScript，在语法上有点更严格。如果缩进少了一个空格都可以让程序跑不起来。这是因为Python是用缩进来创建块级作用域的。例如下面是JavaScript和Python创建作用域的比较：

<h3>Creating a block in JavaScript </h3>

```

function exampleFunction () {
  // This is a block
  var a = 5;
}

{
  // This is also a block
}

```

<h3>Creating a block in Python </h3>

```

# This is a block with its own scope

def example_function():
  # This is also a block with its own scope
  x = 5
  print(x)

```

 如果包含了`print(x)`的一行代码有一个或者更多额外的空格，Python解释器就会报`IndentationError`错误。因为这些额外的空格会创建一个非法的作用域。

```

def example_function():
  x = 5

  # IndentationError!
    print(x)

```

如果这一行代码缺少一个或者更多空格，像这样：

```

def example_function():
  x = 5
 print(x)

```

Python解释器会报错：

```

NameError: name 'x' is not defined

```

因为`print(x)`中的x是在声明x的作用域之外的作用域。

<h3>流程控制</h3>

Python的if...else，while和for和JavaScript非常相似：

<h4>if…else</h4>

```

if x > 2:
  print('hai!')
elif x > 3:
  print('bye!')
else:
  print('hey now')

if not x:
  print('x is falsy!')

```

<h4>while循环</h4>

```

while x > 0:
  print('hey now')

```

<h4>for循环</h4>
和JavaScript的`foreach`类似：

```

ex_list = [1, 2, 3]

for x in ex_list:
  print(x)

```

<h2 id="type">类型</h2>

Python的类型和JavaScript的类型很相似，所以就不像其他例如Java喝C#之类的语言严格

实际上变量有类型，但是你不用像java之类的静态类型语言去声明变量类型。

下面是一个Python数据类型的概览：

<h3>Numbers</h3>

和JavaScript不同，Python不止有一种number类型：

- Integers:1,2,3
- Floats:4.20,4e520
- Complex numbers:4 ＋ 20j
- Booleans:True,False

用Python你可以像JavaScript一样进行numbers的运算。还有一个求幂运算符（**）

```

# a = 4
a = 2 ** 2

```

<h3>Lists</h3>

Lists和JavaScript中的arrays类似，Lists可以包含不同的类型：

```

[4, "2", [0, "zero"]]

```

还有特殊的语法可以取子元素

```

a_list = [1, 2, 3, 4, 5]

# 1, 2, 3
a_list[0:2]

# 4, 5
a_list[3:]

# 3, 4
a_list[2, -2]

```

和一些内置方法：

```

# 3
len([1, 2, 3])

# 3, 2, 1
[1, 2, 3].reverse()

# 1, 2, 3
[1, 2].append(3)

```

你深知可以用＋号来连接两个lists：

```

# 1, 2, 3, 4
[1, 2] + [3, 4]

```

<h3>String</h3>

Python的Strings和JavaScript的很像，他们也是不可变的，而且独立的自负可以像数组子元素那样被获取到：

```

name = 'Mario'

# M
print(name[0])

# Nope, name is still 'Mario'
name[0] = 'W'

```

<h3>Dictionaries</h3>

Dictionaries是组合的arrays，类似JavaScript的objects。实际上，dictionaries可以用类似json语法来声明：

```

# Dictionaries in python
person = {
  'name': 'Mario',
  'age': 24
}

# Mario
print(person['name'])

```

Dictionaries有一个便利的方法，当获取一个不存在的key的value时，可以返回一个默认值：

```

# Because `gender` is not defined, non-binary will be returned
person.get('gender', 'non-binary')

```

<h3>None</h3>

None等于JavaScript的null。它表示一个值的缺失，并且可以表示false

```

x = None

if not x:
  print('x is falsy!')

```

<h2 id="function">函数</h2>

跟JavaScript一样，Python的函数是对象。这意味着你可以将函数当作参数，或者给函数设置属性：

```

def func(a, fn):
  print(a)
  fn()

func.x = 'meep'

# 'meep'
print(func.x)

def another_func():
  print('hey')

# 5
# 'hey'
func(5, another_func)

```

<h2 id="module">模块</h2>

Python的模块跟ES6的模块不会相差很多

<h3>定义一个模块</h3>

Python的模块就是一个包含Python代码的文件

```

# my_module.py
hey = 'heyyy'

def say_hey():
  print(hey)

```

不像JavaScript，你不用声明要暴露什么，默认所有东西都会被暴露出去

<h3>引入一个模块</h3>

你可以引入Python的整一个模块：

```

# importing my_module.py from another_module.py; both files are in the same
# directory
import my_module

# Do things
my_module.say_hey()
print(my_module.hey)

```

或者从一个模块引入单独项：

```

# another_module.py
from my_module import hey, say_hey

# Do things
say_hey()
print(hey)

```

你也可以从[pip](https://pypi.python.org/pypi/pip)安装别人写的模块，pip是一个Python的包管理器

```

pip install simplejson

```

<h2 id="object-oriented">面向对象编程</h2>

Python支持面向对象编程，而且不像JavaScript的原型继承，它支持class和传统的继承。

<h3>类</h3>

```

# Defining a class
class Animal:
  # Variable that is shared by all instances of the Animal class
  default_age = 1

  # Constructor
  def __init__(self, name):
    # Defining a publicly available variable
    self.name = name

    # You can define private variables and methods by prepending the variable
    # name with 2 underscores (__):
    self.__age = default_age

  # Public method
  def get_age(self):
    return self.__age

  # Private method
  def __meow():
    print('meowwww')

  # Defining a static method with the `staticmethod` decorator
  @staticmethod
  def moo():
    print('moooo')

# Creating an Animal object
animal = Animal()

# Accessing public variables and methods
print(animal.name)
print(animal.default_age)
print(animal.get_age())

# Accessing a static method
Animal.moo()

# ERR!!!! .__age is private, so this won't work:
print(animal.__age)

```

<h3>继承</h3>

class可以继承其他class：

```

# Inheriting from the Animal class
class Human(Animal):
  def __init__(self, name, ssn):
    # Must call the __init__ method of the base class
    super().__init__(name)
    self.__ssn = ssn

  def get_ssn(self):
    return self.__ssn

# Using the Human class
human = Human('Mario', 123456789)

# Human objects have access to methods defined in the Animal base class
human.get_age()
human.get_ssn()

```

<h2 id="resource">资源</h2>

除了这篇指南之外，Python还有许多内容，我强烈建议你去看一下[Python docs](https://docs.python.org/3/)，里面有教程和一些详细的语言特性。

还有记得学习一门语言最好的方法就是去写，要写很多。所以开始编码吧！

P.S:如果你对新建一个项目需要什么建议，或许可以试一下用[Flask](http://flask.pocoo.org/)创建一个简单的API？
