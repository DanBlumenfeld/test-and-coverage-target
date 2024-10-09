# Test and Coverage MSBuild Target

Adds a custom MSBuild target which will run tests and generate a coverage report locally.

Mostly just a convenience so that those who use test-driven coverage reports in their CI pipeline can check coverage locally before pushing

# How to use

This may vary depending on which IDE (if any) you choose to use, but should be sufficient to get you going.

## Add the package source
Open the NuGet.Config file in your project or solution's root (or create it if it doesn't exist)
Add the source as shown:
```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!-- GitHub Packages as a public source -->
    <add key="github" value="https://nuget.pkg.github.com/DanBlumenfeld/index.json" />
  </packageSources>
</configuration> 
```

## Add a reference to the package in each test project
Modify your .csproj as shown:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <!-- Reference the custom GitHub package -->
    <PackageReference Include="TestAndCoverageTarget" Version="1.0.0" />
  </ItemGroup>
</Project>
```
NOTE: be sure to choose your desired version

## Restore the package
`dotnet restore`

## Run the custom target

### Command line
`dotnet msbuild -target:TestAndCoverageReport`

### Launch profile
In `launchsettings.json`:
```
{
  "profiles": {
    "Test & Coverage": {
      "commandName": "Executable",
      "executablePath": "dotnet",
      "commandLineArgs": "msbuild -target:TestAndCoverageReport",
      "workingDirectory": "$(ProjectDir)",
      "launchBrowser": false,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

