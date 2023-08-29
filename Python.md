
## Basic Details
### Python Virtual Machine
- A python program is executed over a **Python virtual machine**.  
- When we type `python3 file.py` in the terminal what actually happens is  that a software/program named `python` starts running which takes a file  name - `file.py` as its input.
- This software/program is actually the `python virtual machine`, which is usually implemented (programmed) in some native language such as C/C++.

- The Program(python virtual machine) reads the content of the file and compiles them to python **byte-code**.
```
                     compilation
  `file.py` ----------------------------> `python bytecodes`
```
  
  - After converting the python program to byte-codes, the python virtual machine starts **interpreting** the byte-codes **line by line**. 
  - To understand what `interpretation` means, focus on the term *virtual machine*. The python virtual machine in a sense simulates the actual hardware machine.
  - The **byte-code** is the machine language for this virtual machine.
    
  - Generally, when we compile a program written in C/C++, it is compiled to the machine language of the actual hardware. When we run such a program we call it `execution of the program`. 
  - But when the machine on which instruction will run is simulated, such as in case of python and java, we call running of the program as `interpretation of the program`. 
  - This is because the simulator (virtual machine) itself uses the resources(CPU, RAM etc) of the actual hardware, on top of which it is running, to actully perform any operation. It just `interprets` what the program is designed to do and uses the underlying hardware resources to perform those actions. 
  - This is different from execution because the machine instruction of underlying hardware is not directly used to perform the actions that the program is expected to do. Instead, the virtual machine interprets from the program what to do and does it in **its own way**.
    
  - To understand what **its own way** means, a simple example of calculator will help -
	  - Suppose we want to add two integers.
	  - One way for us to do it is to write a c/c++ program and use 'int' data type to store two input integers and `a` and `b` and do `sum = a + b`.
	  - Other way is to use a calculator program. It will take 2 integers as input using a GUI.
	    But in the background it can do the sum in any way possible. For example it can take two double data types and calculate the sum and then type cast them to integer. Or it can use some other binary operations to calculate the sum. For larger integers that do not fit in 'int' or 'long' it can use some complex 'big integer libraries' to do so.
	    But to the user it will appear as if just two integers have been added.
	- Python virtual machine acts like this calculator here. It makes it easy to write those program which would otherwise take large number of lines in c/c++.

