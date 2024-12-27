---
title: Part 5 - What is Core?
parent: Sample Introduction
nav_order: 6
---

# What is Core?
Like `ECSExtensions.cs` you will also very commonly see a script called `Core.cs`. This is an initializer (factory) God Singleton. A bit of an anti-pattern, but also the most common way in Unity to manage and track subsystems. The alternative and proper way of doing this in C# would be with Dependency Injection, but in the words of every game dev I've ever met: [Keep It Simple, Stupid](https://en.wikipedia.org/wiki/KISS_principle). Sometimes a God object is just what you need to get things done. 

```csharp
 public static World Server { get; } = GetServerWorld() ?? throw new Exception("There is no Server world (yet)...");
 public static EntityManager EntityManager => Server.EntityManager;
 ```
 Here are two of the most commonly referenced things in all of V Rising. `World` is how you can access majority of systems in the game. It is the "root". We call it `Server` typically because we're doing mainly making server mods. Would probably call it `Client` if doing a client mod, but would need to do the GetClientWorld route instead? I don't make client mods.

 `EntityManager` this is how you can access any Entity anywhere. You'll see it referenced a lot in that `Extensions.cs` file, but for the most part the beginning of this tutorial doesn't interact with Entities much. We will use this more in later tutorials. 

 ```csharp
 public static DiceService DiceService { get; internal set;}
 ```
 This isn't a built-in type of V Rising (ProjectM or Stunlock Assemblies), but a custom service that is defined in the `Services` folder by us. We need to define it somewhere and since this is our God object, we create all our services in this script. 

 ```csharp
 public static void Initialize()
{
    if (hasInitialized) return;

    DiceService = new DiceService();
    hasInitialized = true;
}
```
We can see that this script has a single public method called Initialize. This is the magic. When it gets called it creates our needed services which can then be used anywhere in our mod. It also sets a static bool to ensure that it can't be re-initialized because of the [early return](https://www.c-sharpcorner.com/article/early-return-pattern-in-c-sharp/).

Now you might be wondering how do we call this method? Where is the correct place to do so if services depend on information from the Server that needs to load?

That leads us smoothly to... Next [What is Patching?](06_sample_patch.md)