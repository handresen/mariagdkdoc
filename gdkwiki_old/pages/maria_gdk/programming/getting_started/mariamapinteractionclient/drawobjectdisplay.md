## Draw Object Visualization

### Draw Object Filtering

Display of tracks may be limited to tracks satisfying specific filter conditions, through the ***IMariaDrawObjectLayer*** ***DisplayFilter*** property//.//

Add filter properties to your interface class (//DrawObjectViewModel//)

```csharp
public bool FilterActive
{
    get { return (_drawObjectLayer.DisplayFilter != null) && _drawObjectLayer.DisplayFilter.Active; }
    set
    {
        if (_drawObjectLayer.DisplayFilter != null)
            _drawObjectLayer.DisplayFilter.Active = value;
    }
}
```
To your main window xaml, add a check box for enabling/disabling the filter, and bind it to the filter property above:

```xml
<CheckBox Content="Filter" ToolTip="Filter Active" 
          Name="chkDrawObjectDisplayFilter"                       
          IsChecked="{Binding DrawObjectViewModel.FilterActive}" Width="60" />
```
Create and add a composit draw object filter (during the DrawObjectLayerOnLayerInitialized event) 
. . .

```csharp
ICompositeCondition cc = new CompositeCondition{Operator = GroupOperator.Or};
cc.Children.Add(CreateStandardIdentityNotSetQuery());
cc.Children.Add(CreateFriendlyDrawObjectQuery());

_drawObjectLayer.DisplayFilter.Name = "Draw object Friendly or no identity";
_drawObjectLayer.DisplayFilter.Filter = cc;
_drawObjectLayer.DisplayFilter.Active = false;
NotifyPropertyChanged(() => FilterActive
);

AddTgObjects();
. . .
```

Create the filters like this:

```csharp
internal static ICondition CreateStandardIdentityNotSetQuery()
{
    var drawObjectCompositeQuery = 
        new CompositeCondition {Operator = GroupOperator.And};

    drawObjectCompositeQuery.Children.Add(
        new FieldCondition(
            TacticalDrawObjectDataFields.StandardIdentity,
            "", FieldOperator.Eq));

    return drawObjectCompositeQuery;
}
internal static ICondition CreateFriendlyDrawObjectQuery()
{
    var drawObjectCompositeQuery =
        new CompositeCondition { Operator = GroupOperator.Or };

    drawObjectCompositeQuery.Children.Add(
        new FieldCondition(
            TacticalDrawObjectDataFields.StandardIdentity,
            StandardIdentity.Friend,
            FieldOperator.Eq));

    drawObjectCompositeQuery.Children.Add(
        new FieldCondition(
            TacticalDrawObjectDataFields.StandardIdentity,
            StandardIdentity.AssumedFriend,
            FieldOperator.Eq));

    return drawObjectCompositeQuery;
}
```
Also add some tactical graphic objects to test the filters -- one hostile and one friendly:

```csharp
private void AddTgObjects()
{
    var objs = new List`<ISimpleDrawObject>` ();
    var obj = _drawObjectLayer.DrawObjectFactory.CreateTacticalGraphic( "STBOPS.VIOATY.ASN");
    obj.Points = new[]
                     {
                         new GeoPoint() {Latitude = 63, Longitude = 10}
                     };
    obj.TacticalDataFields.StandardIdentity = StandardIdentity.Friendly;
    objs.Add(obj);
    
    obj = _drawObjectLayer.DrawObjectFactory.CreateTacticalGraphic( "TACGRP.C2GM.SPL.LNE.AMB");
    obj.Points = new[]
                     {
                         new GeoPoint {Latitude = 63.20, Longitude = 9.6}, 
                         new GeoPoint {Latitude = 62.80, Longitude = 9.6},
                         new GeoPoint {Latitude = 62.80, Longitude = 10.4},
                         new GeoPoint {Latitude = 63.20, Longitude = 10.4}
                     };
    obj.TacticalDataFields.StandardIdentity = StandardIdentity.Hostile;
    objs.Add(obj);

    _drawObjectLayer.UpdateStore(objs.ToArray());
}
```

`<WRAP center round box>`
Zoom to the added tactical graphic objects, and add some plain objects as well. You should now be able to toggle between display of all objects, and display of friendly objects and objects with no identity.
`</WRAP>`


{{maria_gdk:programming:maria_2012_tutorial_html_m6dbb18c0.png|Draw Object Filtering}} \\ Figure: Draw Object Filtering.
### Draw Object style XML

 

The main mechanisms for controlling draw object appearance and styling is by providing draw object style information (draw object style xml). Several aspects of draw object visualization can be controlled using styling. Conditional styling allows styling based on draw object attributes, map attributes and external settings. For details on how to create and maintain draw object style xml, see “MARIA 2012 Draw Object Default Value Styling”.

The style xml is accessed through the draw object layer interface ***StyleXml*** property.

### Draw objects Symbol display

In addition to, and combination with the draw object style xml, the display may be controlled by scale and color scheme:


*  Symbol Scale \\ Scale factor for drawing of single point fixed size object symbols. Combined with symbol scale factors given by the draw object style XML, if any. Numeric value (double), normal range 0 -- 10.


*  Color Scheme \\ Applies to objects with defined color schemes, e.g. tactical graphic objects.\\ Enumeration, ***TPG.GeoFramework.Symbols.Contracts.SymbolColorScheme***\\ Values: { Dark, Medium, Light }

The following is an example on how to control draw object symbol display:

Add display properties to your interface class (//DrawObjectViewModel//)

```csharp
public double SymbolScale
{
    get { return _drawObjectLayer.SymbolScale; }
    set
    {        
        _drawObjectLayer.SymbolScale = ((int)(value*10))/10.0;// Adjust to one decimal...
        NotifyPropertyChanged(() => SymbolScale);
    }
}
public SymbolColorScheme DrawObjectColorScheme
{
    get { return _drawObjectLayer.SymbolColorScheme; }
    set { _drawObjectLayer.SymbolColorScheme = value; }
}
```

Add data provider for the color scheme enumeration to your app.xaml:

```xml
`<Application.Resources>`        
    <ObjectDataProvider x:Key="drawObjColorSchemeEnum" 
                        MethodName="GetValues" 
                        ObjectType="{x:Type System:Enum}">
        `<ObjectDataProvider.MethodParameters>`
            `<x:Type TypeName="SymbolsContracts:SymbolColorScheme"/>`
        `</ObjectDataProvider.MethodParameters>`
    `</ObjectDataProvider>`
    . . . 
```

Then, add a combo box for selection of color scheme, and a slider and a text box for size specification, to your main window (xaml).

```xml
<ComboBox Name="cmbDrawObjectColorScheme"                       
          ItemsSource="{Binding Source={StaticResource drawObjColorSchemeEnum}}" 
          SelectedItem="{Binding DrawObjectViewModel.DrawObjectColorScheme}" 
          IsSynchronizedWithCurrentItem="true"
          Width="Auto" />          
<Slider Grid.Row="0" 
        Name="sldDrawObjectDisplaySize" Width="100" 
        Minimum="0" Maximum="10" 
        Value="{Binding DrawObjectViewModel.SymbolScale}" />
<TextBox Width="32" 
         TextAlignment="Right" 
         Text="{Binding Path=Value, ElementName=sldDrawObjectDisplaySize, Mode=Default}" />

```

`<WRAP center round box>`
You should now be able to switch between different symbol scales and color schemes.
`</WRAP>`

{{maria_gdk:programming:maria_2012_tutorial_html_mb64b050.png|Different symbol scales}}\\ Figure: Different symbol scales.

{{maria_gdk:programming:maria_2012_tutorial_html_1832cc3b.png|Different color schemes}}\\ Figure: Different color schemes.

