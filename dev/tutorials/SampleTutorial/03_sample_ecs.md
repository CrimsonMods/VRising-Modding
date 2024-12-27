---
title: Part 3 - What is ECSExtensions?
parent: Sample Introduction
nav_order: 3
---

# What is ECSExtensions?
This is a large reason for this Template. Every V Rising mod will contain a file called `ECSExtensions` or `Extensions`. 

These are extension methods that apply almost exclusively to the `Entity` type from Unity. The reason for this script is that reading, creating, and modifying Entity types is not as straight-forward as if V Rising was created with GameObjects (the traditional method of making games in Unity). Instead the game is made with the Entity Component System (or ECS). 

*Notice I don't include links here. Don't worry about what ECS is for now.*

Just know that every object in V Rising derives from `Entity` and these allow you to access them. This version of the file in the template contains XML Summaries. These are blocks of documentation that generate tooltips when you use them in Visual Studio. 

We'll go over a few of the most commonly used one though. 

```csharp
public unsafe static void Write<T>(this Entity entity, T componentData) where T : struct
```
`Write` is used for modifying the data on an Entity. Say you want to change the `Health` of an Entity? You would get reference to the `Health` data, change the values, and then `entity.Write(healthData)` to update it with your changes.

```csharp
public unsafe static T Read<T>(this Entity entity) where T : struct
```
`Read` is what allows you to read the data from an Entity. From the above example if you wanted to know the Health values you would need to use `entity.Read<Health>()` first. 

```csharp
public static bool Has<T>(this Entity entity)
```
You may wonder what would happen if you try to `Read` data that doesn't exist on the Entity? Well it will cause an exception. First you need to check that it exists with `Has` like `entity.Has<Health>()`.

```csharp
public static void Add<T>(this Entity entity);
public static void Remove<T>(this Entity entity);
```
These two are pretty self-explanatory. You can also add entirely new data to an Entity or remove it completely. 

## Are all (ECS)Extensions.cs created Equal?
Absolutely not. Although there is a lot of overlap in what this file will contain between modders, there are sometimes differences both in functions and even intention. 

For instance the one in this template also contains a development utility method called Dump

```csharp
public static void Dump(this Entity entity, string filePath)
```

Calling `entity.Dump("WhatInHere.txt")` on an Entity would create a file that lists all the available data on that entity. I use this method extensively while developing mods; so I include it in my Extensions.cs file. 

## Is this file the best place for these functions?
Maybe. That's a very opinionated topic. In the [VAMP](https://github.com/CrimsonMods/VAMP) framework this pattern is broken in favor of having `EntityUtil`, `DevUtil`, and other utility scripts.

Next [Part 4: What are Configs?](04_sample_config.md)
