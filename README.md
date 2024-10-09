# Test and Coverage MSBuild Target

Adds a custom MSBuild target which will run tests and generate a coverage report locally.

Mostly just a convenience so that those who use test-driven coverage reports in their CI pipeline can check coverage locally before pushing

# How to use

This may vary depending on which IDE (if any) you choose to use, but should be sufficient to get you going.

## Add the package source
If you don't already have a GitHub PAT, create one [here](https://github.com/settings/tokens) with minimal read:packages permissions to access public repositories.

Run this:
```
dotnet nuget add source "https://nuget.pkg.github.com/DanBlumenfeld/index.json" --name "Github Feed - DanBlumenfeld" --username "[github username]" --password "[PAT]"
```

## Add a reference to the package in each test project
Modify your .csproj as shown:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="TestAndCoverageTarget" Version="1.0.0" />
  </ItemGroup>
</Project>
```

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

## Review the coverage report
Found at {ProjectDir}/CoverageReport/index.html

