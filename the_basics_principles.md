# The Basics, Part 1: Development and Design Principles

In this Part 1 of **The Basics** series, I will list and briefly explain what I
consider to be the most important software development and design principles.

If you don't know, and more importantly, consistently apply these principles,
you might not be a Senior developer just yet, but don't worry, with a little bit
of study you can understand them, and with some focused practice, master them.

This post is not meant to be an in-depth explanation of each principle, but
rather a refresher that allows you to reflect on whether you know and remember
these things properly.

## DRY

The **DRY** principle is probably the first principle that most
developers ever learn. DRY stands for **Don't Repeat Yourself**, which is kind
of a self-explanatory name.

Whenever you have two identical pieces of code or two pieces of code that
differ only in values or variables, extract that piece of code, usually into a
method, and call that new method wherever you had the repeated piece of code in
the first place.

Here's an example of how this principle is applied; the example might be too
simple, but don't get distracted, the idea is that you learn to apply the
principles from the most basic cases and build up on that.
```ruby
# Before applying DRY
width = 5
height = 10
area = width * height
puts "The area of a rectangle of size 5*10 is #{area}"

width = 3
height = 7
area = width * height
puts "The area of a rectangle of size 3*7 is #{area}"

# After applying DRY
def puts_rectangle_area(width, height)
  area = width * height
  puts "The area of a rectangle of size #{width}*#{height} is #{area}"
end

puts_rectangle_area(5, 10)
puts_rectangle_area(3, 7)
```

Now in real life, the repeated pieces of code will most probably not be written
side by side, but rather in different parts of a file or maybe even on separate
files. 

The art of applying the **DRY** principle is being able to identify these
repeating patterns and abstracting them into common functions.

Sometimes tho, the repeated pattern might be a whole class or module, and not
just a few simple lines of code. Be on the look!

## YAGNI

The **YAGNI** principle is about not writing code, so no code example here!

**YAGNI** stands for **You are not gonna need it**, and this principle tells you
to never write code for something you think will happen in the future or code
that you don't immediately need. 

You might think that you know what the future will bring, but you don't, don't
try to be clever by making code more flexible than it needs to be immediately,
if you need to change it in the future, you can do that when the time comes.

The corollary for this is that you **should** write your code in a way that is
easy to change, once that change is necessary, and there are other principles
and techniques concerned with that, like the Open-Closed principle, or the
Dependency Injection technique, which I will present now.

## CQS

The **CQS** principle is often overlooked, but it is one of the principles that
can make a huge difference in how you code.

It stands for **Command/Query Separation**, which means that your methods should
either be Commands, or Queries, but never both at the same time.

A Query is a method that produces a return value. In some languages there are
`void` methods, that never return anything, and then in other languages where
every method has to return something some methods that always return
`nil`, `null`, `None`, or `()`; notice the word **always**.

Again, any method that returns something (even if it's sometimes something and
sometimes `null`) is a Query.

Commands on the other hand can be of two types. A Command can be either a method
that changes the state of the program, for example, it changes the value of a
variable inside an object; or it can be a method that performs I/O, meaning that
it interacts with some other system like the screen, the keyboard, or other
programs, a database, an internet connection, et c.

There are very few cases in which a Query-Command is better than having them
separated, the typical one being popping an object from a list, which will
modify the list and return the popped object.

The benefit of applying this principle is that by separating Queries from
Commands, each method becomes easier to reason about, and also to test, since
Queries and Commands are tested differently.

Queries are way easier to test and reason about than Commands, so you should
strive to write your code using the least amount of Commands possible, and write
most of your code using solely Queries.

## Law of Demeter

The **Law of Demeter** is actually a principle, rather than a law, and it states
that an object (or any other piece of code for that matter) should never know
the interface of the dependencies of its dependencies.

Another way to state it is that it's commonly thrown around, tho quite
imprecise, is that you should never have "two dots" in a line of code, meaning
you should avoid things like `post.comment.length` for example.

