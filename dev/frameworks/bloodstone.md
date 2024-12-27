---
title: Wetstone -> Bloodstone
parent: For Developers
nav_exclude: true
has_toc: false
---

![bloodstone-banner](https://i.imgur.com/Py0MwUL.png)

---

Bloodstone is a modding library for both client and server mods for V Rising. By itself, it does not do much except allow you to reload plugins you've put in the Bloodstone plugins folder.

## Installation

### CLI

```
dotnet add package VRising.Bloodstone
```
or 

### csproj
```csproj
<PackageReference Include="VRising.Bloodstone" Version="0.1.1" />
```

### Configuration

Bloodstone supports the following configuration settings, available in `BepInEx/config/gg.deca.Bloodstone.cfg`.

**Client/Server Options:**
- `EnableReloading` [default `false`]: Whether the reloading feature is enabled.
- `ReloadablePluginsFolder` [default `BepInEx/BloodstonePlugins`]: The path to the directory where reloadable plugins should be searched. Relative to the game directory.

**Client Options:**
- Bloodstone keybinding can be configured through the in-game settings screen.

**Server Options:**
- `ReloadCommand` [default `!reload`]: Which text command (sent in chat) should be used to trigger reloading of plugins.


## Migration from Wetstone

Follow installation steps and either way make sure to clear out any Wetstone references you have in your project.

### Namespace changes

You can safely find and replace to do this update, or do it manually:
`Wetstone` -> `Bloodstone`

### Package Id Change

`xyz.molenzwiebel.Wetstone` -> `gg.deca.Bloodstone`
