# Service addin infrastructure

The service addin infrastructure is based on standard .NET addin technology. For a full overview of this, the [msdn](https///msdn.microsoft.com/en-us/library/bb384200(v=vs.110).aspx) page is a good start.
This article will give an overview on how we have implemented our own addin infrastructure for hosting services.

## The Teleplan AddIn infrastructure

{{ :maria_gdk:internal:serviceaddins:serviceaddinstoplevelview.png?nolink&600 |}}
We have basically defined an interface that addins has to adhere to in order to be loaded as a service addin. This interface is actually more than just an interface, it is defining a pipeline that information can flow back and forth through. This pipeline is what connects a hosting application that loads addins and the addins themselves. The pipeline term is from the MSDN article linked above and is an intergral part of .NET addins technology.
We have tried to keep the pipeline as lean as possible. The only thing that should flow through the pipeline are simple control commands, like stop and start, and status information. This way we can control the addins from the hosting application, but we do not interfere with the actual service that is implemented in the addin.

## Hosting application

Currently we provide two hosting applications. One that is a standalone console application, and one that runs as a windows service. The task of a hosting application is to load addins that implement a given add in interface using the .NET addin library and provide a management interface in order to supply a way of controlling loaded addins and getting status information. The management interface is in the form of a WCF service that defines both a regular API and a REST based API.
One important role for the hostin application is that it defines base addresses to be used for all loaded addins. The means that as a deployer of Maria GDK, you only have to define the address and port in the hosting application. The service addins will then register themselves below that particular base address. For detail on the URI scheme used refer to the [Uri Scheme for addin services](maria_gdk/internal/serviceaddins/urischeme) wiki.
## AddIns

To create an addin follow the recipe from the [Create addin for service](maria_gdk/internal/serviceaddins/writeaddin) wiki article.
For us an addin is a hoster for a service of some kind. 

