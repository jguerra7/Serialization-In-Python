# Python Pickle Instructor Examples
Python Pickle is used to serialize and deserialize a python object structure. Any object on python can be pickled so that it can be saved on disk.

At first Python pickle serialize the object and then converts the object into a character stream so that this character stream contains all the information necessary to reconstruct the object in another python script.

Note that the pickle module is not secure against erroneous or maliciously constructed data according to the documentation. So, never unpickle data received from an untrusted or unauthenticated source.

Python Pickle dump
In this section, we are going to learn, how to store data using Python pickle. To do so, we have to import the pickle module first.

Then use pickle.dump() function to store the object data to the file. pickle.dump() function takes 3 arguments. The first argument is the object that you want to store. The second argument is the file object you get by opening the desired file in write-binary (wb) mode. And the third argument is the key-value argument. This argument defines the protocol. There are two type of protocol – pickle.HIGHEST_PROTOCOL and pickle.DEFAULT_PROTOCOL. See the sample code to know how to dump data using pickle.

```python

import pickle

# take user input to take the amount of data
number_of_data = int(input('Enter the number of data : '))
data = []

# take input of the data
for i in range(number_of_data):
    raw = input('Enter data '+str(i)+' : ')
    data.append(raw)

# open a file, where you ant to store the data
file = open('important', 'wb')

# dump information to that file
pickle.dump(data, file)

# close the file
file.close()

```
The following program will prompt you to enter some input. In my case, it was like this.

![image](https://user-images.githubusercontent.com/47218880/61137160-a445f100-a48a-11e9-866f-003b11837fa1.png)

# Python Pickle load
To retrieve pickled data, the steps are quite simple. You have to use pickle.load() function to do that. The primary argument of pickle load function is the file object that you get by opening the file in read-binary (rb) mode.

Simple! Isn’t it. Let’s write the code to retrieve data we pickled using the pickle dump code. See the following code for understanding.

```python

import pickle

# open a file, where you stored the pickled data
file = open('important', 'rb')

# dump information to that file
data = pickle.load(file)

# close the file
file.close()

print('Showing the pickled data:')

cnt = 0
for item in data:
    print('The data ', cnt, ' is : ', item)
    cnt += 1

```
The outpus will be following:

```

Showing the pickled data:
The data  0  is :  123
The data  1  is :  abc
The data  2  is :  !@#$
```

# Python json dumps
In the example below we convert a Python object to a JSON object.

Start with a class (Student), create the object (pythonObj) and
finally convert it to JSON using the method json.dumps().

```python
import json

class Student:
    def __init__(self, id, name, password):
        self.id = id
        self.name = name
        self.password = password

pythonObj = Student(1,'ashley','123456')

# Convert Python object to JSON object
jsonObj = json.dumps(pythonObj.__dict__)
print(jsonObj)
```
This outputs:
abstract-base-classes.md

> {"password": "123456", "id": 1, "name": "ashley"}

# Python json loads
If you want to convert JSON data to a Python dictionary, just use loads:

```python
import json

j = json.loads('{"password": "123456", "id": 1, "name": "ashley"}')
print(j["name"])
```
To convert it to a Python object, define the class and pass the parameters as a map.

```python
import json

class Student:
    def __init__(self, id, name, password):
        self.id = id
        self.name = name
        self.password = password

j = json.loads('{"password": "123456", "id": 1, "name": "ashley"}')
pythonObj = Student(**j)

print(pythonObj.id)
print(pythonObj.name)
print(pythonObj.password)
```

