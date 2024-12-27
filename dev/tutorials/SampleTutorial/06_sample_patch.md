---
title: Part 6 - What is Patching?
parent: Sample Introduction
nav_order: 7
---

# What is Patching?
Okay. We are now on to the part where this gets complicated. Patching is the act of modifying the game's code. This is done by using a tool called [Harmony](https://harmony.pardeike.net/) to modify the IL (Intermediate Language) of the game. This was mentioned in the [Introduction](01_sample_intro.md) but is worth going into more detail because it is the core of modding the game's Systems. 

We left off the last chapter wondering where in the game we could call our Initialize method. Well we know a System in the game that will be called every time the game has finished loading. We are going to Patch that system. Open `Patches/InitializationPatch.cs`.

```csharp
[HarmonyPatch(typeof(SpawnTeamSystem_OnPersistenceLoad), nameof(SpawnTeamSystem_OnPersistenceLoad.OnUpdate))]
public static class InitializationPatch
{
    [HarmonyPostfix]
    public static void OneShot_AfterLoad_InitializationPatch()
    {
        Core.Initialize();
        Plugin.Harmony.Unpatch(typeof(SpawnTeamSystem_OnPersistenceLoad).GetMethod("OnUpdate"), typeof(InitializationPatch).GetMethod("OneShot_AfterLoad_InitializationPatch"));
    }
}
```
The first line there is the Attribute that tells Harmony what to patch. The first parameter is the type of the method we want to patch. The second parameter is the name of the method we want to patch. In this case we want to patch the `OnUpdate` method of the `SpawnTeamSystem_OnPersistenceLoad` class.

*Note: Your class must be static for Harmony to patch it.*

Above the method we have a `HarmonyPostfix` attribute. This is a special attribute that tells Harmony to run our method after the original method has finished. This is important because we want to make sure that our Initialize method has been called after the System has loaded.

Then we call our Core.Initialize method knowing that the game has finished loading.

And because this is a one-shot patch, we need to unpatch it after we have called it. This is done by using the `Unpatch` method of the Harmony instance. The first parameter is the type of the method we want to unpatch. The second parameter is the name of the method we want to unpatch. In this case we want to unpatch the `OnUpdate` method of the `SpawnTeamSystem_OnPersistenceLoad` class.

## Types of Patches
1. Prefix - Runs before the original method
   
   `[HarmonyPrefix]`
   
   These patches can prevent the original method from running by returning false.

2. Postfix - Runs after the original method
   
   `[HarmonyPostfix]`
   
   These patches are commonly used to modify the return value or perform actions after the original method.

3. Transpiler - Modifies the IL code of the method
   
   `[HarmonyTranspiler]`
   
   These patches directly modify the method's instructions, allowing for complex modifications.

4. Finalizer - Handles exceptions in the original method
   
  `[HarmonyFinalizer]`
   
  These patches run in a finally block and are useful for cleanup or exception handling.
   
  Example:
  ```csharp
      [HarmonyPatch(typeof(GameClass), "MethodName")]
      public static class FinalizerExample
      {
          [HarmonyFinalizer]
          public static Exception HandleException(Exception __exception)
          {
              if (__exception != null)
              {
                  Plugin.Logger.LogError($"An error occurred: {__exception.Message}");
                  return null; // Suppress the exception
              }
              return __exception; // Let the exception propagate
          }
      }
    ```
      
   
5. Reverse Patch - Allows calling the original method implementation
   
    `[HarmonyReversePatch]`
   
  These patches let you call private/internal methods of the original class.
   
    Example:
    ```csharp
      [HarmonyPatch(typeof(OriginalClass))]
      public static class ReversePatchExample
      {
          [HarmonyReversePatch]
          [HarmonyPatch("PrivateMethod")]
          public static string CallPrivateMethod(object instance, string parameter)
          {
              // This is a stub - Harmony will implement the actual method
              throw new NotImplementedException();
          }
      }
      ```

You can learn more about Patching [here](https://harmony.pardeike.net/articles/patching.html).

## Patching the Game
So now you're probably wondering how to patch other systems in the game. Well, there are an absolute ton of systems in the game. And a lot of the challenge in modding V Rising is finding the right system to patch and extracting the right data from the System to modify, remove, or add to.

Here are some different examples of patching systems in V Rising.

[CrimsonChatFilter - ChatMessageSystem](https://github.com/CrimsonMods/CrimsonChatFilter/blob/master/Hooks/ChatHook.cs)
[CrimsonMoon - DropItemThrowSystem](https://github.com/CrimsonMods/CrimsonMoon/blob/master/Hooks/Sacrafice.cs)
[KindredCommands - ServerBootstrapSystem](https://github.com/Odjit/KindredCommands/blob/main/Patches/RevealMapPatches.cs)
[OfflineRaidGuard - DealDamageSystem](https://github.com/CrimsonMods/OfflineRaidGuard/blob/master/Hooks/StatChange.cs)

Next [What are Services?](07_sample_services.md)