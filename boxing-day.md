# Boxing Day

## Sugar, sugar

Suppose you have a list of items you want to iterate over and do something for each item in C#. Also suppose you want to avoid any heap allocations while doing so, therefore fancy list extensions using lambda expressions are out of question. So you write something like this:

```csharp
foreach (var item in list) {
    sum += item;
}
```

Which is syntactic sugar for:

```csharp
using (var iter = list.GetEnumerator()) {
    while (iter.MoveNext()) {
        sum += iter.Current;
    }
}
```

Which, in turn, is syntactic sugar for:

```csharp
var iter = list.GetEnumerator();
try {
    while (iter.MoveNext()) {
        sum += iter.Current;
    }
} finally {
    disposable.Dispose();
}
```

Sweet, sweet syntactic sugar! Profiling confirms that that last piece of code generates zero GC alloc, if list is of type `System.Collections.Generic.List<T>`. So all is well, right? Wrong! Both the foreach loop and the second version with explicit using block generate 48 bytes of garbage. So why is that?

## Boxing

There is a small but important lie in the code transformations above: the `var` keyword. The only reason why that third snippet generates zero garbage is because `List<T>` implements a custom enumerator which is a `struct`. `struct`s are stack allocated and gets passed by value, meaning they are copied every time they get passed around as a function parameter or a return value. So zero heap allocation.

But the `foreach` pattern does not know what it gets. It expects an `IEnumerator<T>` out of the `GetEnumerator()` call. And even though `List<T>.Enumerator` implements `IEnumerator<T>` a boxing conversion happens as soon as you write something like this:

```csharp
IEnumerator iter = list.GetEnumerator(); // boxing conversion!
```

Why? I don’t know. Bug, oversight, or feature? I don’t know. Does it happen for all versions of .NET? I don’t know. However, I do know that it happens in all versions of Unity released to this date (May 2016), and that is bad enough for me.

Also note that the same behavior is true for the `using` block which expects an `IDisposable`. Additionally a `using` block also emits a `null` check before disposing of the disposable, which of course doesn’t make any sense for a `struct` either.

```csharp
System.IDisposable disposable = ...; // potentially boxing!
try {
    ...
} finally {
    if (disposable != null) {
        disposable.Dispose();
    }
}
```

So bottom line neither the `foreach` statement nor the `using` block can take advantage of having a `struct` instead of a class getting passed to them.

## Workaround

I tend to use a plain `T[]` array instead of a `List<T>` whenever possible. If I have to use a list, I tend to use old school indexer loops to iterate over them:

```csharp
for (int i = 0; i < list.Count; i++ ) {
    var item = list[i];
    ...
}
```

To iterate over collections like `HashSet<T>` or `Dictionary<K,V>` I use the following pattern:

```csharp
for (var iter = hashSet.GetEnumerator(); iter.MoveNext();) {
    var item = iter.Current;
    ...
}
```

It isn’t quite as succinct as the `foreach` loop but almost. The `for` loop syntax with empty increment statement might feel weird at first. The worst part is that the code never disposes of the enumerator. Therefore it’s borderline dangerous or simply wrong. But then again, as far as I know, for these collections there is nothing to dispose of anyway.

## Remarks

C# compiles an [iterator block](https://msdn.microsoft.com/en-us/library/9k7k7cf0.aspx) using `yield` statements into an inner `class` implementing `IEnumerator`. Jon Skeet writes about [iterator blocks in great detail](http://csharpindepth.com/Articles/Chapter6/IteratorBlockImplementation.aspx). Note that it generates an inner `class`, not an inner `struct`! Therefore there is no further cost for boxing involved but `GetEnumerator()` itself already allocates memory on the heap.

Why did they choose to implement it like this? I don’t know. But the implementation does adhere to the design guidelines for [choosing between `class` and `struct`](https://msdn.microsoft.com/en-us/library/ms229017.aspx). More specifically the enumerator implementing the iterator block neither is immutable, nor necessarily smaller than 16 bytes, nor logically represents a single value, and most likely it will be used in a pattern requiring boxing anyway.

Funny though that they chose to implement collection enumerators as `struct`s, since those violate at least two or three of the four characteristics as well...
