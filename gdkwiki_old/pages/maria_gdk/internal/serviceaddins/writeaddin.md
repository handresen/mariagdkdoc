# Create addin for service

## Creating the project
In the Hosts.AddInHosting.AddIns directory of the main solution create a new class library project. Open the project properties and inspect the build section. Change the output directory to ..\..\bin\Debug\AddInRoot\AddIns. This ensures that the addin is places where our hosters look for new addins, thus this is the only step you have to perform in order for the addin hosters to pick up your addin.

Add System.AddIn and System.AddIn.Contract reference to the project. 

Add the AddInView project as a reference. When you have added the AddInView project, right click in the references folder of the project and select properties. Change the **Copy Local** property to false. This as done because the AddInView project is part of the add in pipeline and is loaded externally by the add in loading application.

## Create the add in class

In your newly created project add a new class. Let this new class implement **IAddInView**, and implement the functions according to their documentation.

Add the AddInAttribute and set of QualificationDataAttribte to your class:

```csharp
[AddIn("My Service Add In", 
       Version = "1.4.0.0", 
       Publisher = "Teleplan Globe AS", 
       Description = "Add in for my fantastic service.")
[QualificationData("Isolation", "NewAppDomain")]
[QualificationData("SupportsMultipleInstances", "false")]
public class MyNewAddIn : IAddInView
{
}
```
 ==== Supported QualificationData ====
 | Key                       | Value                  | Description                                                                                                                                                       | 
 | ---                       | -----                  | -----------                                                                                                                                                       | 
 | Isolation                 | NewAppDomain (default) | Ensures that the add in runs in its own app domain. This is the minimum level of isolation we support                                                             | 
 | Isolation                 | NewProcess             | The add in will run in its own process.                                                                                                                           | 
 | Platform                  | x86                    | This is used together with Isolation:NewProcess. The process the add in will be hosted in will run as a 32 bit process.                                           | 
 | Platform                  | x64                    | This is used together with Isolation:NewProcess. The process the add in will be hosted in will run as a 64 bit process.                                           | 
 | Platform                  | AnyCpu (default)       | This is used together with Isolation:NewProcess. The process the add in will be hosted in will run as a Any Cpu process.                                          | 
 | SupportsMultipleInstances | true                   | The add in can be instantiated multiple times from the controlling hoster app. The add in itself has to ensure that it registeres a unique URI for each instance. | 
 | SupportsMultipleInstances | false (default)        | The add in can only be instantiated once.                                                                                                                         | 
## Add app.config file 

Each add in has its own app.config to use. Add this to your project and add desired configurations to it. Minimum are the endpoints to use for your service.
For address scheme and endpoints to offer for the service refer to [Uri Scheme for addin services](maria_gdk/internal/serviceaddins/urischeme) wiki.


