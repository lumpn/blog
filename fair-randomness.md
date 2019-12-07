# Fair randomness
`2016-05-16`

## Evolution

This is an addendum to my last post about [randomness and fairness](apparent-probability.md). As I was implementing a randomizer in the spirit of the random threshold model discussed before, a nice simplification occurred to me. I knew the probabilities `p` in my use case would be small, between 5% and 30%, and long streaks of bad luck can feel like a bug to the user. Therefore I wanted a tight upper bound for a streak of bad luck, so I chose `c = 0.5`, which results in an upper bound of `2 / p`, while still allowing a two-streak of good luck. The code looked simple enough:

```csharp
public bool Random(float probability) {
    entropy += probability;
    if (entropy >= random(0.5, 1.5)) {
        entropy -= 1;
        return true;
    }
    return false;
}
```

I noticed a couple of problems though.

1. It is somewhat likely to return `true` for probability `p = 0%` and below
2. It is somewhat likely to return `false` for probability `p = 100%` and above
3. The first call can never return `true` for small `p`, if `entropy` is initialized to `0`.

It is easy enough to handle points #1 and #2 explicitly, but what about the initialization of entropy? Of course I want the probability of returning true on the first call to be equal to `p`. It is easy to see that `entropy = 0.5` guarantees exactly that. But then one can just subtract `0.5` from both sides of the threshold test and obtain the following simplified randomizer:

## Elegance

```csharp
public class FairRandomizer {
    private float entropy = 0;

    public bool Random(float probability) {
        // handle edge cases
        if (probability <= 0) {
            return false;
        } else if (probability >= 1) {
            return true;
        }

        // randomize with entropy
        entropy += probability;
        if (entropy > random()) {
            entropy -= 1;
            return true;
        }
        return false;
    }
}
```

Simply elegant, isn't it? Feel free to use the code above.
