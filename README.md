# psdespatpy
##5. The Command Pattern
###1 Overview and Motivating Example
Classification: creational  
create undoable operations and macros
encapsulate a request as an object
###2 Demo 1: Initial Attempt
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
logger_classic.py
```
import datetime

class Logger(object):
    log_file = None
    
    
    #same as above program, using _instance
    @staticmethod
    def instance():
        if  '_instance' not in Logger.__dict__:
             Logger._instance = Logger()
        return Logger._instance

    def open_log(self, path):
        self.log_file = open(path,mode='w')

    def write_log(self, log_record):
        now = str(datetime.datetime.now())
        record = '%s: %s' % (now, log_record)
        self.log_file.write(record)
    
    def close_log(self):
        self.log_file.close()

logger = Logger.instance()
logger.open_log('ke.log')
logger.write_log('Logging..')
logger.close_log()

with open('ke.log', 'r') as f:
    for line in f:
        print(line)

```
Disadvantages:
- Violates single responsibility principle
- Non-standard class access

###3 Demo 2: Building a Singleton Base Class
singleton_base.py, can create as many singletons as we need
```
class Singleton(object):
    _instances = {}     # dict([cls, instance])

    def __new__(cls, *args, **kwargs):
        if cls not in cls._instances:
            instance = super().__new__(cls)
            cls._instances[cls] = instance
        return cls._instances[cls]
```
logger_base.py
```
import datetime
from singleton_base import Singleton

class Logger(Singleton):
    log_file = None

    def __init__(self, path):
        if self.log_file is None:
            self.log_file = open(path,mode='w')

    def write_log(self, log_record):
        now = str(datetime.datetime.now())
        record = '%s: %s\n' % (now, log_record)
        self.log_file.write(record)

    def close_log(self):
        self.log_file.close() 
        self.log_file = None       

```
###4 Demo 3: Singleton as a Meta Class
Metaclass:
- class is an instance of metaclass  

singleton_meta.py
```
import datetime

class Singleton(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances: 
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]
```
logger_meta.py
```
import datetime
from singleton_meta import Singleton

class Logger(metaclass=Singleton):
    log_file = None

    def __init__(self, path):
        if self.log_file is None:
            self.log_file = open(path,mode='w')

    def write_log(self, log_record):
        now = str(datetime.datetime.now())
        record = '%s: %s\n' % (now, log_record)
        self.log_file.write(record)

    def close_log(self):
        self.log_file.close() 
        self.log_file = None       

```
###5 Demo 4: The MonoState Mini-pattern
monostate.py
```
class MonoState(object):
    _state = {}

    def __new__(cls, *args, **kwargs):
        self = super().__new__(cls)
        self.__dict__ = cls._state
        return self
```
logger_mono.py
```
import datetime
from monostate import MonoState

class Logger(MonoState):
    log_file = None

    def __init__(self, path):
        if self.log_file is None:
            self.log_file = open(path,mode='w')

    def write_log(self, log_record):
        now = str(datetime.datetime.now())
        record = '%s: %s\n' % (now, log_record)
        self.log_file.write(record)

    def close_log(self):
        self.log_file.close() 
        self.log_file = None       

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
