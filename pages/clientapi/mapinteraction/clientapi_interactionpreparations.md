---
title: Preparations
keywords: 
tags: 
sidebar: clientapi_sidebar
permalink: clientapi_interactionpreparations.html
summary: This section describes the neccesary steps to prepare your application for interactions.
---
##  Preparing for interacting user controls

In this tutorial most of the interaction after the layer initialization is through GUI controls, like checkboxes and push buttons. To make it simple, the main window is divided in two, with GUI controls in one, and the Maria Control in the other. A poor example of how to build good user interface, though.

In the main part of the map window grid, define two rows -- one for interaction controls, and one for the MariaUserControl.

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <WrapPanel Grid.Row="0">

    </WrapPanel>
    <MariaUserControl:MariaUserControl Name="MariaCtrl" Grid.Row="1"
         ...
</Grid>

```

Definition and handling of GUI controls is described for the first sections only. For more details, refer to the sample project.

##  View model base class

For proper communication between the ***WPF-components*** in the applicattion GUI and the ***View Models***, we need to include some property event handling.

Create a view model base class (*ViewModelBase*) containing the following

```csharp
public class ViewModelBase : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    protected void NotifyPropertyChanged<TProperty>(Expression<Func<TProperty>> property)
    {
        var propertyName = ExtractPropertyName(property);
        PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }

    private static string ExtractPropertyName<TProperty>(Expression<Func<TProperty>> property)
    {
        var lambda = (LambdaExpression)property;
        MemberExpression memberExpression;
        if (lambda.Body is UnaryExpression)
        {
            var unaryExpression = (UnaryExpression)lambda.Body;
            memberExpression = (MemberExpression)unaryExpression.Operand;
        }
        else memberExpression = (MemberExpression)lambda.Body;

        return memberExpression.Member.Name;
    }
}
```

 To prepare the different view model classes, include inheritance from this class!

Update the following classes (from the ***BasicMapClient*** sample): 

*  *MariaWindowViewModel : ViewModelBase* 

*  *TrackViewModel : ViewModelBase* 

*  *MapViewModel : ViewModelBase* 



{% include links.html %}