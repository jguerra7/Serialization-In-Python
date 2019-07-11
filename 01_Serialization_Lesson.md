# Serializing and Saving – JSON, YAML, Pickle, CSV, and XML
To make a Python object persistent, we must convert it to bytes and write the bytes to a file. We'll call this serialization; it is also called marshaling, deflating or encoding. We'll look at several ways to convert a Python object to a string or a stream of bytes.

Each of these serialization schemes can also be called a **physical data format**. Each format offers some advantages and disadvantages. There's no best format to represent the objects. We must distinguish a **logical data format**, which may be a simple reordering or change in the use of whitespace that doesn't change the value of the object but changes the sequence of bytes.

It's important to note that (except for CSV) these representations are biased towards representing a single Python object. While that single object can be the list of objects, it's still list of a fixed size. In order to process one of the objects, the entire list must be de-serialized. There are ways to perform incremental serialization, but they involve extra work. Rather than fiddling with these formats to handle multiple objects, there are better approaches to process many distinct objects in Storing and Retrieving Objects via Shelve, Storing and Retrieving objects via SQLite, and Transmitting and Sharing Objects.

As each of these schemes is focused on a single object, we're limited to objects that fit in the memory. When we need to process a large number of distinct items, not all of which can be in memory at once, we can't use these techniques directly; we'll need to move to a larger database, server, or message queue. We'll look at the following serialization representations:

## JavaScript Object Notation (JSON): 
This is a widely used representation. For more information, see http://www.json.org. The json module provides the classes and functions necessary to load and dump data in this format. In Python Standard Library, look at section 19, Internet Data Handling, not section 12, Persistence. The json module is focused narrowly on the JSON representation more than the more general problem of Python object persistence.
## YAML Ain't Markup Language (YAML): 
This is an extension to JSON and can lead to some simplification of the serialized output. For more information, see http://yaml.org. This is not a standard part of the Python library; we must add a module to handle this. The PyYaml package, specifically, has numerous Python persistence features.
## pickle: 
The pickle module has its own Python-specific representation for data. As this is a first-class part of the Python library, we'll closely look at how to serialize an object this way. This has the disadvantage of being a poor format for the interchange of data with non-Python programs. It's the basis for the shelve module in Chapter 10, Storing and Retrieving Objects via Shelve, as well as message queues in Chapter 12, Transmitting and Sharing Objects.
## The Comma-Separated Values (CSV) module: 
This can be inconvenient for representing complex Python objects. As it's so widely used, we'll need to work out ways to serialize Python objects in the CSV notation. For references, look at section 14, File Formats, of Python Standard Library, not section 12, Persistence, because it's simply a file format and little more. CSV allows us to perform an incremental representation of the Python object collections that cannot fit into memory.
## XML: 
In spite of some disadvantages, this is very widely used, so it's important to be able to convert objects into an XML notation and recover objects from an XML document. XML parsing is a huge subject. The reference material is in section 20, Structured Markup Processing Tools, of Python Standard Library. There are many modules to parse XML, each with different advantages and disadvantages. We'll focus on ElementTree.
Beyond these simple categories, we can also have hybrid problems. One example is a spreadsheet encoded in XML. This means that we have a row-and-column data representation problem wrapped in the XML parsing problem. This leads to more complex software to disentangle the various kinds of data that were flattened to CSV-like rows so that we can recover useful Python objects. Transmitting and Sharing Objects, and Configuration Files and Persistence, we'll revisit a number of these topics as we use RESTful web services with serialized objects as well as editable serialized objects for configuration files.

## Understanding persistence, class, state, and representation
Primarily, our Python objects exist in volatile computer memory. They can only live as long as the Python process is running. They may not even live that long; they may only live as long as they have references in a namespace. If we want an object that outlives the Python process or namespace, we need to make it persistent.

Most operating systems offer persistent storage in the form of a filesystem. This usually includes disk drives, flash drives, or other forms of non-volatile storage. It seems like it's simply a matter of transferring bytes from the memory to a disk file.

The complexity arises because our in-memory Python objects have references to other objects. An object refers to its class. The class refers to its metaclass and any base classes. The object might be a container and refer to other objects. The in-memory version of an object is a web of references and relationships. As the memory locations are not fixed, the relationships would be broken by trying simply to dump and restore memory bytes without rewriting addresses into some kind of location-independent key.

Many of the objects in the web of references are largely static—class definitions, for example, change very slowly compared to variables. Ideally, a class definition doesn't change at all. However, we may have class-level instance variables. More importantly, we need to upgrade our application software, changing class definitions, which changes object features. We'll call this the **Schema Migration Problem**, managing change to the schema (or class) of our data.

Python gives us a formal distinction between the instance variables of an object and other attributes that are part of the class. Our design decisions leverage this distinction. We define an object's instance variables to properly show the dynamic state of the object. We use class-level attributes for information that objects of that class will share. If we can persist only the dynamic state of an object—separated from the class and the web of references that are part of the class definition—that would be a workable solution to serialization and persistence.

We don't actually have to do anything to persist our class definitions; we already have an entirely separate and very simple method for that. Class definitions exist primarily as source code. The class definition in the volatile memory is rebuilt from the source (or the byte-code version of the source) every time it's needed. If we need to exchange class definition, we exchange Python modules or packages.

# COMMON PYTHON TERMINOLOGIES
Python terminology tends to focus on the words dump and load. Most of the various classes we're going to work will define methods such as the following:

**dump(object, file):** This will dump the given object to the given file
**dumps(object):** This will dump an object, returning a string representation
**load(file):** This will load an object from the given file, returning the constructed object
**loads(string):** This will load an object from a string representation, returning the constructed object
There's no standard; the method names aren't guaranteed by any formal ABC inheritance or the mixin class definition. However, they're widely used. Generally, the file used for the dump or load can be any file-like object. A short list of methods such as **read()** and **readline()** are required for the load, but we need little more than this. We can, therefore, use the **io.StringIO** objects as well as the **urllib.request** objects as sources for the load. Similarly, dump places few requirements on the data source. We'll dig into these file object considerations next.
