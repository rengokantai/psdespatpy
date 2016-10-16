# psdespatpy
##7. The Builder Pattern
###1 Introduction to the Builder Pattern
Classification: creational  
seperate construction of an object from its representation.
Encapsulate object construction  
Multistep construction process  
Impl can very  
Client see only the abstraction

####2 The Computer Builder: Too Many Parameters!
computer.py
```
class Computer(object):
  def __init__(self,case):
    self.case=case
  def display(self):
    print('\t{:>10}:{}'.format('Case:',self.case))
```
__main__.py
```
from computer import Computer
computer = Computer(case='ke')
computer.display()
```
##8 The Factory Pattern
