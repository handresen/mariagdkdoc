# Example code

This lists different usage scenarios for the protobuf serialization.

#### Creating C2525 draw objects

The code examples uses a list of draw objects. For simplicity, the following code gives an example on how to create a C2525 draw object.

```csharp
//create ISimpleDrawObjects
var simpleDrawObject = drawObjectDataFactory.CreateTacticalGraphic(
                                             "TACGRP.TSK.DLY", 
                                             new SimpleDrawObjectFactory());
                              
var listObjects = new List`<ISimpleDrawObject>`();
listObjects.Add(simpleDrawObject);
```
#### Using IDrawObjectCodec

```csharp
//listObjects contains draw objects to serialize

//create serialization factory
var serializeFactory = new DrawObjectCodec();

//converts draw objects to protobuf using C2525			
var serializedBuffer = serializeFactory.Encode(listObjects, 
                                               SerializeModes.FullPrecision);
                                               
//converts protobuf back to ISimpleDrawObjects 
var serializedDrawObjects = serializeFactory.Decode(serializedBuffer);
```


#### Converting ISimpleDrawObjects to byte[] using ISerializeDrawObjects

```csharp
//listObjects contains draw objects to serialize

//create serialization factory
var serializeFactory = new ISerializeDrawObjects();

//converts draw objects to protobuf using C2525	
var protobufC2525 = serializeFactory .
                            CreateProtobufFromSimpleDrawObjects(
                                          listObjects, 
                                          SerializeModes.FullPrecision);

//get protobuf as byte[]
var buffer = serializeDataFactory.GetBuffer(protobufC2525);
```


#### Converting byte[] to ISimpleDrawObjects using ISerializeDrawObjects

```csharp
//buffer is byte[] created using ISimpleDrawObjects 

//create serialization factory
var serializeFactory = new ISerializeDrawObjects();

//get protobuf objects from byte[]
var transferred = serializeFactory.ReadFromBuffer(buffer);

//convert protobuf to draw objects
var reCreatedSimpleDrawObjects = serializeDataFactory.
                                       CreateSimpleDrawObjects(transferred);
```

#### Save ISimpleDrawObjects to protobuf file using ISerializeDrawObjects

```csharp
//listObjects contains draw objects to serialize

//create serialization factory
var serializeFactory = new ISerializeDrawObjects();

//converts draw objects to protobuf using C2525	
var protobufC2525 = serializeFactory .
                            CreateProtobufFromSimpleDrawObjects(
                                          listObjects, 
                                          SerializeModes.FullPrecision);

//store protobuf to file
serializeFactory.SaveToFile(protobufC2525, filename);
```

#### Create ISimpleDrawObjects from protobuf file using ISerializeDrawObjects

```csharp
//create serialization factory
var serializeFactory = new ISerializeDrawObjects();

//read protobuf from file
var protobufC2525 = serializeFactory.ReadFromFile(filename);

//create draw objects from protobuf5	
var transferedGdkObjects = serializeFactory.CreateSimpleDrawObjects(protobufC2525);
```