I don't like this last way of phrasing it because it doesn't take into account
certain subtleties. For instance, primitives are usually exempt from the Law of
Demeter (but then again you should seldom be working with primitives directly)
and there are some methods, like the ones found in iterators or enumerables
(`map`, `filter`, `reduce`, `flatten`) that you should also not consider for
this principle.

Let me illustrate it better with an example:
```ruby
AgeRestriction = Struct.new(:name, :minimum_age)
Genre = Struct.new(:name, :age_restriction)
Movie = Struct.new(:name, :genre)

age_restriction = AgeRestriction.new(:mature, 14)
genre = Genre.new(:horror, age_restriction)
movie = Movie.new('The Witch', genre)

# Before applying the Law of Demeter, in some other place
# The owner of movie knows the interface of Movie, and the interface of Genre
def get_movie_age_restriction(movie)
  movie.genre.age_restriction
end

# After applying the law of demeter
class Movie
  def age_restriction_for_genre
    @genre.age_restriction
  end
end
# The owner of movie only now knows only about the interface of Movie
def get_movie_age_restriction(movie)
  movie.age_restriction_for_genre
end
```

## Dependency Injection

Dependency Injection is not a principle but rather a technique, but it's so
fundamental to correctly applying other principles that I need to discuss it.

Dependency Injection, not to be confused with Dependency Inversion, is a
technique in which you take a constant dependency, and you make it
injectable. This can be done in several different ways, such as making something
a method, injecting a dependency thru a constructor, changing a dependency thru
a setter, or making a dependency configurable thru some other dependency.

The easiest version of it is using methods, so that's the one I will showcase
here:

```ruby
# Before applying DI
def update_post(id, content)
  # Post is our dependency
  Post.find(id).update(content:)
end

# After applying DI
def update_post(id, content, repository)
  repository.find(id).update(content:)
end
```

By doing this we get two main benefits, and then probably some more.

First, we no longer depend on `Post`, making our code less coupled to other
pieces of code, which in turn makes the code easier to test.

Secondly, the code is more flexible, because now we can not only use `Post` but
rather use other things, like a narrowed scope of posts, like `Post.active`, or
even other models that have the same interface (in which case it would be a good
idea to rename the method) like `Comment` which might also have a `content`
field.

## SOLID

**SOLID** is not one principle, but five of them, and I will present them
separately. They are usually bundled up because they are all principles of
object-oriented programming, but they are each all its own thing, and looking at
them separately will surely make them each easier to understand.

## Single Responsibility Principle - The S in SOLID

This principle states that every piece of code should have exactly one
responsibility, or one reason to ever change.

This applies differently depending on what type of piece of code we are talking
about. For example, a good way to apply this at a method level is by following
the **CQS** principle.

On the other hand, in a module or class, the responsibility is tied to which
part of the domain is being modeled with it. A typical example is that a class
that represents a domain object should not also hold a connection to the
database. `ActiveRecord` is a famous example of code that does not follow this
principle.

## Open-Closed Principle - The O in SOLID

The **Open-Closed** principle states that a piece of code should be Open for
extension but Closed for modification.

This means that you should write your code in a way that makes it possible to
change its behavior without changing the code itself. What? Yeah, you heard it
right.

There are multiple ways of achieving this, but the main two ones (AFAIK) are
Dependency Injection and using external lookup tables.

This example is provided using Ruby-style pseudocode, not literal code
```ruby
# Before applying Open-Closed principle
def some_method(object)
  case object
  when match_a then method_a(object)
  when match_b then method_b(object)
  when match_c then method_c(object)
  end
end

# Applying Open-Closed with a lookup table
LOOKUP = { a: method_a, b: method_b, c: method_c }

def some_method(object)
  LOOKUP[object].call(object)
end

# Applying Open-Closed with Dependency Injection
def some_method(object, dependency)
  dependency.call(object)
end

some_method(object, method_a)
some_method(object, method_b)
```

In the last two cases, you can change the behavior of the code without changing
the code; in the first case you change it by extending the lookup table and not
the method, and on the second one you change it by providing a separate
dependency: The code is open for extension but closed for modification, you
don't need to modify it in order to change its behavior.

