# JSON EXAMPLE IN PYTHON


When you define a python dict variable like below, the variable’s value is saved in memory when the program runs:

> d = dict(name='Richard', age=25, score=100)

You can change the variable value at any time, such as changing the name to ‘Tom’, but once the program ends, the memory taken by the variable is completely reclaimed by the operating system.

If the modified variable is not stored on disk, the variable is initialized to ‘Richard’ the next time you run the program again.

The process of changing variables from memory to storable or transportable is called serialization. Once serialized, the serialized content can be written to disk or transferred over the network to another machine. In turn, rereading variable content from serialized objects into memory is called deserialization.

There are two built-in module that provide serialization and deserialization, one is JSON the other is Pickle.

# Passing Data
If we want to pass objects between different programming languages, we have to serialize objects to standard formats, such as XML.

But a better approach would be to serialize object as JSON, because JSON is represented as a string that can be read in all programming languages, and JSON string can be conveniently stored on disk or transmitted over the network.

JSON is not only standard, but also faster than XML, and can be read directly from a Web page, which is very convenient.

# Python Object To JSON String.
Python’s built-in json module provides Python object to a json format string conversion. Let’s first look at how to turn a Python object into a JSON object.

```python
import json
dict_object = dict(name='Tom', age=28, sex='male', score=100)
# The dumps() method returns a string, which is the standard JSON string.
# Similarly, the dumps() method can write JSON directly to a file-like Object
json.dumps(dict_object)
'{"name": "Tom", "age": 28, "sex": "male", "score": 100}'
```
# JSON String To Python Object.
To deserialize JSON string to a Python Object, use the loads() method, or use the corresponding load() method, which reads the string from the file-like object and deserialize it.

```python
import json
json_string = '{"name":"Tom", "age":28, "sex":"male", "score":100}'
json.loads(json_string)
{'name': 'Tom', 'age': 28, 'sex': 'male', 'score': 100}
```

# Serialize And Deserialize Custom Python Class.

## Python Class Instance To JSON.
Python dict objects can be serialized directly into JSON string, but in many cases, 
we prefer to represent objects with class, such as define an instance of the Employee class and then serialize it.

```python
import json

class Employee(object):
    def __init__(self, name, department):
        self.name = name
        self.department = department

employee = Employee('Tom', 'Dev')
print(json.dumps(employee))
```
But when you execute above code, you will get an error like below.

> TypeError: Object of type 'Employee' is not JSON serializable

The reason for the error is that the Employee object is not an object that can be serialized into JSON.

Take a look at the list of parameters for the dumps() method, and you can see that the dumps() method provides a whole bunch of optional parameters in addition to the first required obj parameter. These optional parameters can be used to customize the JSON serialization.

The reason why the previous code could not serialize the Employee class instance to JSON is that by default the dumps() method does not know how to turn the Employee instance into a JSON {} object.

The optional parameter default is a function name which can turn any python class object into an object that can be serialized as JSON. We just need to write a transformation function specifically for Employee and pass it in the dumps() method.

```python
import json

class Employee(object):
    def __init__(self, name, department):
        self.name = name
        self.department = department

    # This method can return a python dict object by the class instance.
    # And the dict object can be serialized to JSON.
    def employee2dict(self, employee):
        return {
            'name': employee.name,
            'department': employee.department
        }

employee = Employee('Tom', 'Dev')
print(json.dumps(employee, default=employee.employee2dict))
```
#  JSON String To Python Class Instance.
If we want to deserialize JSON string to an Employee object instance, the loads() method would first convert the string to a python dict object, and then the object_hook function we passed in takes care of converting the dict object to an Employee class instance.

```python
def dict_to_employee(self, dict):
    name = dict['name']
    department = dict['department']
    ret = Employee(name, department)
    return ret

employee_string = '{"name":"Tom", "department":"Dev"}'

employee_object = json.loads(employee_string, object_hook=dict_to_employee)
print(employee_object.name)
print(employee_object.department)

```

