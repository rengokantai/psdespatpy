# psdespatpy
##6. The Singleton Pattern
###1 Introduction to Singleton
Classification: creational 
###2 Demo 1: Logger Example and Introduction
singleton_classic.py
```
class SingleTon(object):
  ans = None
  @staticmethod
  def instance():
    if '_instance' not in Singleton.__dict__:
      Singleton._instance = Singleton()
    return Singleton._instance
s1 = Singleton.instance()
s2 = Singleton.instance()
assert s1 is s2
s1.ans=1
print('same object')
```

##7. The Builder Pattern
###1 Introduction to the Builder Pattern
####Classification: creational  
Ensure a class has one instance  
Control access to limited resource  

seperate construction of an object from its representation.
Encapsulate object construction  
Multistep construction process  
Impl can very  
Client see only the abstraction

###2 The Computer Builder: Too Many Parameters!
computer.py
```
class Computer(object):
  def __init__(self,case):
    self.case=case
  def display(self):
    print('\t{:>10}:{}'.format('Case:',self.case))
```
\_\_main\_\_.py
```
from computer import Computer
computer = Computer(case='ke')
computer.display()
```

###3 The Computer Builder: Exposing the Attributes

computer.py
```
class Computer(object):
  def display(self):
    print('\t{:>10}:{}'.format('Case:',self.case))
```
\_\_main\_\_.py
```
from computer import Computer
computer = Computer()
computer.case='ke'
computer.display()
```
same result as previous code.
###4 The Computer Builder: Encapsulation
computer.py
```
class Computer(object):
  def display(self):
    print('\t{:>10}:{}'.format('Case:',self.case))
```
mycomputer.py
```
from computer import Computer

class MyComputer(object):

    def get_computer(self):
        return self._computer

    def build_computer(self):
        computer = self._computer = Computer()
        computer.case = 'ke'
```
\_\_main\_\_.py
```
from computer import Computer
from mycomputer import MyComputer

builder = MyComputer()
builder.build_computer()
computer = builder.get_computer()
computer.display()
```

###5 The Computer Builder: Ordering
change mycomputer.py
```
from computer import Computer

class MyComputer(object):

    def get_computer(self):
        return self._computer

    def build_computer(self):
        self._computer = Computer()
        self.get_case()
    def get_case(self):
        self._computer.case = 'ke'
```
###6 Applying the Builder Pattern
##8 The Factory Pattern
