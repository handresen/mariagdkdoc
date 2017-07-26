---
title: Troubleshooting
keywords: GDK
tags: [troubleshooting, nuget ]
sidebar: clientapi_sidebar
permalink: clientapi_troubleshooting.html
summary: This section describes how to proceed when things go wrong when developing Maria GDK applications.
---

#  Troubleshooting

##  MariaUserControl not found.

*  Check projects *Target framework*, Properties⇒Application. See  [Development Requirements](clientapi_development_requirements.html) 

## Map info not displayed

*  Check that expected map services are available. 

*  Check endpoint definitions in app.config. 

*  Verify proper version of *SlimDx*. See [Development Requirements](clientapi_development_requirements.html) 

## FileLoadException unhandled

Appears when available version of an assembly to be loaded does not match the expected version.

Example: <br>
{% include image.html file="clientapi/troubleshooting/trs_fileloadexept.png" %}
{% include image.html file="clientapi/troubleshooting/trs_fileexplorer.png" %}

The version of the loaded protobuf-net assembly (in binary folder) is higher than the version expected by the Maira2012 NuGet assemblies.

This is fixed by adding a redirection statement for the assembly in the “app.configfile”:

```xml
<runtime>
	<assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
		<dependentAssembly>
			<assemblyIdentity name="protobuf-net"
				publicKeyToken="257b51d87d2e4d67"
				culture="neutral" />
			<bindingRedirect oldVersion="0.0.0.0-2.0.0.666"
				newVersion="2.0.0.666" />
		</dependentAssembly>
	</assemblyBinding>
</runtime>
```

## Memory leaks in MariaUserControl.

Memory leaks are quite common in WPF applications. Maria GDK is tested for memory problems before every release, but many of the problems can be caused by WPF usage outside of Maria GDK. Below are some pointers to common mistakes.

    * Timers in own view model has to be disposed/stopped.
    * Events that has been subscribed in your own view models needs to be unsubscribed.
    * The suggestion here is to implement IDisposable on your own view models and stop any timers and unsubscribe from events.

**Sample Dispose method for code behind of XAML that contains the MariaUserControl:**

```csharp
public void Dispose()
{
   BindingOperations.ClearAllBindings(this);
   //MariaCtrl is the name given to the MariaUserControl instance in XAML
   MariaCtrl.Dispose();
   MariaCtrl = null;

   //Setting DataContext to null avoids many problems that occur
   //with bindings to parts of Maria.
   DataContext = null;

   //_myViewModels is a list of all your own view models.
   foreach(var vm in _myViewModels)
   {
      IDisposable disposableVm = vm as IDisposable.
      if(disposableVm != null)
         disposableVm.Dispose();
   }
}
```

{% include links.html %}