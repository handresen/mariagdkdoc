# Creating client

This section aims to get a user started in creating a client for the map preparation service. Useful information for the rest seciton is the [API documentation](http://support.teleplanglobe.com/MariaGDKDoc/html/1A52F098.htm) for the service interface.\\
Regardless whether the simple process or the process with user supplied parameters is used the general way of using the service consists of the same steps.
 1.  Supply the path to the data thus starting the process.
 2.  Poll the service for progress using [GetProcessInfo](http://support.teleplanglobe.com/MariaGDKDoc/html/77E2D543.htm).
 3.  When finished you will have a map signature that can be used together with the [map template system](maria_gdk/maps/config/templates) to show the new data.
## Simple Path

With the simple path only the path to the source data is provided to the service using [ProcessDataSimple](http://support.teleplanglobe.com/MariaGDKDoc/html/AB174345.htm). The return value from this function has to be examined to check if a script was found for the datatype supplied.\\
It is important to note that there might be differences between datatypes on how a folder versus how a single file is handled. You can check the [Supported formats](maria_gdk/programming/functionality/mappreparation/formats) page for an overview of the supported formats and how they handle files versus folders.\\
As a rule of thumb, formats that support multiple files, a folder with data can be supplied and the service will create a map from the content in the folder. \\
On the other hand if the format of the data only supports single files, and a path to a folder is supplied, which file will be used is non-deterministic.\\
The safest way is to specify a single file for formats that only supports single files.\\
When the path has been supplied and the return value from [ProcessDataSimple](http://support.teleplanglobe.com/MariaGDKDoc/html/AB174345.htm) indicates that a valid script to prepare the data has been found you continue at step 2 in the process described in [Creating client](creatingclient#Creating client) section.
{{ :maria_gdk:programming:functionality:preparation_service:sequencesimple.png?direct&600 |}}
## Path with parameters

In this process an additional step to the list mentioned above has to be performed. In this step the path to the data is supplied to the map preparation service using the [GetAvailableScriptDefinition](http://support.teleplanglobe.com/MariaGDKDoc/html/649DAB97.htm) api. This step will perform some simple analysis of the supplied data and return the [script definition](http://support.teleplanglobe.com/MariaGDKDoc/html/1A87EA1A.htm) that will be used to prepare the supplied data.\\
This will give you extensive information on the script that the service will run in order to prepare the supplied data.\\
In essens the script contains some information on the script itself and a list of script tools that perform operations. Each of these script tools will have a [list of parameters](http://support.teleplanglobe.com/MariaGDKDoc/html/9B7D552F.htm) that can be used as a basis for a UI in order to supply the user with a way to adjust them.\\
Each parameter also contains a [category](http://support.teleplanglobe.com/MariaGDKDoc/html/EE553971.htm) that hints on the type that is expected.\\
When the script parameters are ready, the while script definition is sent together with the [data prepration job](http://support.teleplanglobe.com/MariaGDKDoc/html/F3C6381D.htm) definition to the [ProcessData](http://support.teleplanglobe.com/MariaGDKDoc/html/192672CC.htm) function. This starts the job and the returned id can be used for polling the service about progress.
{{ :maria_gdk:programming:functionality:preparation_service:sequencewithparameters.png?direct&600}}
