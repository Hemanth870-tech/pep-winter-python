# DAY 2 WORK
### COMMON OCCURANCE
1. Question : input is a string = aabbbccdddd
              output should be like this = b 3
                                           c 2
                                           a 4
2. Solution: I have done it in two ways :
 A) using count and for loop and if else statement but output was incorrect because counts consecutive identical characters, not total occurrences in the entire string.

s = input()
count = 1
for i in range(1, len(s)):
    if s[i] == s[i - 1]:
        count += 1
    else:
        print(s[i - 1], count)
        count = 1

print(s[-1], count)

output:
a 2
b 2
c 2
a 2
d 2

so to minimze this error i used counter method:
B) using counter and collections and for loop 
 using Counter counts total occurrences of each character in the entire string

from collections import Counter

s = input("Enter the string: ")
counts = Counter(s)
for ch, count in counts.items():
    print(ch, count)

output:
b 3
c 2
a 4
d 2

### DATA CLASS
1. This provides a concise way to define classes primarily intended to store data . It automatically generates several special methods, such as init, repr, and eq, based on the class attr you define.
2. init() : initializes the object and assigns the provided values to the attributes.
3. repr() : provides a string represnation.

eg:1.

from dataclasses import dataclass
@dataclass
class Person:
    name:str
    age:str       #to get the output we use person1.age


eg:2.

@dataclass
class Rectangle:
    width:int
    height:int
    color:str='white'
    
rectangle1=Rectangle(12,14)
rectangle2=Rectangle(12,13,'red')
rectangle2.color

output: 'red'

There is a arugment called frozen in data class means we can't update the dataclass values unless frozen==false.

@dataclass(frozen=True)  
class Point:
    x:int
    y:int

point=Point(3,4)
point.x,point.y
    
output: (3,4)

#### INHERTIANCE IN DATACLASS

@dataclass
class Person:
    name:str
    age:int
    
@dataclass
class Employee(Person):
    employee_id:str
    department:str


person=Person('krish',31)
employee=Employee("krish",31,'123','ai')
employee.name

output: 'krish'


### CLASS METHODS AND CLASS VARIABLES

class Car:
    base_price = 100000              #class variables
    def __init__(self,windows,doors,power):
        self.windows=windows
        self.doors=doors
        self.power=power
    def what_base_price(self):
        print("the base price is {}".format(self.base_price))
    @classmethod
    def revise_base_price(cls,inflation):
        cls.base_price=cls.base_price+cls.base_price*inflation


Car.revise_base_price(0.70)
#Car.base_price

car1=Car(4,5,2000)
car1.base_price


### PYTHON OOPS-PUBLIC,PRIVATE AND PROTECTED
 
eg:1.

class Car():               #public class
    def __init__(self,windows,doors,power):
        self.windows=windows
        self.doors=doors
        self.enginetype=power
        
audi = Car(4,5,"diesel")
audi.windows=5

eg:2.

class Car():               #protected class means all the class variables are protected   and in the output only the methods that are shown can be use to update
    def __init__(self,windows,doors,power):
        self.windows=windows
        self.doors=doors
        self.enginetype=power

class Truck(Car):
    def __init__(self,windows,doors,enginetype,horsepower):
        super().__init__(windows,doors,enginetype)
        self.horsepower=horsepower
        
truck=Truck(4,4,"diesel",4000)
dir(truck)

['__class__','__delattr__','__dict__','__dir__','__doc__','__eq__',
 '__format__',
 '__ge__',
 '__getattribute__',
 '__getstate__',
 '__gt__',
 '__hash__',
 '__init__',
 '__init_subclass__',
 '__le__',
 '__lt__',
 '__module__',
 '__ne__',
 '__new__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__setattr__',
 '__sizeof__',
 '__str__',
...
 '__weakref__',
 'doors',
 'enginetype',
 'horsepower',
 'windows']

eg:1. 

#private
class Car():              
    def __init__(self,windows,doors,power):
        self.__windows=windows
        self.__doors=doors
        self.__enginetype=power

audi = Car(4,4,"diesell")
audi._car__doors=5
dir(audi)

### Python Assert
1. used to check if a given logical expression is true or false. this will give you the output when it is true or else it will give yu error
eg:1.

num = 10
assert num>10

output:

AssertionError                            Traceback (most recent call last)
Cell In[16], line 2
      1 num = 10
----> 2 assert num>10

AssertionError: 

### Exception

class Absent(Exception):
    pass
    
class studentinfo:
    def __init__(self,name,rollno,year):
        self.name=name
        self.rollno=rollno
        self.year=year

class marks:
    def __init__(self,start,mid,end):
        self.start=start
        self.mid=mid
        self.end=end
class interview:
    def __init__(self,resume,pel,tech):
        self.resume=resume
        self.pel=pel
        self.tech=tech
    def result(self):
        total = self.resume+self.pel+self.tech
        
        if total > 70:
            return "pass"
        elif total < 70:
            return "fail"
        elif total == 0:
            raise Absent("student is absent")
        
try:
    # Creating objects
    s1 = studentinfo("Rahul", 101, 2025)
    m1 = marks(80, 75, 85)
    i1 = interview(0, 35, 0)   # change values to test

    # Display student info
    print("Name:", s1.name)
    print("Roll No:", s1.rollno)
    print("Year:", s1.year)

    # Interview result
    print("Interview Result:", i1.result())

except Absent as e:
    print("Interview Result:", e)

### INTERITANCE

#inheritance
class Car:
    def __init__(self, Windows, doors, enginetype):
        self.Windows=Windows
        self.doors=doors
        self.enginetype=enginetype
              
    def driving(self):
        print("car is used for driving")
        
##Audi car is inherting from Car class
class Audi(Car):
    def __init__(self,Windows,doors,enginetype,horsepower):
        super().__init__(Windows,doors,enginetype)
        self.horsepower=horsepower
        
    def selfdriving(self):
        print("it is a self driving car")
        
audiq7 = Audi(4,5,"diesel",200)

print(audiq7.horsepower)
print(audiq7.Windows)
audiq7.driving()
audiq7.selfdriving()

car1 = Car(4,5,"diesel")
print(car1)
print(audiq7)

print(dir(audiq7))
print(dir(car1))




