---
title: Snap Shots
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_snapshots.html
summary: This section describes how to create snap shots (screen dumps) of information in the map area.
---
##  Create Snap Shot

Create an view model class (*SnapShotViewModel.cs*) for the Snap Shot functionality, inheriting ViewModelBase.

Create fields to hold a the list of Maria layers, and a field to hold a raster export object. Initiate the layers from constructor parameter.

```csharp
private ObservableCollection`<IMariaLayer>` _layers;
private MariaRasterExport _rasterExport;
. . .
public SnapShotViewModel(ObservableCollection`<IMariaLayer>` layers)
{
    _layers = layers;
}
```

Create an “export snap shot” command handler:

```csharp
public ICommand ExportSnapShotCommand
{
    get { return new DelegateCommand(x => ExportSnapShot()); }
}
private void ExportSnapShot()
{
    _rasterExport = new MariaRasterExport(_layers);
    SaveSnapShot();
}
private void SaveSnapShot()
{
    var dlg = new Microsoft.Win32.SaveFileDialog
                  {
                      FileName = "SnapShot-" + DateTime.Now.ToString("yyyy-MM-dd hh-mm-ss"),
                      DefaultExt = ".png",
                      Filter = "Png images (.png)|*.png"
                  };
    var result = dlg.ShowDialog();
    if (result == true)
    {
        try
        {
            var stream = new FileStream(dlg.FileName, FileMode.Create);
            var encoder = new PngBitmapEncoder {Interlace = PngInterlaceOption.On};
            encoder.Frames.Add(BitmapFrame.Create(ExportedBitmap));
            encoder.Save(stream);
            stream.Close();
        }
        catch (Exception exception)
        {
            Trace.WriteLine("Unable to export: {0}", exception.Message);
        }
    }
}
```

Then, create and include the *SnapShotViewModel*in the declarations and constructor of the main view model (*MariaWindowViewModel*):

```csharp
public SnapShotViewModel SnapShotViewModel { get; set; }
. . .
public MariaWindowViewModel()
{
    . . .
    SnapShotViewModel = new SnapShotViewModel(Layers);
}
```

Add a button with binding to the command handler:

```xml
<Button Name="ExportSnapShot" Margin="2" VerticalAlignment="Center"
                            Content="Export Snap Shot" ToolTip="Export Snap Shot" 
                            Command="{Binding SnapShotViewModel.ExportSnapShotCommand}"
                            />
```

Running the application and pressing the Export button, a bitmap image of the current map display will be created, and may be saved.

{% include image.html file="clientapi/interactionclient/snapshotall.png" caption="Snap shot, all layers" %}

{% include image.html file="clientapi/interactionclient/snapshotselected.png" caption="Snap shot, selected layers" %}

Note that information from all visible layers are included in the snap-shot, but the map components (mini map, ruler, pan navigation and Scale bar) are not.

In the sample application you will also find an example of previewing the snap shot image before saving it.

{% include links.html %}