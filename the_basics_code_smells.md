# The Basics, Part 2: Code Smells

In this Part 2 of **The Basics** series, I will talk about Code Smells, another
one of the concepts I believe to be fundamental to understand for writing
software at a professional level.

The ability to recognize and correct Code Smells is one of the basic abilities
that distinguishes inexperienced developers from more senior ones.

This post ins not meant to be an in-depth explanation of every code smell, but
rather an introduction to them and a refresher that will allow you to reflect on
whether you know and remember them correctly.

## What are Code Smells

Code Smells are signs, or indicators, of possible problems within your code.
Each one of these code smells have a relation with one or more remediating
refactoring techniques.

The **possible** word here is important. Not all code that has a smell needs
refactoring, as sometimes these "rules" might be deliberatively broken, or even
if they are not intentionally overlooked, in some cases you might be just fine
with a piece of code stinking a bit.

Churn is an important factor; if you have a file that's a little stinky, but
hasn't been modified in years, maybe there's no need to clean it up.

A code smell indicates a violation of a fundamental design principle.

## Catalog

Code smells have been discovered and documented since the 1990s; this is an old
concept that has been proved to be fundamental.

So since they have been being discovered and documented for quite a while, they
have been also named and catalogued.

Names are already recognized to be one of the most important things in writing
software; they allow us to comunicate important information in a consice and
precise way.

Knowing the names of code smells allows for easy communication between software
developers, and enable a shared understanding of the fundamental problems that
one encounters when writing it.

Code smells have been categorized into five groups: the Bloaters, the
Dispensables, the Couplers, the Change Preventers, and the Object-Orientation
Abusers.

## Bloaters

The Bloaters are pieces of code that have grown to unmanageable proportions.

Most of the time, bloaters don't appear immediately, but rather grow over time
when no one cares to fix them by refactoring.

There are three of them that need no explanation besides their name, these are
the Long Method, the Large Class, and the Long Parameter List.

The best way to deal with them is to have a guideline on what constitutes a long
method, a large class, and a long parameter list. For me these guidelines are
the Sandi Metz rules for developers, which state that:

* No method should be longer than 5 lines of code
* No method should take more than 3 arguments
* No class should be longer than a 100 lines of code

The numbers themselves are arbitrary, and don't really matter that much, you
might settle with slightly different numbers, but having them is the important
thing.

There are cases to break these rules; the more strict you are, the faster you
will learn what this cases are.

When I learned this rules I applied them absolutely 100% of the time. Despite
what some, to my judgement lazy, developers might tell you, this is completely
achievable, and completely adequate. These rules are a shortcut for learning
that will carry you further and way faster to where you should be aiming to go.

Besides these three, there are two other bloaters: Primitive Obsession, and Data
Clumps.

Primitive Obsession is, to my judgement, one of the code smells that even most
experienced developers fail to correct in their craft.

Primitive Obsession is the failure to create types that wrap around a single
primitive object. Let's say in the language you are using there exists no
unsigned integers, and you are using an integer in a place you only expect
positive numbers to appear; that's primitive obsession over there, you have the
option to create an `UnsignedInteger` type, but you chose not to, making your
program more prone to bugs by not counting on the type safety of a user defined
class.

If you are constantly passing around hashes, arrays, numbers, strings, symbols,
et c, that's primitive obsession. You can, and should, create your own types and
classes to represent these concepts in your codebase. Just as you have a class
called `Post`, you might have a class called `PostsCollection`, instead of
passing an array of posts around.

When I see Primitive Obsession in the code of a developer, I immediately know
that they are not that experienced yet; I know that sounds harsh, but it is that
important of a concept.

Lastly there are data clumps. If you have certain parameters or objects that are
always being passed around together, that's a data clump smell. The most typical
example of failing to identify data clumps are the methods that take a `from`
and a `to` arguments, typically dates. The solution is to introduce a new class,
for example: `Dates { from: Date, to: Date }`.

Bloaters are by far the most profitable smells to learn; if you work on solving
your bloaters, incidentally you will learn a huge lot of things about software
design; that's why I went long in this section, the other ones will be shorter.

## Dispensables

Dispensables are things you don't need, as the name implies.

The three more important dispensables are: Comments, Duplicated Code, and Dead
Code.

Dead code is code that is not being used; delete that immediately, if you ever
need to check what that code did once, you have git for that.

Duplicated code might be hard to identify sometimes, some languages like Ruby
have tools that help you find it. The solution to it is refactoring by
introducing the abstraction that's missing. Duplicate code is always a sign of a
missing abstraction. The rule of thumb is that two instances of duplicated code
is tolerable, but three is not.

Comments... these are the most dispensable of all. The best comment possible is
good naming. If your methods, classes and variables are properly named, you will
seldom need any comments. Comments should be reserved for very very special
cases, where the reason of code being a certain way is not evident. If you are
writing comments in all of your methods, you are definitely doing something
wrong.

The only exception for that is if you are writing the documentation for a
project with inline comments, which is the way some communities do it for public
projects. Private software does not need comments.

