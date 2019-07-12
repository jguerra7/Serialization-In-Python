# JSON EXAMPLE IN PYTHON


When you define a python dict variable like below, the variable’s value is saved in memory when the program runs:

> d = dict(name='Richard', age=25, score=100)

You can change the variable value at any time, such as changing the name to ‘Tom’, but once the program ends, the memory taken by the variable is completely reclaimed by the operating system.

If the modified variable is not stored on disk, the variable is initialized to ‘Richard’ the next time you run the program again.

The process of changing variables from memory to storable or transportable is called serialization. Once serialized, the serialized content can be written to disk or transferred over the network to another machine. In turn, rereading variable content from serialized objects into memory is called deserialization.

There are two built-in module that provide serialization and deserialization, one is JSON the other is Pickle.
