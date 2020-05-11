# Outside-in code
`2020-05-11`

## Summary
I'm writing code from the outside-in, starting with the code a user of my software would write, working my way backwards to satisfy those needs.

## Failed paradigms
Over the last 25 years I've seen many different software development paradigms come and go. I used to chase some of them, but most turned out to be counterproductive, especially when taken to the extremes. For example, I most certainly don't believe in [SOLID](https://en.wikipedia.org/wiki/SOLID), [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), or [TDD](https://en.wikipedia.org/wiki/Test-driven_development) anymore.

## Outside-in paradigm
That said, the one paradigm that has been consistently given me good results over the years is writing code from the outside-in, meaning that instead of coding the building blocks first and then putting them together, I pretend the building blocks were already there, so I can focus on putting them together first.

What that allows me to do is to quickly understand the problem domain I'm trying to provide a solution for in the first place.

## Example
For example, I recently came across a [tweet from Eric Lippert](https://twitter.com/ericlippert/status/1256295400274685952), who designs programming languages for a living, complaining about [regular expressions](https://en.wikipedia.org/wiki/Regular_expression).

> I am one of those passionate dislikers but it is not just the terrible notation. It's that with arithmetic we have temporary variables and function abstraction to lower the cognitive load and enable composition, but you almost never see regexes built compositionally.

And I thought, well yes, why is it that we can write arithmetic expressions like `a = b + 3`, but we can't write regular expressions like `word = letter OR number`?

Applying the outside-in paradigm, I then thought, well, what if we _could_? What would that even look like? In pseudocode maybe something like:

```csharp
// 555-555-5555
digit = [0-9];
dash = '-';
space = ' ';
separator = dash OR space;
threeDigits = digit * 3;
fourDigits = digit * 4;
phone = threeDigits + separator
      + threeDigits + separator
      + fourDigits;
```

While certainly not perfect and also lacking lots of functionality you'd expect from regular expressions, I already find it much easier to reason about than the equivalent "baked" expression.

```csharp
// 555-555-5555
phone = "\\d{3}[- ]\\d{3}[- ]\\d{4}"
```

Given the promising concept, how would it look like in a language like C#, without adding new keywords and without clashing with existing syntax? So I wrote a test exploring the problem domain.

```csharp
[Test]
public void TestRegex()
{
  // +1 555-555-5555
  // country code and separators optional

  var dash = Pattern.Dash;
  var space = Pattern.Space;
  var separator = dash | space;
  var optionalSeparator = new Optional(separator);

  var optionalPlus = Pattern.Plus.Optional();

  var digit = Pattern.Digit;
  var someDigits = new OneOrMore(digit);
  var countryCode = optionalPlus + someDigits;

  var phone = (countryCode + separator).Optional()
            + digit * 3 + optionalSeparator
            + digit * 3 + optionalSeparator
            + digit * 4;

  var regex = phone.ToRegex();

  Assert.IsTrue(regex.IsMatch("+1 555-555-5555"));
  Assert.IsTrue(regex.IsMatch("1-555-555-5555"));
  Assert.IsTrue(regex.IsMatch("555-555-5555"));
  Assert.IsTrue(regex.IsMatch("555 555 5555"));
  Assert.IsTrue(regex.IsMatch("+911-555-555-5555"));
}
```

You might notice some overlap in functionality, for example there is `new Optional(foo)` and also `foo.Optional()`. That is because my primary goal is not to come up with a minimal API, but with an API that _serves the user_, and one of the questions to answer along the way is whether a [fluent interface](https://en.wikipedia.org/wiki/Fluent_interface) is better than and [object oriented](https://en.wikipedia.org/wiki/Object-oriented_programming) one or vice versa.

If I had written the building blocks first, I would have probably ended up with a very object heavy API, which quickly becomes cumbersome to compose. Especially when using operator overloading, which I find very fitting to compose regular expressions, classes just tend to get in the way.

```csharp
var separator = new Alternation(dash, space);
var phone = new Concatenation(new Repeat(digit, 3), ...);
```

Or I might have chosen to put everything in one class and expose a very fluent heavy interface, which makes it easy to compose sequentially, but hard to reuse parts. Once captures come into play, it breaks down fast.

```csharp
var phone = Pattern.Repeat(digit, 3)
                   .Alternation(dash, space).Optional()
                   .Repeat(digit, 3)
                   .Alternation(dash, space).Optional()
                   .Repeat(digit, 4);
```

Instead, by focusing on the problem I want to solve, I was able to quickly iterate on the syntax I want to expose eventually. If you are curious how it went, take a look at my [regex composer](https://github.com/lumpn/regex-composer) prototype.

## Further reading
- [Outside–in software development](https://en.wikipedia.org/wiki/Outside%E2%80%93in_software_development) on Wikipedia
- [State the Problem Before Describing the Solution](https://lamport.azurewebsites.net/pubs/state-the-problem.pdf) by Leslie lamport
