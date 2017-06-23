# ISerializeCompleteDrawObjects

Create an instance using **new SerializeCompleteDrawObjects()**.

The [ISerializeCompleteDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializecompletedrawobjects) interface serializes the draw objects without C2525 "compression". 

The [ISerializeDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializedrawobjects) interface would normally be preferred, but this interface could be considered in the following scenarios:

##### No objects have valid C2525 codes

There is a small overhead using C2525 and using this interface will avoid this overhead.

##### C2525 conversion is not acceptable

Using the C2525 conversion, all "known" information is not transferred. Instead, this information is recreated when converting back to draw objects. Use this interface if this lack of transfer is not acceptable.

### Reducing size of serialized data

If size of data is important, it is very important that all redundant attributes are reset before the objects are serialized. See [Useful tips](maria_gdk/programming/functionality/drawobjects/serialize/bestpractises) for more information.


