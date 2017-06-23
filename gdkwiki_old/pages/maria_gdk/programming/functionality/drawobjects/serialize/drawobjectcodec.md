# IDrawObjectCodec

Create an instance using **new DrawObjectCodec()**.

### Reducing size of serialized data

If size of data is important, it is very important that all redundant attributes are reset before the objects are serialized. See [Useful tips](maria_gdk/programming/functionality/drawobjects/serialize/bestpractises) for more information.

### Converting draw objects to protobuf

The [ISerializeDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializedrawobjects) interface is used to transfer from draw objects to protobuf. 

### Converting protobuf to draw objects

The conversion supports data created both by [ISerializeDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializedrawobjects) and [ISerializeCompleteDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializecompletedrawobjects). It is not necessary to check the origin of the byte[] data.
