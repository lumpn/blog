# Language wars
`2014-11-14`

## Anecdote

Recently I had to brush up on my C++ knowledge for a job interview. I have been coding in C++ for maybe fifteen years now but for the last three years I've spend most of my programming time doing C# instead because it pays my bills. So I decided to get a quick overview of the fundamental syntactic, idiomatic, and ideological differences between C++ and C# to restore my memories.

I searched for [C++ vs. C#](http://google.com/search?q=c%2B%2B+vs.+c%23). My bad. I should have known better and I guess I even did but [hoped for some helpful results](http://xkcd.com/1334/) anyway. Instead I was presented lots of [opinionated](http://www.reddit.com/r/learnprogramming/comments/1elrmk/c_or_c_which_one_to_learn/) and [subjective](http://en.wikibooks.org/wiki/C%2B%2B_Programming/Programming_Languages/Comparisons/C_Sharp) attempts to answer ill-fated questions about [performance](http://stackoverflow.com/questions/138361/how-much-faster-is-c-than-c) and [features](http://www.thinkingparallel.com/2007/03/06/c-vs-c-a-checklist-from-a-c-programmers-point-of-view/). I will stop right here because I couldn't say what happens when you ask these kinds of questions any better than [Eric Lippert](http://ericlippert.com/2012/12/17/performance-rant/), [Jeff Atwood](http://blog.codinghorror.com/php-sucks-but-it-doesnt-matter/), and [Steve Yegge](https://sites.google.com/site/steveyegge2/tour-de-babel). In fact you should stop reading my blog now and read theirs instead.

## Language matters?

Don't ask which is better. Ask what is the right tool for the job. In the end you should base the choice of programming language solely on project constraints and productivity considerations. The latter in turn are likely much more dependent on the expertise of your team than anything else. And for the vast majority of all software performance doesn't even matter much. But then again the vast majority of software are apps, websites, and scripts. Not my line of business. I'm a specialized minority.

I do [massively parallel computations](http://blog.lumpn.de/diplomarbeit/) and [soft real-time applications](open-development.md). Stuff where you can't afford having an operation take ten milliseconds to complete. Obviously I care a lot for performance. When it comes to languages and libraries less overhead and more control is better for me – as long as productivity doesn't suffer too much. Because in the end someone has to pay for the time my colleagues and I spend on turning ideas into code.

On a more holistic scale for performance and productivity both C++ and C# are somewhat in the middle. Perhaps with [C++ favoring performance](http://en.wikipedia.org/wiki/C%2B%2B#Philosophy) and [C# favoring productivity](http://en.wikipedia.org/wiki/C_Sharp_(programming_language)#Design_goals) whenever the two are mutually exclusive. Which is less often than you might think. If you really need raw performance you should head towards C, assembler, programmable hardware, or even custom chips. Oppositely if productivity is prime look for existing solutions in terms of frameworks, libraries, or even complete software.

## C++ vs. C#

Lets compare these programming languages anyway. For reasons of scope I will just ignore all other programming languages. But you shouldn't! Always be aware of what tools are available. They might be the [better fit for the job](http://weblogs.asp.net/alex_papadimoulis/408925).

I will also not consider platform dependence at all. If you have a very specific target platform in mind or need to support a wide variety of platforms chances are high that C# is not an option. This is a good example of project constraints mentioned above. Make sure to get those right before going into details.

Because my post turned out much longer than envisioned I'm going to break the actual comparison up into multiple parts and link them here.

1. Memory management
2. Type system
3. Polymorphism
4. Syntax