- The python virtual machine acts like an actual hardware machine and hence the concepts of `stack` and `heap` that we learnt in C/C++ is also applicable here.
- Python is pure object oriented language so even the basic types such as 'int' are represented as objects in python. 
- Remember that an **object** is just a memory location which can have some **methods** associated with it.
- Even classes in Python are objects (See [Class Objects](#class-objects)). It means that every class has a corresponding memory location which contains information related to that class. This is not possible in C++ because there, classes are a feature of the compiler which does not exists at runtime. Here in python classes are a feature of the python virtual machine which actually exists at runtime (PVM interprets the bye codes remember ?).
- NOTE: *class* and *type* are often used interchangeably in python.

### Objects
- An Object at its core is just a **memory location** which has some corresponding **operations** to operate on it.
- As mentioned earlier, everything (classes, functions, modules(discussed later) etc ) in python is represented as an object.
- The Python Virtual machine (PVM) makes it possible to represent everything as an object (we don't need to go into details of that).

#### Variables
- A variable in python is just a *label* for an object that can be reassigned to some other object at any time.
- All the variables names in Python act like references in C++, but with additional flexibility as listed below:
	- A variable name is not associated with any specific type, so, we don't have to declare the type for a variable in python
		- Example:
```
		  # a is just a reference to an object of type 'xyz'.
		  # The 'type/class' of an object will be discussed in next section.
		  # It is an instance of the class
		  a = xyz()
		  #         _____________
		  #        |             |
		  # a ---->| [some data] |
		  #        |             |
		  #         -------------
		  a = "hello D"
		  # Now a refers to an object of type 'str'.
		  #         ____________
		  #        |            |
		  # a ---->|  "hello D" |
		  #        |            |
		  #         ------------  
```
- It is inconvenient to say all the time that a variable is a reference to an object, therefore, for simplicity, from here on will will refer to the reference of an object (i.e. the variable) as the object itself. 
- For example, let's say we have `x = 10`. So instead of saying that x is a reference to an `int` object with value 10, we will just say that x is an `int` object with value 10.
- But it is important to keep in mind all the time that a variable is just a reference to an object.

##### Attributes
- An attribute is a variable that is part of an object.
- It is created and accessed using dot(`.`) operator.
- They are also known as **instance variables** or **object variables**.
```
# x is a reference to an object
x = xyz()
# The line below will create a new attribute x.abc which will refer to an 'int' object.
x.abc = 10
# we can access the attributes of an object using the same notation
print(x.abc)

# a diagramatic representation will help
#         _____________                    __________
#        |             |      ----------->|          |
# a ---->| [some data] |      |           |    10    |
#        |    abc -----|-------           |          |
#         -------------                    ----------
# For simplicity, instead of showing an arrow for the value of attributes, from now on
# we will use '=' to denote the object they are associated with
# See the convention in the figure below.
#         _____________                  
#        |             |      
# a ---->| [some data] |      
#        |  abc = 10   |          
#         -------------                    
```

#### Literals
- A literal in python can be thought of as a *label* that **cannot* be reassigned to any other object.
- For example, `10` is a label that will always refer to an integer object with value 10.
- Similarly, `"hello"` is a label that will always refer to a string object with value "hello".
- Whenver any literal is used in a python program, the PVM creates the object corresponding to it automatically. We don't have to explicitly create it.
- An example below will clarify the concept of constants:
```
                  a = 10
                  # The line above will be processed in three steps
                  # 1. The PVM will create an object with value 10
                  # 2. PVM will assign a label `10` to that object
		  # 3. PVM will copy the value of label `10` into label `a`.
                  #         ____________
		  #        |            |
		  # a ---->|     10     |<----- 10
		  #        |            |
		  #         ------------
                  # `a` can be reassigned that is why it is called 'variable'
		  a = 20
                  # However, `10` cannot be reassigned to refer to a different object
                  # Uncommenting the line below will generate an error.
                  # 10 = "xyz" 
```

#### Class Objects
- Every object in python is associated with a **class/type**. This **class/type** defines how the memory locations associated with the object will be used and what all operations can be performed on that memory location.
	- For example, let's suppose two memory location $A$ and $B$ are storing the same binary data - 0100000101000001. 
	- We can treat the data at memory location $A$ as an integer whose value is 16705.
	- Similarly we can treat the data at memory location $B$ as a string whose value is "AA".
	-  The operations permitted on the data at location $A$ will be the operations that are valid for an integer but maybe invalid for a string. 
	   Vice-versa the operations permitted on the data at location $B$ will be the operations that will be valid for a string may be invalid for an integer. 
	- For example, negation(~) is valid for an integer but not for a string. Similarly, concatenation is valid for string but not for an integer.
- To represent this information we need to associate some **class/type** information with every object.
- This **class/type** information in python is not actually stored inside the object but as a seperate object. We call such objects as **class objects**.
- Every object (including class-objects) instead has a pre-defined attribute named `__class__`. This attribute refers to the object which stores class/type information of this object.
- This approach of storing the class data as seperate objects has the following major advantages:
	- Data which is common to all the objects do not need to be duplicated for every object. For example, the methods associated with a particular type of 
	  object will be same for all the objects, so, only one copy of methods is required which can be stored in class-object for the that type.
   	- The class data can be manipulated at run-time. For example we can easily add new methods to a class at run-time.
- The example below shows a visual representation of a class-object and instances of the class/type represented by it.
```
# There are three integers here with values 10, 20 and 30. The object on the right most side is a class object.
# Class data here means methods and other information needed to represent a class/type.
#                                                                                     int 
#                                                                                      ^
#                                                                                      |
#  __________________    __________________     __________________             ________|_____________   
# | __class__ = int  |  | __class__ = int  |   | __class__ = int  |           |                      |
# |        10        |  |        20        |   |        30        |           | [class data for int] |
#  ------------------    ------------------     ------------------             ----------------------
#                                                                                  Class Object
```

- Now a question arises - Do the class-objects have a class/type associated with them ? 
- The answer is Yes! Every class-object also has an associated class/type.
	- It is represented by a **special class-object** which is created by PVM during startup.
 	- The PVM stores a reference to this special class-object in a variable named `type`.
  	- This class object is **special** in the sense that the class/object information for this object is represented by itself.
  	- The `__class__` attribute of all the class-objects is set to `type`.
  	- There is **only one class-object** corresponding to every class/type.
- An example below shows this relation.
```
# the line below assigns to variable x, a reference to an integer object containing value 10.
x = 10 # binary value - 1010 (not showing leading 0s here for simplicity)
#             __________________               ______________________                   _______________________
#            | __class__ = int  |             | __class__ = type     |                 |   __class__ = type    |
# x ----->   |       10         |  int -----> | [class data for int] |   type ----->   | [class data for type] |
#            |                  |             |                      |                 |                       |
#             ------------------               ----------------------                   -----------------------
#   | Object/Instance of class/type int  | Class Object for class/type int     |   Class Object for class/type type |
```
- In the example above, the class/type associated with the integer object `x` with value 10 is a represented by class-object `int`.
- Further, the class/type associated with the class-object `int`, is the special object `type` that PVM created while starting.

##### Important conventions to follow from here on:
- If object `A` stores the class/type data associated with an another object `B` then we say that **`B` is an instance of class/type `A`** or **`B` is an object of class/type `A`**. For example, every object whose `__class__` attribute is set to `int`  is an instance of the class `int`. Keep this notation in mind as it will be used throught this article.
- In python a *class* is also referred to as a *type* because it represents the data type of an object. We will use class and type interchangebly unless explicitly specified.
- Don't get confused between type and `type`. We use type to refer to the class/type of an object and `type` is the variable that refers to the special object created by the PVM.

#### Special Attributes
- Each object has some special attributes which have a naming convention that starts with double underscores and ends with double underscores - `__attribute_name__` .
- Some set of special attributes are common to all objects but some are available only in certain type of objects.
- These attributes can refer to an object storing some information about the current object. For, example, the `__name__` attribute, which is present only in class-objects, is a string object containing name of the class/type.
- Likewise, these attributes can also refer to an object containing set of instructions to be run.
- In this section we will describe some of the common special-attributes that are necessary for a python programmer to understand.
  	- `__init__` : It is a special attribute that refers to
  	- 
-  It is to be noted that the `__init__` method defined earlier is also a special method that works as the constructor for the object. Whenever a new object for a class is created, the first method of the object that is called by default is `__init__` . It is used like a constructor to initialise some instance variables or do some other initialisation task.
- There are several other special methods such as `__add__`, `__mul__`, `__str__` . These can be used to achieve operator overloading for the objects, like in C++.
- We will discuss a special method `__call__` here which is necessary to understand the process of explicit object creation.
	- This method corresponds to the operator `()` . 
	- Suppose that we have a variable reference `xyz` to an object of type `type` (which in the simplest way can be obtained by defining a class `xyz` as in the previous example ), then if we write `xyz()`, it will call the method `__call__` of the object referred to by `xyz`. In simple terms `xyz()` is equivalent to `xyz.__call__()`.
	- All the objects of the class/type `type` (i.e. all the **class objects**) have a default implementation of this method, which creates a new object of the class/type represented by this **class object** and returns a reference to it.
	- All the objects of the class/type `function` (i.e. all the **function objects**) have a default implementation of this method which runs the associated function body.
	- Any object which is not an instance of class `type` (i.e. an object which is not **class object**) or an instance of class `function` (i.e. an object which is not a function) do not have a default implementation of this method.
	- So if we write `x = xyz()` and then do `x()` , we will get an error, because `x` is a reference to object of type `xyz` and not type `type` or `function`. 
	- But, if we explicitly define a method `__call__` inside the class `xyz` then the above statement will call this method. It is not generally used in competitive coding and mostly is required for functional programming.

#### Defining a custom *class-object* (or simply class)
- The PVM reads the byte-code of the current file/script line by line.
- Whenever the PVM encounters a class definition it *interprets* all the code that is defined under line by line.
- It simultaneously creates a new object of type `type` (i.e. creates a new class-object) and stores the data related to the class definition inside this class-object.
- An example below shows the syntax for defining a class and describes how the PVM creates a correspondng class-object.
```
print('After printing this line, PVM will read the next line which contains the starting of a class definition')

# syntax for creating class
class xyz:
	# The operations that are permitted on the instances/objects of this class are defined using 'methods'.

        # A 'method' is just a function that is associated with an object. (Functions themselves
	# are objects so methods are also treated as objects - function objects are discussed later)

	# A Parameter `self` is required as the first parameter for every method. It refers to the current object,
	# just like `this` in C++. In C++ `this` is passed implicitly but here it is explicitly passed.
	# Nevertheless, the purpose remains same. It is required otherwise the method will have no way to know which instance it is currently
	# operating on.

	# Remember that objects are just some memory locations and same class/type can have multiple instances.

	def method1(self):
		# some operations
		print("method1")
	
	# constructor (details ahead)
	def __init__(self):
		# create some instance-variables/attributes
		self.x = 10
		self.y = 20
		
	# another method
	def method2(self):
		print("method2")
		print(self.x)
		print(self.y)

# By the time the PVM reads the line below and interprets it, it would have finished creating a new object of 
# type 'type' (i.e., a new class-object) in the memory and assign a reference to that object to the variable 'xyz'. 
# The object will contain all the class data of the class/type xyz.
print('PVM would have now created an instance of type 'type' in the memory')
#             _____________________
#            |  __class__ = type   |
# xyz -----> | __init__            |
#            | method1             |
             | method2             |
#             ---------------------
```
- Before going into details of explicitly creating an object of a given type/class we need to know about some special methods inside a class that are defined below.


#### Explicitly creating an Object of a class/type
- Let `xyz` be an object of class `type`. 
- To create an instance of `xyz` we use `()` operator which creates a new object of type `xyz` and returns a reference to it. We can store this reference in a variable and use it to refer to that object.
	- Ex: `x = xyz()`

#### Function Objects
- Functions in python are also represented by objects. 
- Every function in python is an instance of a pre-defined class-object `function`
- The
- The PVM reads a file/script line by line and when it encounters a function defintion, it reads it and creates a corresponding object of type `function` and assigns its reference to a variable with same name as the function name. 
- Example below illustrates this:
```
def func():
	# function body
	print("hello D")
# `func` now referes to an object of class/type 'function'
#            ______________________
#           | __class__ = function |
# func ---->|  [function bytecode] |
#           |                      |
#            ----------------------
# func can be used just like any other variable. 
temp = func
# temp can now be used as a function.
temp()

func = 10
# func now refers to an integer object
```

#### Immutable and Mutable types
```
# x refers to an `int` object with value 10
x = 10
# x now refers to a new 'int' object with value 30.
# Also note that when the code below is running, an `int` object with value 20 will also be created first.
x = x + 20
```

### Conditional Statements
#### if/elif/else statement syntax
```
x = 10
if (x == 10):
	print("ten")
elif (x == 20):
	print("twenty")
else:
	print("other value")

# parenthesis are optional
if x == 10:
	print("ten")
```


### Iterative Statements 
#### for statement
- For loop in python works in a different way than in languages such as C/C++, JAVA etc.
- To properly understand the for loop it is necessary to understand the concept of `iterables`
