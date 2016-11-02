## Liskov substitution principle

This principle applies only to inheritance, so let's look at an example of inheritance that breaks it:

```ruby
class Animal
  def walk
    do_some_walkin
  end
end

class Cat < Animal
  def run
    run_like_a_cat
  end
end
```

In order to comply with the LSP, as Bob Martin puts it:
> Subtypes must be substitutable for their base types.

So, they must have the same interface. Since Ruby does not have abstract methods, we can do it like so:

```ruby
class Animal
  def walk
    do_some_walkin
  end

  def run
    raise NotImplementedError
  end
end

class Cat < Animal
  def run
    run_like_a_cat
  end
end
```
