# M6M indexing

*Applies to Maria GDK 1.5 and older*

The FME and OGR translation processes described in this wiki produce m6m files. These must be indexed and converted to datasets before they can be used in MARIA. This is done with the command line tool "ToM6MDataset.exe", which is distributed by Teleplan Globe. 

Use as follows: Open a command-prompt and navigate to the folder where "ToM6MDataset.exe" is located. Type: 

```bash
ToM6MDataset.exe `<source folder>` `<target filename>` [temp folder]
```
Note that the target and temp folders must exist prior to execution.
## Optional parameters

*  ''-recursive'' if you want to go through subfolders as well. 

*  ''-cellsize'' followed by number of bytes. Default is 10000. Determines the size of the cells, and affects size of the m6mindex files. Smaller cells means larger indexes. Currently this parameter is only necessary to adjust if the dataset is very large and performance using the default value is poor.

## Example

```bash
ToM6MDataset.exe C:\Data\Natural_earth\m6m C:\Data\Natural_earth\NaturalEarth -t C:\Data\tempfolder -recursive -cellsize 1200
```

This will index the m6m files in the \m6m folder and produce the files NaturalEarth.m6mdata and NaturalEarth.m6mindex, ready to be used in a map package.


{{tag>[m6m] [m6mdataset] [indexing] [convert]}}


