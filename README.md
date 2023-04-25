This is a repository for a minimal reproduction of an issue with solution filters on the latest version of nuget.
The problem is that nuget resolves the paths to the `.csproj` files in a solution filter.
It evaluates them from the path of the solution filter rather than the path of the solution.

For example in this repository, I have a solution filter as `src/nuget-slnf-bug/nuget-slnf-bug.slnf`.
It references the project `src/nuget-slnf-bug/nuget-slnf-bug.csproj`.

In the logs when it tries to resolve dependencies it tries to use the path ```src/nuget-slnf-bug/nuget-slnf-bug/nuget-slnf-bug.csproj``` and fails.

```
Run nuget restore src/nuget-slnf-bug/nuget-slnf-bug.slnf
MSBuild auto-detection: using msbuild version '15.0' from '/usr/lib/mono/msbuild/15.0/bin'.
WARNING: Project file /home/runner/work/nuget-slnf-bug/nuget-slnf-bug/src/nuget-slnf-bug/nuget-slnf-bug/nuget-slnf-bug.csproj cannot be found.
Nothing to do. None of the projects in this solution specify any packages for NuGet to restore.
```

This is further shown through the two workflows set up on this repository.
One uses the `dotnet restore` command.  The other uses the `nuget restore` command.
The `nuget restore` fails to restore the packages, but the `dotnet restore` works fine both using the same solution filter.