# Preparation Service

The preparation service is basically a service that executes scripts on input geo data in order get the data ready for deployment to Maria GDK map infrastructure. The goal if this wiki with its sub parts is to give a technical overview of the inner workings of the service. For a wiki of the api side of things see the [Map Preparation wiki](maria_gdk/programming/functionality/mappreparation)

## Main Concepts

The preparation service really consists of three main parts. 

*  The script execution engine. This part of the service is responsible for orchestrating the execution scripts. It has the capability to run multiple scripts in parallell.

*  The scripts. These define steps included in a script, the execution flow and the parameter transferring between different steps of the scripts. 

*  The script tools. These are the tools that are available for use in the scripts. Their functionarlity can range from doing all functionality in code to be simple runners of external command line tools.
