# Serialize

Objects of type **ISimpleDrawObject** can be serialized using the **TPG.DrawObjects.Serialize.dll** component.The objects are serialized using protobuf version3 (proto3). 

The protobuf data can either be stored to file or retained as byte[] (see [Accessing protobuf data](maria_gdk/programming/functionality/drawobjects/serialize/protobufdata)).

The serialization can be done using different interfaces offering slightly different functionality. 

## IDrawObjectCodec

The [IDrawObjectCodec](maria_gdk/programming/functionality/drawobjects/serialize/drawobjectcodec) interface encapsulates the serialization and offers a very convenient interface to convert to/from byte[]. 

//This interface is recommended if the purpose is to transfer draw object to/from byte[] as easily as possible.//

## ISerializeDrawObjects

The [ISerializeDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializedrawobjects) interface uses C2525 to reduce the size of serialized data. Only the differences from corresponding C2525 draw objects are serialized. All wellknown-information based on the C2525 codes are recreated when converting back to draw object thereby reducing serialized data. 

Draw objects without valid C2525 codes are still supported by completely serialize all attributes.



## ISerializeCompleteDrawObjects

The [ISerializeCompleteDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializecompletedrawobjects) interface serializes the draw objects without C2525 "compression". 

The [ISerializeDrawObjects](maria_gdk/programming/functionality/drawobjects/serialize/serializedrawobjects) interface would normally be preferred, but this interface could be considered in the following scenarios:

##### No objects have valid C2525 codes

There is a small overhead using C2525 and using this interface will avoid this overhead.

##### C2525 conversion is not acceptable

Using the C2525 conversion, all "known" information is not transferred. Instead, this information is recreated when converting back to draw objects. Use this interface if this transfer method is not acceptable.
