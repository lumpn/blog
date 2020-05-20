# Imaginary problems
`2020-05-20`

## Summary
I prefer running into actual problems instead of solving imagined problems ahead of time. Mostly because it gives me confidence that the problem is actually a problem and also because it's easier to tell whether my solution actually solves the problem.

## Imagined problems
> When writing code, I really struggle with "future thinking". I can never just write the code with today's problem in my head. Instead, I'm constantly bogged down with "what if" scenarios and it really bogs me down. Any advice from seasoned devs on how to overcome this?
>
> &mdash; [@adbertram](https://twitter.com/adbertram/status/1262731817553190913)

We've all been there. After hearing about design patterns for the first time, I spent years obsessing over reusability and imagined future use cases. Don't do that.

## You aren't gonna need it
The best advice is to forget about the future, because it's going to turn out different anyway. Martin Fowler explains why [you aren't gonna need it](https://martinfowler.com/bliki/Yagni.html) and honestly you should just stop reading this and read his article instead.

The only thing I have to add is that it is much easier to solve the problems you have right now instead of also solving future problems, because you know how the solution will have to look like from the [outside-in](outside-in.md). Or at least you can find somebody to ask.

Everything else is just speculation. That's a waste of time. Don't do that. Instead, make sure your code lends itself to refactoring.

## Refactoring
Adapting to change requires refactoring. In itself, refactoring does not add customer value, so let's make sure to get it done as efficiently as possible. I found striving to [write code that is easy to delete](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to) particularly useful.

## Further reading
- [You aren't gonna need it](https://martinfowler.com/bliki/Yagni.html) by Martin Fowler
- [Write code that is easy to delete](https://programmingisterrible.com/post/139222674273/write-code-that-is-easy-to-delete-not-easy-to) by tef
- [Write code that is easy to debug too](https://programmingisterrible.com/post/173883533613/code-to-debug) by tef
- [State the Problem Before Describing the Solution](https://lamport.azurewebsites.net/pubs/state-the-problem.pdf) by Leslie lamport
