# Unity tips
`2020-11-22`

I saw a [post by Lazlo Bonin](https://twitter.com/lazlobon/status/1330224625901580297) today with a list of questions about how to structure projects in Unity. Here's my attempt to answer them.

| Question | Answer |
|----------|--------|
| How should I handle dependencies between my components? | `[SerializeField] private OtherComponent otherComponent;` and then assign in the inspector. |
| How should I implement singletons? | No. Use `ScriptableObject`s instead. |
| How should I handle cross scene references? | Use `ScriptableObject`s to hold the shared data, then reference from both scenes. |
| How can I use multiple scenes to my benefit? | Scenes are just like prefabs. |
| How should I organize my code assemblies? | Not at all. It's not worth it. |
| Where should I put my GUI? | In a prefab. |
| How do I tween (without a plugin)? | Use an animation clip. |
| How can I write coroutines without polluting my code? | You probably want `async`/`await` at this point. |
| How should I let my designers trigger some logic? | Use Unity events. |
| How do I make my components look good? | Buy Odin Inspector. You won't regret it. |
| How can I avoid memory allocation? | Use a profiler. |
| Should this be a class, a struct, an interface? | `struct`s for small data. Other than that you're probably only going to need `MonoBehaviour`s, `ScriptableObject`s, and some `static` utility classes.
| What classes should I derive from? | No. |
| Should I use inheritance or composition? | Composition. |
| How can I save and inspect dictionaries? | Use a list of key-value pairs instead. |
| What's the deal with `null` and `UnityEngine.Object`? | It's bad. Stay away from it and use `implicit operator bool` instead. |
| How do I modify objects from editor scripts and get it to save? | `EditorUtility.SetDirty(target);` |
| How should I refer to a scene in a build? | By name. |
| What should I use for event handling? | Use Ryan Hipple's runtime sets. |
| How should I organize the contents of my script file? | Group similar things together. |
| How do I rename things in code without breaking things? | Use a proper IDE. |
| How can I optimize my playmode entry time? | Reduce dependencies. Make prefabs and scenes playable standalone. |
| How should I access "global" assets? | By direct reference assigned in the inspector. Addressables for streaming. |
| How should I load all the stuff required for my game to run from any scene? | It should all be `ScriptableObject`s anyway. No loading. |
| Where should I put global configuration values? | In `ScriptableObject`s.
| How do I execute some logic automatically before my builds? | Don't. You're solving the wrong problem. |
| Which static flags should I check? | If it doesn't move, probably all of them. |
| How many layers do I need? | More than you can have. |
| How do I extend the Unity Timeline? | Use `PlayableBehaviour`. |
| What input solution should I use? | The `com.unity.inputsystem` package. |
| How do I reuse values across fields? | Use `ScriptableObject`s as variable holders. |
| What should I use instead of `FindObjectsOfType`? | Runtime sets. |
| Where should I test new features in my project? | In test scenes. |
| How do I setup my version control? | Use GitHub to generate a project with the right `.gitignore` file. |
| How do I troubleshoot Unity when it crashes or freezes? | Check the editor log in `%LOCALAPPDATA`. |
| How should I split my components? | Follow the single responsibility principle. |
| In which order do build-in events happen? | Check [Unity's documentation](https://docs.unity3d.com/Manual/ExecutionOrder.html). |
| What coding style should I use? | The same as everyone else on your team. |
| Where should I put my editor code? | In an `Editor` folder. |

Please watch [Ryan Hipple's excellent talk on game architecture](https://www.youtube.com/watch?v=raQ3iHhE_Kk) and read this [guide on modular design](https://unity.com/how-to/architect-game-code-scriptable-objects). Both cover most of the questions above in detail.
