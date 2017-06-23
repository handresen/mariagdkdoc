# Track performance tips

### Use typed track fields for AnyIn filters

*AnyIn* allows very efficient many to many comparisons in field conditions/filters if used correctly. Note that *AnyIn* will work even if untyped (standard) track fields are used, however optimizations will not be enabled.
#### Condition/filter setup

To create a condition using AnyIn:

```csharp
new TrackFieldQuery
{
    op = TrackQueryFieldOp.AnyIn,
    fieldPath = "cargo",
    compareValue = "apples, pears, oranges"
}
```
#### Typed track field setup

The track system assigns field type automatically by using the field type for first track with that field value set. When using *ITrackData* to write data to the track service, make sure to use *TypedFields* to set the field type to *FieldType.FTStringList*. This will allow the track service to optimize filters using *AnyIn* field queries.

```csharp
ITrackData data=new TrackData(new ItemId("somelistid", "sometrackid"));
data.TypedFields["cargo"]=new FieldValue
{
	fieldId = "cargo",
	fieldType = FieldType.FTStringList,
	fieldValue = "bananas,cars,apples"
};
```

### Use server side cell data whenever possible for large data volumes

Reading large amounts of track data is time consuming. Use track cells to only transfer cell statistics between server and client. See [Cell styling](maria_gdk/programming/functionality/styling/track/stylingdetails/trackcell)


### Only update changed fields

The track system keeps previous field values for all tracks unless they are explicitly deleted. By only updating changed fields, the load on the service will be reduced.

### Use LockWrite when initialising database or when performing large updates 

Writing tracks to the track service is in general very efficient. However, if clients are constantly reading data, the writes can be slow. Avoid this by surrounding large write operations by LockWrite/UnlockWrite.

```csharp
Guid lockId=_serviceClient.LockWrite(10);	// Locks for max 10 seconds
_serviceClient.ProcessPackedTracks(trackList, updates);
_serviceClient.UnlockWrite(lockId);
//...
```
Note that Basic locking is automatically performed when using TrackServiceWriter. It can still be useful to explicitly lock if multiple write operations are planned, for instance when initialising track service from external data sources. Nesting locks is allowed, the lock will active as long as at least one sub lock is active. Calling LockWrite does not block the caller.
### Update client side field table

*Does not apply if using ITrackData/TrackData in conjunction with TrackServiceWriter.*
When updating tracks, the field table is used to compress track data and to reuse fields. The field table on the server is updated automatically each time tracks are written to the service. The utility classes *TrackServiceWriter* and *TrackServiceReader* automatically updates the client side field table. In order to achieve optimal track performance, avoid writing large batches of tracks without updating the field table. This can be achieved by writing a smaller set of tracks before performing the main updates. 

```csharp
ProcessTracksResponse ptr = _serviceClient.ProcessPackedTracks(trackList, updates);

if (ptr != null && ptr.fieldDefinitionUpdates != null &&
    ptr.operationResult == ProcessTracksResponse.OperationResult.Success)
{
    _fieldDefs.UpdateFieldDefinitions(ptr.fieldDefinitionUpdates);
}
else if (ptr != null && 
         ptr.operationResult == ProcessTracksResponse.OperationResult.DecodeFailure)
{
    // Failed for some reason. Reset field definitions...
    _fieldDefs.Reset();
}
```

### Use raw logs to spool large amounts of data

Using raw logs allow for much faster data entry. The track core does not need to consider updates from other sources, and rates upwards of 200K-400K tracks per seconds can be achieved. Logging can be enabled for any tracklist, to reconstruct the state, simply spool the entire log.

### Disable client read when initialising large track stores

If many clients are connected to the track store, initialising the store with large amounts of data can be very inefficient. Pause client track reading or do not start clients until the store has completed initialising.
