# nuget-license-minimal
Minimal example project to show error in [nuget-license](https://github.com/sensslen/nuget-license) when adding `NuGet.Config` file to solution and using project 
filters.

## Test Cases

### 1. Run without `NuGet.Config` (works)
Remove/Rename `NuGet.Config` and run
```bash
nuget-license -i NugetLicenseMinimal.sln > output/LICENSES.html
```

### 2. Run with `NuGet.Config` (works)
```bash
nuget-license -i NugetLicenseMinimal.sln > output/LICENSES.html
```

### 3. Run without `NuGet.Config` and with `--json-input` filter (works)
Remove/Rename `NuGet.Config` and run
```bash
nuget-license --json-input projects-filter.json > output/LICENSES.html
```

### 4. Run without `NuGet.Config` and with `--json-input` filter (doesn't work)
```bash
nuget-license --json-input projects-filter.json > output/LICENSES.html
```

or

```bash
tests=$(find src/*/*.csproj -name '*.csproj' ! -path "src/Tests.*/*.csproj" -exec echo -n '"{}", ' \;) \
    && tests=${tests::-2} && echo "[${tests}]" > output/projects-filter.json \
    && nuget-license --json-input output/projects-filter.json > output/LICENSES.html
```

throws exception
```c#
Unhandled exception. NuGet.Configuration.NuGetConfigurationException: Unexpected failure reading NuGet.Config. Path: '/mnt/c/repositories/misc/nuget-license-minimal/src/WebApi/WebApi.csproj/nuget.config'.
 ---> System.IO.IOException: The file '/mnt/c/repositories/misc/nuget-license-minimal/src/WebApi/WebApi.csproj' already exists.
   at System.IO.FileSystem.CreateDirectory(String fullPath, UnixFileMode unixCreateMode)
   at System.IO.Directory.CreateDirectory(String path)
   at NuGet.Configuration.FileSystemUtility.AddFile(String fullPath, Action`1 writeToStream)
   at NuGet.Configuration.FileSystemUtility.GetOrCreateDocument(XDocument content, String fullPath)
   at NuGet.Configuration.SettingsFile.<>c__DisplayClass23_0.<.ctor>b__0()
   at NuGet.Configuration.SettingsFile.<>c__DisplayClass32_0.<ExecuteSynchronized>b__0()
   --- End of inner exception stack trace ---
   at NuGet.Configuration.SettingsFile.<>c__DisplayClass32_0.<ExecuteSynchronized>b__0()
   at NuGet.Common.ConcurrencyUtilities.ExecuteWithFileLocked(String filePath, Action action)
   at NuGet.Configuration.SettingsFile.ExecuteSynchronized(Action ioOperation)
   at NuGet.Configuration.SettingsFile..ctor(String directoryPath, String fileName, Boolean isMachineWide, Boolean isReadOnly)
   at NuGet.Configuration.Settings.ReadSettings(String settingsRoot, String settingsPath, Boolean isMachineWideSettings, Boolean isAdditionalUserWideConfig, SettingsLoadingContext settingsLoadingContext)
   at NuGet.Configuration.Settings.<>c__DisplayClass29_0.<LoadSettings>b__0(String f)
   at System.Linq.Enumerable.SelectEnumerableIterator`2.MoveNext()
   at System.Linq.Enumerable.WhereEnumerableIterator`1.MoveNext()
   at System.Collections.Generic.List`1.AddRange(IEnumerable`1 collection)
   at NuGet.Configuration.Settings.LoadSettings(String root, String configFileName, IMachineWideSettings machineWideSettings, Boolean loadUserWideSettings, Boolean useTestingGlobalPath, SettingsLoadingContext settingsLoadingContext)
   at NuGet.Configuration.Settings.LoadDefaultSettings(String root)
   at NuGetUtility.Program.GetPackageInfos(ProjectWithReferencedPackages projectWithReferences, IEnumerable`1 overridePackageInformation, CancellationToken cancellation) in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/Program.cs:line 155
   at NuGetUtility.Program.<>c__DisplayClass35_0.<OnExecuteAsync>b__1(ProjectWithReferencedPackages p) in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/Program.cs:line 132
   at NuGetUtility.Extension.AsyncEnumerableExtension.SelectMany[TSource,TResult](IEnumerable`1 input, Func`2 transform)+MoveNext() in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/Extension/AsyncEnumerableExtension.cs:line 21
   at NuGetUtility.Extension.AsyncEnumerableExtension.SelectMany[TSource,TResult](IEnumerable`1 input, Func`2 transform)+System.Threading.Tasks.Sources.IValueTaskSource<System.Boolean>.GetResult()
   at NuGetUtility.LicenseValidator.LicenseValidator.Validate(IAsyncEnumerable`1 packages, CancellationToken token) in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/LicenseValidator/LicenseValidator.cs:line 35
   at NuGetUtility.LicenseValidator.LicenseValidator.Validate(IAsyncEnumerable`1 packages, CancellationToken token) in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/LicenseValidator/LicenseValidator.cs:line 35
   at NuGetUtility.Program.OnExecuteAsync(CancellationToken cancellationToken) in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/Program.cs:line 133
   at McMaster.Extensions.CommandLineUtils.Conventions.ExecuteMethodConvention.InvokeAsync(MethodInfo method, Object instance, Object[] arguments)
   at McMaster.Extensions.CommandLineUtils.Conventions.ExecuteMethodConvention.OnExecute(ConventionContext context, CancellationToken cancellationToken)
   at McMaster.Extensions.CommandLineUtils.Conventions.ExecuteMethodConvention.<>c__DisplayClass0_0.<<Apply>b__0>d.MoveNext()
--- End of stack trace from previous location ---
   at McMaster.Extensions.CommandLineUtils.CommandLineApplication.ExecuteAsync(String[] args, CancellationToken cancellationToken)
   at McMaster.Extensions.CommandLineUtils.CommandLineApplication.ExecuteAsync[TApp](CommandLineContext context, CancellationToken cancellationToken)
   at NuGetUtility.Program.Main(String[] args) in /home/runner/work/nuget-license/nuget-license/src/NuGetUtility/Program.cs:line 93
   at NuGetUtility.Program.<Main>(String[] args)
Aborted

```