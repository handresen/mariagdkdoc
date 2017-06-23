# Useful tips

The protobuf serialization has been optimized, but it is also important that the draw objects do not carry unnecessary information.

This mean that attributes that do not have to be transferred should be reset:


*  Text = ""

*  Number = 0

*  Lists = empty

Also, the *Extra Fields* attribute can significantly increase the data size since values are represented both with attribute name and value. 

Tips!
The data size can be verified by checking the size of the protobuf buffer:

```csharp
//converts draw objects to protobuf using C2525			
var serializedBuffer = serializeFactory.Encode(listObjects, 
                                               SerializeModes.FullPrecision);
//get the size of the buffer                                             
var dataSize = serializedBuffer.Length;
```



