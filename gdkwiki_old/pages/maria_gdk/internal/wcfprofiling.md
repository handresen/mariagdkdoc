# Profiling WCF services using perfmon.exe

When profiling a service or a set of services, regular profiling software is not enough. The typical profiling software is aimed at profiling a single process, not how services interact.
Windows has a high performance logging framework called [ETW](https///msdn.microsoft.com/en-us/library/windows/desktop/aa363668(v=vs.85).aspx) that is used in WCF for logging events. The logs are on three levels:

*  service, this will log different metrics on service level. Meaning aggregated numbers for all endpoints and operations contained in a service.

*  endpoint, logs metrics for a given endpoint of a service. If you are only interested in traffic for e.g. the web endpoint, this can be very useful.

*  operation. Metrics are logged for each operation contained in the service.
Using the perfmon.exe software from Microsoft helps us to capture and analyze the ETW data.

## Settings up the services

For a service to use ETW logging it has to be enabled in the configuration of the service. A description on how to do this can be found on [MSDN](https///msdn.microsoft.com/en-us/library/ms735098%28v=vs.110%29.aspx).

## Capturing data

We will be using perfmon for capturing ETW data. perfmon has two modes, one for datacollection that can be used for future analysis. The other is a realtime monitor of events you choose to view. 
{{:maria_gdk:internal:perfmon_overview.png?direct}}
We will go through how to set up realtime monitoring of WCF ETW data. To make this process as easy as possible you should have the services you want to monitor running during this process.\\
First press the pluss sign under the perfomance monitor view. This will open a window where you can choose from a long list of ETW events. The ones we are interested in are

*  ServiceModelService 4.0.0.0

*  ServiceModelEndpoint 4.0.0.0

*  ServiceModelOperation 4.0.0.0
{{:maria_gdk:internal:perfmon_addcounter.png?direct|}}
    
When clicking on of these categories the window called "instances of selected object:" will be populated with services that are currently running on the system. The contents of the window are context sensitiv based on the ETW category selected. I.e. if endpoints are selected, all endpoints for all services are shown.\\
You should also drop-down the ETW category to get the actual ETW metric you want to view in order to filter out information that is not relevant.\\
When the metrics are added you can close the window and graphs with the selected data will start to show.




{{tag>[WCF perfmon profiling]}}
