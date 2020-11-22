# Unity tips
`2020-11-22`

I saw a [post by Lazlo Bonin](https://twitter.com/lazlobon/status/1330224625901580297) today with a list of questions about how to structure projects in Unity. Here's my attempt to answer them.

| Question | Answer |
|----------|--------|
| How should I handle dependencies between my components? | `[SerializeField] private OtherComponent otherComponent;` and then assign in the inspector. |
| How should I implement singletons? | No. Use `ScriptableObject`s instead. |
| How should I handle cross scene references? | Use `ScriptableObject`s to hold the shared data, then reference from both scenes. |
| How can I use multiple scenes to my benefit? | Scenes are just like prefabs. |
| How should I organize my code assemblies? | Probably not at all. |
