# Accessing protobuf data

Both interfaces 
[ISerializeDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializedrawobjects)
 and [ISerializeCompleteDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializecompletedrawobjects) converts between draw objects and protobuf objects. 

In addition they also have methods to convert protobuf objects to/from byte[] and data file:

#### GetBuffer

Converts protobuf to byte[].
		
#### ReadFromBuffer

Converts byte[] to protobuf.

#### ReadFromFile

Converts data stored in file to protobuf.

#### SaveToFile

Stores protobuf to given filename.