## Liskov Substitution Principle - The L in SOLID

This is such an intimidating concept when you only look at its name; but it's so
easy to understand when you don't.

Whenever you are doing inheritance, or implementing interfaces, a subtype should
always be able to do everything that a supertype does; you should never remove
behavior when subtyping.

Put another way, if you have an `Animal` and a `Cat < Animal`, a `Cat` should be
able to do absolutely everything that an `Animal` does.

This also means that all possible return values and error types have to be
present in the supertype's methods declarations; subtypes should never expand
return types or raise errors not declared in the parent class' interface
contract.

This principle is less evident and harder to reason about and apply in
dynamically typed languages, but it's just as, if not more, important to follow.

This is tangentially related to the following principle, the Interface
Segregation Principle.

## Interface Segregation Principle - The I in SOLID

This principle applies mostly, but not necessarily exclusively, to strictly
typed languages. In dynamically typed languages it applies in any part where you
are doing runtime type checks.

This principle states that you should never depend on an interface if you are
not using all of its methods; to achieve that you should separate, or segregate,
interfaces in a way that makes this possible.

For instance, let's say you have a class `Dog` than can both `#bark` and
`#walk`. If you are only going to make it `#bark` but not `#walk` you should
separate these two methods in different interfaces `Barks` and `Walks` so you
can depend on `Barks` instead of `Dog`. This makes it so you can use that piece
of code with other animals that might `#bark` but not necessarily `#walk`.

If you keep the interfaces mixed together, whenever you want to make something
bark you will be restricted to something that can also walk, even if you don't
need it to walk.

Do not keep methods that will not be used together in the same interface.

## Dependency Inversion Principle - The D in SOLID

The **Dependency Inversion** principle, not to be confused with the Dependency
Injection technique, states that instead of depending on concrete things, a
piece of code that depends on another one should declare an interface and have
the dependency implement that interface.

It might be easier to understand with an example. Let's go back to the movie
example. Let's say that a `Movie` only needs a `Genre` to be able to know the
`AgeRestriction`. Instead of having `Movie` depend directly on `Genre`, you will
create an interface (that is conceptually, or concretely, living "near" `Movie`)
that has whatever method it needs from the concrete dependency.

```ruby
# Before applying Dependency Inversion
AgeRestriction = Struct.new(:name, :minimum_age)
Genre = Struct.new(:name, :age_restriction)
Movie = Struct.new(:name, :genre)

# After applying Dependency Inversion
module AgeRestrictionCategory
  def age_restriction -> AgeRestriction
  end
end

Genre = Struct.new(:name, :age_restriction) do
  include AgeRestrictionCategory

  def minimum_age
    age_restriction.minimum_age
  end
end

Movie = Struct.new(:name, :age_restriction_category) do
  def minimum_age
    age_restriction_category.minimum_age
  end
end
```

You might be tipped off by the fact that you have way more code after applying
the Dependency Inversion principle, and you do; the benefit is that now `Movie`
knows nothing about `Genre`, but instead `Genre` implements whatever interface
it needs to implement to be used as a dependency for `Movie`, and you have the
option of implementing this interface in however many classes you want; the age
restriction can now come from a whole set of objects instead of just from the
`Genre`.

Just as the Interface Segregation principle, the need for this is way more
evident in strictly typed languages, and might feel superfluous in dynamically
typed ones where if something quacks like a `Duck`, it is a `Duck`.

IMHO with time you eventually learn that dynamic typing is not a good thing, and
you start looking for those guarantees provided by types in one way or another,
and then, following these principles starts to make much more sense, as you add
runtime checks for types and interfaces.

## Closing Thoughts

I'm not saying that you should apply all these principles in every single piece
of code that you write; that might even be impossible, or impractical.

I think you should try to apply most of these principles most of the time, and
that the more you apply them, the better your code will be. With time and 
practice you will learn the cases where following any of these principles has
more downsides than benefits, or when the benefits are not worth the cost.

If you are used to separating your code into small pieces, a lot of these
principles will be applied without you even noticing; others not so much, so you
will need to apply them more consciously.
