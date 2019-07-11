# Serialization
Since files work with text strings by default, to save Python data types like dictionaries or tuples, you could convert them to strings first, but an easier method is to serialize the data.

Serialization (pickling) allows you to save non-textual or binary information or transmit it over a network. Pickling essentially takes any data object, such as dictionaries, lists, or even class instances, and converts it into a byte set that can be used to reconstitute the original data. When pickling data, you must use the binary mode for files, as the data is being stored in the raw, rather than the normal text strings file operations are used to.

The following screenshot shows how to use the pickle library:

![image](https://user-images.githubusercontent.com/47218880/61061542-0dadfd00-a3c2-11e9-8678-de1ad5ac607c.png)

## List pickling
We first import the pickle library in line 18, create a list of strings in line 19, and open a save file in line 20.

In line 21, the list is pickled to the file, then the file is closed. If you were to look at the current directory now, you would see that the pickled file has been created.

The file is reopened in line 23 and the pickled data loaded into a variable (line 24). When we print the new variable, we see the original list is returned.

In Python 3, only the pickle library is available. For Python 2.x, there are two different pickle libraries for Python: cPickle and pickle. Since Python is interpreted, it runs a bit slower compared to compiled languages, such as C. Because of this, Python has a precompiled version of pickle that was written in C; hence cPickle. Using cPickle makes your program run faster; if you decide to use it, you might consider importing cPickle as pickle, so you're not always typing cPickle.

Shelves are similar to pickles except that they pickle objects to an access-by-key database, much like dictionaries; as a matter of fact, the pickle library provides the backend functionality. Shelves allow you to simulate a random-access file or a database. It's not a true database but it often works well enough for development and testing purposes. Alternatively, shelves can be used when the values will be class instances, recursive data types, containers with multiple sub-objects, and similar Python objects.

The following screenshot shows how the shelve library is used:

![image](https://user-images.githubusercontent.com/47218880/61061595-21596380-a3c2-11e9-8d2c-897c36b7fb1a.png)

## Shelve object persistence
The shelve library is imported, and a new shelve object is created (lines 32 and 33). A new list is created in line 34, then the shelve object is populated (lines 35 and 36) with the list of strings from the preceding screenshot, as well as the list from line 34.

The two lists within the shelve object are assigned to new variables (lines 37 and 38). We can verify this action by looking at the individual variables in lines 39 and 40.

One thing to note about pickling: there is no data security/integrity performed. Passing pickled objects around or storing them doesn't prevent the data from becoming corrupted or being maliciously modified. Never unpickle data from untrusted or unauthenticated sources.
