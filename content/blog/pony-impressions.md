+++
author = "Aaron Kanter"
categories = ["tech"]
tags = ["pony", "language"]
date = "2016-05-18"
description = "Initial impressions of the Pony language"
featured = ""
featuredalt = ""
featuredpath = ""
linktitle = ""
title = "Giddyup: First Look at Pony Lang"
type = "post"
+++

A friend of mine jokingly brought up the [Pony](ponylang) language in the context that he's resigned himself to learn the language because his manager (also a mutual friend) is a staunch pony-phile (e.g., attended [BABSCon](http://www.babscon.com/), had a Rainbow Dash balloon floating above his desk at a previous job). At first I was intrigued: How many of the core designers are diehard [bronies](http://www.urbandictionary.com/define.php?term=Brony)? Is this a legitimate language or just horsing around? As it turns out, this is a **well thought out** language with central design ideas and, as far as I can tell, is not in any way related to My Little Pony. I'm going to take the rest of the post to give my $0.02 based on what I've read so far (the [Pony tutorial](ponytut) followed by a couple of [blog](post1) [posts](post2) by Bluish Coder/Chris Double) and what I've tinkered with.

### Overview
What do I mean by a "well thought out language"? [PonyLang](ponylang), from its inception, was designed to emphasize (in order of importance)

- **correctness**
- performance
- **simplicity**
- consistency
- completeness

Almost everything that I've read so far about Pony has pointed back to these principles (together called the "get-stuff-done" approach) and used this ordering to determine how to proceed on an issue. How should race conditions be handled? Well, a race condition is a form of "incorrectness" so eliminating them entirely became part of the plan. Ahead of time compilation? That should help with correctness and performance at the expense portability and scripting capabilities among others (neither of which you'll note are on the list).

What else can be said about the language? It's object-oriented, [actor-model](actor-model-wiki) (of Erlang fame), open-source, compiled, and very young (v0.1.0 relased in April 2015, currently at v0.2.1). There are a few [academic papers](papers) written about it, with more coming I'd imagine.

{{< figure src="/img/posts/ponylang_mascot.jpeg" caption="The brand new (May 13, 2016) mascot. His name is Main." class="align-center" attr="@ponylang" attrlink="https://twitter.com/SeanTAllen/status/731129619143925760">}}

### Pony is a natural stupidity repellant

You'll notice I emphasized "correctness" and "simplicity". I did this because of all of these guiding principles, these two are clearly the conceptual parents of another less specified, but wonderfully pervasive, doctrine: "save developers from shooting themselves in the foot". I'll call any characteristics born of this concept "DevSafe".

#### No Null
Yeah, they got rid of Tony Hoare's famed [billion dollar mistake](hoare-quote) called "null". Don't worry, there is still the primitive type `None`, but you'll never see it when you're not expecting it.

```
var x: String // x will only ever be a String. If there is a part of the code that could potentially assign x to be None, the compiler will spit out an error

var y: (String | None) // y can be a String OR None
```

DevSafe!

