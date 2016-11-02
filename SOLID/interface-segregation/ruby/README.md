## Interface segregation principle

The interface-segregation principle (ISP) states that:
> No client should be forced to depend on methods it does not use.

Simple as that. Lets see some code examples and explain them.

```ruby
class Computer
  def turn_on
    # Turns on the computer.
  end

  def type
    # Type on the keyboard.
  end

  def change_hard_drive
    # Opens the computer body
    # and changes the hard drive.
  end
end

class Programmer
  def use_computer
    @computer.turn_on
    @computer.type
  end
end

class Technician
  def fix_computer
    @computer.change_hard_drive
  end
end
```

In this example, there are `Computer`, `Programmer` and `Technician` classes. Both, `Programmer` and
`Technician` use the `Computer` in a different way. The programmer uses the computer for typing, but
the technician knows how to change the computer hard drive. What Interface Segregation Principle (ISP)
enforces is that one class should not depend on methods it does not use.

In our case, `Programmer` is unnecessarily coupled to the `Computer#change_hard_drive` method because
it does not use it, but the state changes that this method enforces can affect the `Programmer`. Let's
refactor the code to obey the LSP.

```ruby
class Computer
  def turn_on
  end

  def type
  end
end

class ComputerInternals
  def change_hard_drive
  end
end

class Programmer
  def use_computer
    @computer.turn_on
    @computer.type
  end
end

class Technician
  def fix_computer
    @computer_internals.change_hard_drive
  end
end
```

After this refactor the `Technician` uses a different object from the type `ComputerInternals` which is
isolated from the state of the `Computer`. The state of the `Computer` object can be influenced by the
`Programmer` but the changes wont affect the `Technician` in any way.
