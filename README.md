This is a repository for a minimal reproduction of an issue with solution filters on the latest version of nuget.
The problem is that nuget resolves the paths to the `.csproj` files in a solution filter.
It evaluates them from the path of the solution filter rather than the path of the solution.

For example in this repository, I have a solution filter as `src/nuget-slnf-bug/nuget-slnf-bug.slnf`.
It references the project `src/nuget-slnf-bug/nuget-slnf-bug.csproj`.

In the logs when it tries to resolve dependencies it tries to use the path ```src/nuget-slnf-bug/nuget-slnf-bug/nuget-slnf-bug.csproj``` and fails.


# Failed NuGet Restore

In a github action, it shows the following result with nuget restore.

```
Run nuget restore src/nuget-slnf-bug/nuget-slnf-bug.slnf
MSBuild auto-detection: using msbuild version '15.0' from '/usr/lib/mono/msbuild/15.0/bin'.
WARNING: Project file /home/runner/work/nuget-slnf-bug/nuget-slnf-bug/src/nuget-slnf-bug/nuget-slnf-bug/nuget-slnf-bug.csproj cannot be found.
Nothing to do. None of the projects in this solution specify any packages for NuGet to restore.
```

It cannot find the .csproj file.  Note the extra nuget-slnf-bug directory in the path output.

You can see this behavior in the `.NET nuget restore` GitHub actions workflow on this repository.

# Successful Dotnet Restore 

dotnet restore works fine for the same solution filter.

```
Run dotnet restore src/nuget-slnf-bug/nuget-slnf-bug.slnf
  Determining projects to restore...
  Restored /home/runner/work/nuget-slnf-bug/nuget-slnf-bug/src/nuget-slnf-bug/nuget-slnf-bug.csproj (in 2.15 sec).
```

It can find the .csproj file.

You can see this behavior in the `.NET dotnet restore` GitHub actions workflow on this repository.