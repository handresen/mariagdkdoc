# Using custom track symbol provider

This HowTo will describe how to use your own symbol provider for tracks. We will create a symbol provider that takes simple URI's to local image files.

*  Create your own symbol provider by implementing the [IRasterSymbolProvider](http://support.teleplanglobe.com/MariaGDKDoc/?topic=html/93379128-02b4-3153-4ccf-f546e583fef0.htm). In our example we call the class BitmapFileSymbolProvider.

*  Next, register the symbol provider to the track system using the [IMariaTrackLayer.SymbolProviders](http://support.teleplanglobe.com/MariaGDKDoc/?topic=html/83d97691-8c24-54e8-4ae9-97b17ba38506.htm)
`<code language="csharp">`
TrackLayer.SymbolProviders["BitmapFile"] = new BitmapFileSymbolProvider();
`</code>`

*  First we need to edit the style xml for the track module in order to map our new symbol provider. We must add the following under the `<stylecategory name="TrackSymbol">`.\\ Notice that the `<valueitem name="Symbology" value="''BitmapFile''">` corresponds to the name that we registered our symbol provider with.\\ The SymbolKeyField describes the field on the track that will be used as the identifier for the symbol to create. \\ The fieldcondition tag is used to filter tracks that will use this custom symbol provider. With the setup given here all tracks that have a field named "CustomSymbol" with the value "True" will use our custom symbol provider.

```xml
    `<style>`
      `<fieldcondition field="CustomSymbol" value="True"/>`
      `<compositeitem name="CoreSymbol">`
          `<valueitem name="SymbolKeyField" value="BitmapFileAddress"/>`
          `<valueitem name="Symbology" value="BitmapFile"/>`
      `</compositeitem>`
    `</style>`
```

   * The following code will add a track that uses the new symbol provider.

```csharp
double speed = 0.0;
double course = 0.0;
var pos = TrackLayer.GeoContext.CenterPosition;
var trackData = new TrackData(Guid.NewGuid, pos, course, speed) {
  ObservationTime = DateTime.UtcNow };
trackData.Fields["name"] = "SomeName";
trackData.Fields["BitmapFileAddress"] = @"C:\temp\test.png";
trackData.Fields["CustomSymbol"] = "True";
```
 
