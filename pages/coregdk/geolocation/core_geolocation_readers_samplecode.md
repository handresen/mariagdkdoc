---
title: Sample geolocation reader code
keywords: geolocation, readers, example
tags: [geolocation]
sidebar: core_geolocation_sidebar
permalink: core_geolocation_readers_samplecode.html
summary: Sample code for creating geolocation readers.
---

To start you need to create a file containing a reader class for your location data. This class must implement the IPlaceNameDataInterfacer.

```csharp
namespace TPG.GeoFramework.LocationServiceCore
{
    public class GenericReader : IPlaceNameDataInterfacer
    {
        public void Dispose()
        {
        }

        public void CreateTables(string inputFileName)
        {
        }

        public void LoadData(string inputFileName)
        {
        }
    }
}
```

Start implementing *CreateTables*. *CreateTables* is responsible for creating tables used by the GeoLoc service when running placename searches. Mandatory tables are Rtree for spatial searches, FTS for free text search and main table for available placename information. 

Use helper functions in [IPlaceNameDB](http://support.teleplanglobe.com/MariaGDKDoc/html/715B3D03.htm) for creating tables in the main database. The main database will be created in advance and sent as a parameter to the readers constructor.

```csharp
        private readonly IPlaceNameDB _dbCore;

        public GenericReader(IPlaceNameDB dbCore)
        {
            _dbCore = dbCore;
        }

        public void CreateTables(string inputFileName)
        {
            _dbCore.CreateRTree();
            _dbCore.CreateFTS();
            _dbCore.CreateMainTables();
        }
```
        
Next step is to populate the tables with data from the datasource. This is done in the LoadData function and depends on the input format. <br/>
Main steps are:

*  Open input file

*  Start database transaction

*  Process header information if it exists

*  Process data and use the results to populate tables

*  Commit database transaction

*  Optimize data in FTS table

```csharp
public void LoadData(string inputFileName)
{
  DataFileStream = File.OpenText(inputFileName);
  ProcessHeader(DataFileStream.ReadLine());

  DbTransaction transaction = _dbCore.BeginTransaction();

  var rowId = (int) _dbCore.GetLastRowId("placenames_main"); 
  for (string row = DataFileStream.ReadLine(); 
       row != null; 
       row = DataFileStream.ReadLine())
    ProcessDataRow(row, rowId++);
    
  transaction.Commit();
  _dbCore.OptimizeFTSdata();
}
```

Example code how to add data to sqlitedatabase:

```csharp
private void AddMainData(string[] stringData, int rowId)
{
    // Note: column names must match those defined in PlaceNameDbResources
    StringBuilder sb=new StringBuilder();
    sb.Append("insert into placenames_main");
    sb.Append("(rowid, lat, long, feature_class, feature_code, 
              cc1, placename, placename_alt) ");
    sb.Append("values ("+rowId);
    sb.Append(", " + stringData[IdsToIdx["lat"]]);
    sb.Append(", " + stringData[IdsToIdx["long"]]);
    sb.Append(", '" + stringData[IdsToIdx["fc"]] + "'");
    sb.Append(", '" + stringData[IdsToIdx["dsg"]] + "'");
    sb.Append(", '" + stringData[IdsToIdx["cc"]] + "'");

    string placeName = DBCoreUtils.ToDbStringValue(
      stringData[IdsToIdx["full_name"]]);
    string altPlaceName = DBCoreUtils.ToDbStringValue(
      stringData[IdsToIdx["full_name_nd"]]);
    sb.Append("," + placeName);
    sb.Append("," + (altPlaceName!=placeName?altPlaceName:"''") +")");

    _dbCore.DoSQL(sb.ToString());
}
```

