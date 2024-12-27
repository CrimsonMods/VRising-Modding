---
title: Part 4 - What are Configs?
parent: Sample Introduction
nav_order: 5
---

# What are Configs?
We skipped on discussing the Settings script in the Plugin script explination, but now we are discussing the [BepInEx Config](https://docs.bepinex.dev/articles/user_guide/configuration.html) system. A TOML-like file generated in `BepInEx/config` for users to customize the experience of your mod. 

Having a detailed and descriptive config file can help your mod go from `It does X` to being able to do a varied number of configurations on different servers. 

Imagine you wanted to make a mod that changed the base stats of players. You would want to put each stat into their own Config entry; that way each server could configure how they want the stats to be instead of forcing your "hardcoded" values on every server that uses the mod. 

Let's start simple though. 

```csharp
 public static ConfigEntry<bool> ToggleMod { get; private set; }
 ```
 This defines a `ConfigEntry` with the type of `bool` (true or false) called `ToggleMod`. 

 Then in our InitConfig method we define it using the InitConfigEntry method. 
 ```csharp
 public static void InitConfig()
{

    ToggleMod = InitConfigEntry(OrderedSections[0], "Toggle", true,
            "If true the mod will be usable; otherwise it will be disabled.");

    ReorderConfigSections();
}
```
InitConfigEntry has four parameters. 

Section - The section it would appear under in the config.cfg. In this instance we pull from a List of titles, we'll come back to that. 
Key - The name of this config that will be shown in the config for users.
DefaultValue - The default value of the setting. In this instance we set it to true.
Description - Explains to the user what this setting does. 

Based on the description and key of this setting; we can see that Toggle will turn the mod on and off with true and false. 

We can then check this setting anywhere in the mod with the following code
```csharp
// You'll often see this in short-hand
// if (Settings.ToggleMod.Value) checks for True
// if (!Settings.ToggleMod.Value) checks for False
if (Settings.ToggleMod.Value == true) 
{
    DoThing(); // Mod is active lets do stuff
}
else
{
    return; // Mod is disabled, simply return and do nothing
}
```

## What is ReorderConfigSections?
You will often create a mod with a lot of different sections. BepInEx by default will put these in alphabetical order. Which means "Base", "Default", "Config" would all come after more in-depth or subsequent config sections. 

ReorderConfigSections allows you to define the specific order you want the seconds to appear in the config file by using a `List<string>`.

```csharp
private static readonly List<string> OrderedSections = new()
{
    "Config",
};
```
In this example we only have a single Section, but the code is included in the template anyways as it is quite common to have multiple sections. 

Next [What is Core?](05_sample_core.md)