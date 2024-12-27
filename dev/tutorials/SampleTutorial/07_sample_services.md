---
title: Part 7 - What are Services?
parent: Sample Introduction
nav_order: 8
---

# What are Services?
Services are a way to make your mod more modular and reusable. They are a way to separate functionality into different classes that can be used anywhere in your mod. I like to categorize them into two categories: V Rising and Mod functionality. 

V Rising Services are those that are created to make interacting with a specific feature of the game easier or more efficient. For example, it is very common to see mods have a `PlayerService` that handles all the logic for player entities. This is a good example of a V Rising Service. You can see one [here](https://github.com/Odjit/KindredCommands/blob/main/Services/PlayerService.cs).

Then as included in the sample project, we have a `DiceService` that is a custom service that is used to roll dice in the `Services` folder. Let's take a look at the `DiceService` now.

```csharp
internal class DiceService
{
    public static List<int> ValidDice;

    public DiceService()
    {
        ValidDice = new List<int> { 4, 6, 8, 10, 12, 20, 100 };
    }
```
We can see that it's an [internal class](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/internal) that has a static list of valid dice. It also has a [constructor](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/constructors) that sets the list of valid dice.

The constructor is called when the service is created. This is important to know because it means that the service can be created in the `Core.cs` file and then used anywhere in the mod.

It then contains a series of methods that are related to rolling dice; keeping the Service limited to just that functionality which can be used anywhere.

```csharp
public static bool IsValidDie(int die)
{
    return ValidDice.Contains(die);
}

public static int RollDie(int die)
{
    return new Random().Next(1, die + 1);
}

public static int RollDice(int die, int count)
{
    int total = 0;
    for (int i = 0; i < count; i++)
    {
        total += RollDie(die);
    }
    return total;
}
```

Now that we have a basic understanding of what Services are, let's take a look at how they are used in the mod.

Services are typically either used in Patches or in this instance we'll be using them in VampireCommandFramework commands.

Next [What are Commands?](08_sample_commands.md)