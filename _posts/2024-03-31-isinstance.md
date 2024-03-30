---
title: isinstancce # 文章名称
tags: [Python,Code]
categories: [Code,Python]
---

`isinstance` 是 Python 编程语言中的一个内置函数，它用于检查一个对象是否是指定类或者是该类所定义的类的实例。

具体来说，`isinstance()` 函数接受两个参数：第一个参数是需要检查的对象，第二个参数是一个类或者包含多个类的元组。如果第一个参数是第二个参数指定的类或者是从属于这个类的子类，那么 `isinstance()` 函数将返回 `True`，否则返回 `False`。

这个函数在面向对象编程中非常有用，尤其是在需要根据对象的类型来执行不同操作或者在处理继承和多态性时。它可以帮助开发者确保对象具有期望的类型，从而避免类型错误和潜在的运行时异常。

下面是一个简单的使用 `isinstance` 函数的例子：

```python
class Animal:
    def __init__(self, name):
        self.name = name

class Dog(Animal):
    def __init__(self, name):
        super().__init__(name)

# 创建一个 Dog 类的实例
my_dog = Dog("Buddy")

# 使用 isinstance 检查 my_dog 是否是 Dog 类的实例
is_dog = isinstance(my_dog, Dog)
print(is_dog)  # 输出: True

# 检查 my_dog 是否是 Animal 类的实例
is_animal = isinstance(my_dog, Animal)
print(is_animal)  # 输出: True

# 检查 my_dog 是否是 str 类型的实例
is_str = isinstance(my_dog, str)
print(is_str)  # 输出: False
```

在这个例子中，`my_dog` 是 `Dog` 类的一个实例，而 `Dog` 继承自 `Animal` 类。因此，`isinstance(my_dog, Dog)` 和 `isinstance(my_dog, Animal)` 都返回 `True`，而 `isinstance(my_dog, str)` 返回 `False`，因为 `my_dog` 不是字符串类型的实例。