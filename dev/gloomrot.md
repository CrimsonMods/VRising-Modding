---
title: Gloomrot Migration Guide
parent: For Developers
---

### BepInEx **EXPERIMENTAL** [0.668.001](https://github.com/decaprime/VRising-Modding/releases/tag/0.668.001) 
Please don't distribute this, it's not final nor the way we want to distribute BepInEx


### Game Libs: [nuget](https://www.nuget.org/packages/VRising.Unhollowed.Client/0.6.0.571080001)
Initial version is not the *correct* way, just unblocking builds. Notice this privately packages the interop changes from above with the already interop'd client assemblies. This avoids the need for the MSBuild task creating the interop and is intended to get developers rolling on release date. 

Versioning on nuget is now game version+zero padded 4 digit version to clarify updates between game versions.


## `.csproj` changes
```xml
<TargetFramework>net6.0</TargetFramework>

...

<PackageReference Include="BepInEx.Unity.IL2CPP" Version="6.0.0-be*" IncludeAssets="compile" />
<PackageReference Include="BepInEx.Core" Version="6.0.0-be*" IncludeAssets="compile" />
<PackageReference Include="VRising.Unhollowed.Client" Version="0.6.0.57108*" />

```

## `nuget.config`

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="BepInEx" value="https://nuget.bepinex.dev/v3/index.json" />
    <add key="Samboy Feed" value="https://nuget.samboy.dev/v3/index.json" />
  </packageSources>
</configuration>
```