The other Dispensables are: Data Classes, Lazy Classes, and Speculative
Generality, which I will leave for your own research. I don't consider the first
two to be too bad, but the third one is, and is related to the YAGNI principle
we discussed in Part 1.

## Couplers

Couplers are... signs of high coupling, as you might have guessed. They are: the
Feature Envy, the Inappropriate Intimacy, the Incomplete Library Class, the
Message Chains, and the Middle Man.

Feature Envy is when you access more data from dependencies than you do from
your own (if you are a class or method, that is).

Inappropriate Intimacy is when you access private data or methods from another
object or class. This can happen mostly because of two reasons: either you have
not properly declared the accessibility of a method, or your language let's you
do something that you shouldn't be doing (looking at you Ruby). Don't do this,
seriously.

Incomplete Library is when a library doesn't meet your needs anymore, and
modifying it might be impossible. That's a situation no-one likes to be in; you
might end up patching it, or better yet, writting your own.

Message Chain is simply the violation of the Law of Demeter, which we discussed
on the previous article. It's code that looks like `A.b().c().d()`.

I'm more of a functional programmer than an OOP one, and with that I have the
view that if you are only using query methods that are pure, it's fine to have
some method chains arising from the usage of methods like `map`, `filter` and
`reduce` and others that behave in a similar way. You can always create the
appropriate classes to hide these details if necessary.

The Middle Man is that class or object that the only thing it does is passing
one message from one place to another, without doing anything special to it. Not
to be confused with some design patterns that do sit in the middle of other two
objects but help them communicate by making their interfaces compatible.

Particularly important for dealing with bloaters are the Dependency Injection
technique, and the Open-Closed, Interface Segregation, and Dependency Inversion
principles, plus the Law of Demeter.

Ok, that's the couplers for you.

## Change Preventers

In a sense, all code smells make changing code more difficult, but these ones do
it the most. They are: Divergent Change, Parallel Inheritance Hierarchies, and
the Shotgun Surgery, which has the coolest name of them all.

Divergent Change is whenever you are changing something in a class, and you end
up having to change methods unrelated to the main change. This might be a
consequence of the Large Class bloater, a violation of the Open-Closed Principle
or some other thing.

Parallel Inheritance Hierarchies is kind of self-explanatory; you have two
different Inheritance Hierarchies that are parallel to each other, meaning that
for each subclass or superclass in one, you will have a matching class on the
other one. This is most usually solved by moving the specialized bits into one
of the hierarchies and simplifying the other one into a single class.

A Shotgun Surgery happens whenever that you intend to make changes in one place,
you end up having to make multiple changes in multiple files. This is similar to
the Divergent Change, with the difference that the Shotgun Surgery happens at
many places, and the Divergent Change happens to multiple parts of one unit.
This usually happens from violations to the Single Responsibility Principle,
where a single responsibility is separated into multiple classes or pieces of
code. It is a sign of low Cohession.

## Object-Orientation Abusers

These are incorrect or incomplete applications of Object-Oriented Programming
principles. They are: Temporary Field, Refused Bequest, Switch Statement, and
Alternative Classes with Different Interfaces.

A Temporary Field is a field that is only present in certain specific cases and
not all the time; maybe they are used only by some methods. This usually hints
that there is an abstraction missing somewhere, maybe a class that should hold
these temporary values. The Null Object pattern might serve as the value to hold
when you don't need these temporary fields.

Refused Bequest is the violation of the Liskov Substitution Principle, a
subclass that cannot perform as its parent class.

The Switch Statement code smell appears whenever you have a big switch
(sometimes called case) statement, or an if-elsif-else statement with many elsif
branches. This is usually a violation of the Open-Closed Principle, and can be
solved by using Dependency Injection or lookup tables or hashes, the factory
pattern, the strategy pattern, or even inheritance.

Lastly, there are the Alternative Classes with Different Interfaces. This is
when you have two or more classes that have methods that do essentially the same
thing, or are comparable to one another, but they have different method names,
or different arguments order. Solve this by refactoring to make the two or more
interfaces the same; if you do this, why not also make it an abstract interface.

## Closing Thoughts

Even tho the catalog is extensive, these are not all of the signs of stinky
code, for each codebase and for each team you will find that there are other
things that can be considered inacceptable or unwanted.

For example, for me, being a functional programmer, I always consider the
excessive or even unnecessary use of state a code smell in itself.

Mind that most of these code smells were discovered and documented in the
context of Object-Oriented Programming. This doesn't mean in any case that these
are not present in other styles of programming, but it does mean that some of
these things are not bad signs in other paradigms, look at Data Classes for
example.

It's not the same thing saying "I don't like this code, it looks ugly", than
saying "I don't like this code, because it has this code smell" or "...because
it violates such principle". Knowing the names and precise definition of each
code smell enhances your ability to communicate properly about the potential
problems of any piece of code, and is a fundamental skill for designing and
implementing proper software.
