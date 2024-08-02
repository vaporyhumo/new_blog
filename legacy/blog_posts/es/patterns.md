# Design Patterns


## Value Object

A Value Object is an object that is immutable, and compared by value and not
identity.

To achieve immutability we just need to call `freeze` at the end of
`initialize`.

To compare by value we need to implement `#==` and `#eql?`.

```ruby
class Length
  def initialize(value, unit)
    @value = value
    @unit = unit
    freeze
  end

  def eql?(other)
    self.class == other.class &&
      @value == other.value &&
      @unit == other.unit
  end
  alias :== :eql?
end
```


## Service Object

A service object is a design pattern where we create an ephemerous object to
perform one action and then we discard it.

They way we achieve this is by providing a public class method that will create
the object and use it to perform an action, returning the result of said action
that ought not to be the service itself.

```ruby
  class AddTwoNumbers
    private_class_method(:new)

    def self.call(a, b)
      new(a, b).__send__(:perform)
    end

    private

    def initialize(a, b)
      @a = a
      @b = b
    end

    def perform
      @a + @b
    end
  end
```

If you'd like to extract the pattern into a module it can be done like so:

```ruby
module Service
  def self.extended(klass)
    klass.private_class_method(:new)
  end

  def call(...)
    __send__(:new, ...).__send__(:perform)
  end
end

class AddTwoNumbers
  extend Service

  private

  def initialize(a, b)
    @a = a
    @b = b
  end

  def perform
    @a + @b
  end
end
```


## Decorator

A decorator is an object that wraps around other object to extend its functionality,
while still providing access to the original objects
