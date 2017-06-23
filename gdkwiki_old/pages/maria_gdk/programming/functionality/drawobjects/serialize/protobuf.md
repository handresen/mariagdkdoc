# Protobuf

An advantage of the protobuf serialization is that unset attributes do not result in transferred data. This mean that the draw objects structure can be replicated without resulting in huge data transfer. The draw objects structure is replicated using protobuf version3 (proto3). 

Even though the protobuf serializes the data quit smartly, there are some attributes that produce unnecessary overhead. To avoid this, some attributes are serialized indirectly using bit-mapping. Normally, the definition of the protobuf serialization is an internal issue and should be of no concern.

#### boolean attributes

All boolean attributes in the draw object structure are bit-mapped into a common protobuf attribute.

#### enum attributes

All enum attributes in the draw object structure are bit-shifted into protobuf attributes.



