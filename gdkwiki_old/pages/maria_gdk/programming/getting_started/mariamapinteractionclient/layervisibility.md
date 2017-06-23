#  Layer visibility

The visibility of each layer is controlled individually through each layers Visibility property, ***IMariaInternalLayer.Visible***.

To select layers to be displayed, first add a layers property to the main view model (//MariaWindowViewModel//):

```csharp
public ObservableCollection`<IMariaLayer>` LayersDisplay
{
    get { return new ObservableCollection`<IMariaLayer>`(Layers);}
}
```

Then, add a list box with check items your XAML:

```xml
<ListBox Name="lstLayers" Height="Auto" Margin="2" 
             ItemsSource="{Binding LayersDisplay}" >
    `<ListBox.ItemTemplate>`
        `<DataTemplate>`
            <CheckBox Width="Auto" 
                      Content="{Binding Path=Name}" 
                      IsChecked="{Binding Path=Visible, Mode=TwoWay}" >
            `</CheckBox>`
        `</DataTemplate>`
    `</ListBox.ItemTemplate>`
`</ListBox>`
```

`<WRAP center round box>`
Running your application, you should be able to turn the different layers on/off !
`</WRAP>`

{{maria_gdk:programming:maria_2012_tutorial_html_m1dd63516.png|Layer visibility}}\\ Figure: Layer visibility.

