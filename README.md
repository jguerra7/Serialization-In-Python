# Serialization-In-Python
Data serialization is the process of converting structured data to a format that allows sharing or storage of the data in a form that allows recovery of its original structure. In some cases, the secondary intention of data serialization is to minimize the data’s size which then reduces disk space or bandwidth requirements.

Since files work with text strings by default, to save Python data types like dictionaries or tuples, you could convert them to strings first, but an easier method is to serialize the data.

Serialization (pickling) allows you to save non-textual or binary information or transmit it over a network. Pickling essentially takes any data object, such as dictionaries, lists, or even class instances, and converts it into a byte set that can be used to reconstitute the original data. When pickling data, you must use the binary mode for files, as the data is being stored in the raw, rather than the normal text strings file operations are used to.
