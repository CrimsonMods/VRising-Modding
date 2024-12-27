---
title: Part 8 - What are Commands?
parent: Sample Introduction
nav_order: 9
---

# What are Commands?

[VampireCommandFramework](https://thunderstore.io/c/v-rising/p/deca/VampireCommandFramework/) or "VCF" is a mod that allows you to create commands in your mod. It is not required to make mods, but quite often you will want to have a command to do something. 

Let us take a look in the `Commands` folder and select `DiceCommand.cs`.

```csharp
[CommandGroup("dice")]
internal static class DiceCommands
```
You can see here above our Command class we use the CommandGroup attribute. This means that all commands in this class will be prefixed with `dice`.

```csharp
[Command(name: "roll", shortHand: "r", description: "Roll dice", adminOnly: false)]
public static void Roll(ChatCommandContext ctx, string dies = "1d20")
{
    if (!DiceService.ValidateHandOfDice(dies, out int die, out int amount))
    {
        ctx.Reply("Invalid dice format. Use the format 'AMOUNTdDIE' (e.g. 1d20)");
        return;
    }

    int result = DiceService.RollDice(die, amount);

    ServerChatUtils.SendSystemMessageToAllClients(Core.EntityManager, $"{ctx.User.CharacterName} Rolled {amount}d{die} = {result}");
}
```
And here is our command `roll` defined with the Command attribute. This defines the name of the command, the shorthand, the description, and if it is admin only.

In our method we define a `ChatCommandContext` which is a wrapper around the `ChatCommand` class. This is a class that contains all the information about the command that was sent. We can then use this to reply to the user. And we require an input parameter from the command called `dies`. This is the dice that the user wants to roll.

You can see that the `dies` parameter has a default value of 1d20. This means that if the user does not specify a dice, it will default to 1d20.

We check if the dice is valid using the `ValidateHandOfDice` method from the `DiceService`. If it is not valid, we reply to the user and return. If it is valid, we roll the dice and send the result to all clients.

So the command when typed into the chat would look like this:
```
.dice roll 1d20
```

And that covers all the systems and scripts included in the sample project. Next let's see how to build this into a usable DLL. 

Next [How to Build](09_sample_build.md)