#### No global variables
Remember that [fatal acceleration bug](http://www.safetyresearch.net/blog/articles/toyota-unintended-acceleration-and-big-bowl-%E2%80%9Cspaghetti%E2%80%9D-code) Toyota had a couple of years ago? Well apparently they had 10,000 global variables meaning potentially 10,000 different ways for any part of the code to manipulate the rest of the system referencing those variables. Yuck.

DevSafe!

#### No variable shadowing
If you want to reuse a variable name, just stick a `'` (prime, from mathematics) on it. I can't tell you how many times I've been bitten by this in one way or another...Javascript would suck at least 5% less if this were true.

DevSafe!

#### Forced parens for operator precedence
I know people who have issues with this with simple arithmetic (c'mon guys, **p**lease **e**xcuse **m**y **d**ear **A**unt **S**ally), so the idea of people remembering operator precedence for more than _40_ in Java is patently absurd. If you mix your operators in an expression, the compiler will complain if you don't use parens.
```
var x = 1 + 2 + 3 // OK!
var y = 1 * 2 + 3 // NOPE! Compiler error.
var z = (1 * 2) + 3 // That'll work.
```

DevSafe!

#### Forced semicolons for same-line expressions
Similar to the point above, the compiler obviously knows when statements have ended just as it knows the internal operator precedences. The problem is that developers are easily confused.

```
var x = 1 let y = 2 // NOPE!
var x = 1; let y = 2 //OK!
```

DevSafe!

#### All exceptions must by handled -> far fewer crashes
The compiler checks that all exceptions are handled. That means if you have something built and running you know it won't ever throw a weird runtime exception and fall over. Obviously, you can still get yourself into problems (e.g., endless loops may not crash but they can still be bad, memory leaks can still exist). Also it's worth noting that if you call C code, you lose this guarantee (among many others).

DevSafe!

### Other interesting bits

#### Capabilities
[Capability calculus](http://tutorial.ponylang.org/capabilities/index.html) is really, really sweet. It is unlike anything I've ever seen built-in to any other language. In a nutshell, it is an annotation (explicit or implicit) that exists for every variable and parameter determining how it can be safely accessed, especially concerning itself across threads and between function calls. There are a total of 6 states and how each interacts with another or upgrades/downgrades is very clearly defined. Furthermore, **this is all checked at compile time**, so you'll know if you screwed up your data access across threads before you ever run your code! I love how clear it is to show whether a function even has the ability to mutate its arguments. It's fascinating that you can pass opaque pointers across actors (threads) and not worry about race conditions. It is a tremendous system that a LOT of thought went into and absolutely deserves its own post. I know some folks argue that the cognitive overhead is going to be too high for this, but 1) any new programming concept has an initially high cognitive overhead, but often you get used to it (promises, I'm looking at you) 2) the compiler-time checking creates a short feedback loops to help you learn the system and ensures you're using them properly.


#### Runtime Type Information
In many languages, runtime type information can be expensive or impossible to retrieve (e.g., Java has type erasure) and so it is generally avoided whenever possible. Not so in Pony! Type information is available at runtime and doing such a comparison at runtime is "generally just a pointer comparison" (their words, not mine - I haven't tested it to look at the LLVM code). This means you can actually have `match` statements (called `switch` in many other languages) based on the type of the variable. 

```
fun interestingFunction(unionVar: (U32 | CustomType | None)): String =>
  match unionVar
  | None => "none"
  | let ct : CustomType => "CustomType"
  | let u : U32 => "u32"
  end
```

It's interesting that while this type of checking can be done, reflection is not currently supported (though you can check whether `x is None`). I'm guessing this will change in a future release, but as of right now it's sitting in the [issue backlog](https://github.com/ponylang/ponyc/issues/87).
- Everything in the language is an expression. The result of an `if` (and most control structures for that matter) block is the last expression within the block or `None` if it doesn't execute. The result of a `break` is `None` unless it is followed by an expression. You get the idea.

#### Destructo-Read
Pony has destructive reads. This means that you can move variables around without needing a "3rd hand" like you normally would in most other languages.

```
var x : (String | U32) = "foo"
var y : (String | U32) = 1337
y = x = "bar"
y // "foo"
x // "bar"
```
This works because the value of the expression `x = "bar"` is `x`'s previous value, which we know to be `"foo"`. Taking it one step further
```
x = y = x
x // "foo"
y // "bar"
```
Neat! It also seems like it won't even negatively affect readability too much since only the right-most assignment can have real logic in it. Though, if people get too trigger-happy assigning variables mid-expression it may cause some comprehension problems.

```
var x : "bazz"
if (x = "foo") == "bazz" then
  env.out.print("that's not how Java works'") // Yep, that prints.
end
```

### The whole $0.02
Pony looks to be a serious contender to help "solve" concurrent programming. I probably would not recommend it to a beginning programmer right now since the tutorial and other resources are not super robust, but given the developer-friendly features and the very enthusiastic (though rather small) community I think it is absolutely worthwhile for more experienced developers to experiment with.

### Further information
- Web: https://www.ponylang.org/
- IRC: #ponylang @ irc.freenode.org
- Groups: https://pony.groups.io/g/pony
- GitHub: https://github.com/ponylang

[ponylang]: http://www.ponylang.org/
[ponytut]: http://http://tutorial.ponylang.org/
[post1]: https://bluishcoder.co.nz/2015/11/04/a-quick-look-at-pony.html
[post2]: https://bluishcoder.co.nz/2016/05/11/exploring-actors-in-pony.html
[hoar-quote]: https://www.lucidchart.com/techblog/2015/08/31/the-worst-mistake-of-computer-science/
[actor-model-wiki]: https://en.wikipedia.org/wiki/Actor_model/
[papers]: https://github.com/ponylang/ponylang.github.io/tree/master/papers/
