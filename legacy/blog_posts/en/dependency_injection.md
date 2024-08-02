# Dependency Injection

<!--- What it is --->
Dependency Injection is a software development design and implementation
technique.


<!--- What's it not --->
- It is not a **software design principle**
- It is not the same as **Dependency Inversion**


<!--- What is a dependency --->
A dependency is anything that a piece of code knows invariably.
Some dependency examples are:
- Literal values
- Constants
- Global State
- Other classes
- Other modules
- Other methods


<!--- Types of Dependency Injection --->
The way in which **Dependency Injection** can be used will vary depending on the
programming language being used.

## Parameter Injection

If there's a method that has a dependency, you can always inject it via the
parameters.

Let's say you have some code like this:
```ruby
# Definition
def some_method(value)
  SomeDependency.do_something(value)
end

# Usage
value = 10
some_method(value)
```

You can use Dependency Injection to make it more flexible. Notice how the usage
of default values can be a nice middleground that will keep the existing
behavior while still getting the benefits of DI.

```ruby
# Definition
def some_method(value, dependency: SomeDependency)
  dependency.do_something(value)
end

# Usage with default dependency
value = 10
some_method(value)

# Usage with injection
value = 10
some_method(value, dependency: OtherDependency)
```

Notice however, that the method still knows something invariably, which is the
name of the method to call unto the dependency that's being injected. We could
as well inject the method itself.

```ruby
# Definition
def some_method(value, method: SomeDependency.method(:do_something))
  method.call(value)
end

# Usage with default dependency
value = 10
some_method(value)

# Usage with injection
value = 10
method = ->(value) { value > 10 }
some_method(value, method: method)
```


## Constructor Injection

Before:
```ruby
class Klass
  def initialize(value)
    @value = value
  end

  def some_method
    SomeDependency.do_something(@value)
  end
end
```

After:
```ruby
class Klass
  def initialize(value, dependency: SomeDependency)
    @value = value
    @dependency = dependency
  end

  def some_method
    @dependency.do_something(@value)
  end
end
```

Just as the example above, you could be injecting either the constant or the
method itself.

## Setter Injection

Setter Injection can be used whenever we want a dependency to be able to change
during the lifetime of an object.

```ruby
class Klass
  def dependency=(dependency)
    @dependency = dependency
  end

  def some_method(value)
    @dependency.do_something(value)
  end
end

# Usage
klass = Klass.new(SomeDependency)
klass.do_something(10)
klass.dependency = OtherDependency
klass.do_something(10)
```

## Configuration Injection

You can use Configuration Injection whenever you want to have DI without moving
the responsibility of choosing the appropriate dependency to the consumer of
the code, but rather as a configuration of the application itself.

```ruby
module Config
  def self.logger
    @logger ||= YAML.load(File.read(‘config.yaml’)[:logger] || Logger
  end
end

class Something
  LOGGER = Config.logger

  def method
    LOGGER.info "User logged in"
  end
end
```

Notice that you can have a static configuration like the one above, which would
happen only one time (at first call, or even better at initialization), but you
could also have a more dynamic approach where you look up the configuration
every time.

```ruby
def method
  Config.logger.info "User logged in"
end
```

## Factory Injection
```ruby
class SomeClass
  def initialize(value)
    @value = value
  end
end

def build_class_with_dependencies(one_dependency, two_dependency)
  Class.new(SomeClass) do
    One = one_dependency
    Two = two_dependency

    def the_method
      other_value = One.fetch_value
      Two.do_something(value)
    end
  end
end

klass = build_class_with_dependencies(SomeClass, OtherClass)
other_klass = build_class_with_dependencies(ThirdClass, FourthClass)
klass.new(10).the_method
other_klass.new(10).the_method
```
## Generic Injection
