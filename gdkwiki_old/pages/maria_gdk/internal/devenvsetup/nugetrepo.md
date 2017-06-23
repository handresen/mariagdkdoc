# NuGet Repositories

The Maria GDK build produces nuget packages that is ment for the GDK users externaly and internaly. In addition we also produces som packages that is itended for internal TPG use only. Those packages are available from:\\ 

*  Nightly: http://support.teleplanglobe.com/3AE31B8E-168B-4D42-9428-C5B62E86343C\nuget\\ 

*  Releases: http://support.teleplanglobe.com/6237D211-78BE-40ED-B98E-FA37E23AE62A\nuget
The external Maria GDK Nuget packages are loaded from the Teleplan Globe support site. Use the following source:

   * Nightly: [http://support.teleplanglobe.com/NuGetNightly/nuget](http://support.teleplanglobe.com/NuGetNightly/nuget) *Not guaranteed to be compatible with the released service installations.*
   * Release: http://support.teleplanglobe.com/NuGetReleases/nuget 

The API key for pushing packages to TPG Nuget Servers is: 7116FB97-C289-4FCC-B606-A1F75B8A3BC1

## TPG packages

The TC buildserver fetches packages from *U:\TPG-Packages\External* when building Maria GDK. After adding online nuget packages, they must be copied to this location, or the build will not pass!
