## Open/Closed principle

The Open/Closed Principle states that classes or methods should be open for extension, but closed for
modification. This tells us we should strive for modular designs that make it possible for us to change
the behavior of the system without making modifications to the classes themselves. This is generally
achieved through the use of patterns such as the strategy pattern. Let’s look at an example of some
code that is violating the Open/Closed Principle:

```ruby
class FileParser
  def initialize(format)
    @format = format
  end

  def parse
    case @format
      when :xml
        parse_xml
      when :csv
        parse_csv
    end
  end

  private

  def parse_xml
    # Parse xml.
  end

  def parse_csv
    # Parse csv.
  end
end
```

In the above example we can see that we’ll have to modify our file parser anytime we add a client that
reports usage information to us in a different file format. This violates the Open/Closed Principle.
Let’s take a look at how we might modify this code to make it open to extension:

```ruby
class FileParser
  def initialize(parser)
    @parser = parser
  end

  def parse(file)
    parser.parse(file)
    # Common parse actions.
  end
end

class XmlParser
  def parse(file)
    # Parse xml.
  end
end

class CsvParser
  def parse(file)
    # Parse csv.
  end
end
```

With this refactor we’ve made it possible to add new parsers without changing any code. Any additional
behavior will only require the addition of a new handler. This makes our FileParser reusable and
in many cases will keep us in compliance with the Single Responsibility Principle as well by encouraging
us to create smaller more focused classes.
