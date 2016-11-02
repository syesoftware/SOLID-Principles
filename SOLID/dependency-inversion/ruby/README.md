## Dependency inversion principle

Lets see the example:

```ruby
class Report
  def initialize
    @body = 'whatever'
  end

  def print
    XmlFormatter.new.generate(@body)
  end
end

class XmlFormatter
  def generate(body)
    # Convert the body argument into XML.
  end
end
```

The `Report` class is used to generate an XML report. In it's initializer we setup the report and its body.
The `print` method uses the `XmlFormatter` class to convert the body of the report to XML. Easy as that.

Let's think a bit about this class. Look at it's name - `Report`. It's a generic name and it tells us that
it will return a report of some kind, but, it doesnt say much about it's format. In fact, in our example,
we can easily rename our class to `XmlReport` since we know the implementation details. But insead of making
it very specific, let's think about abstracting this code.

Right now, our class is dependant on the `XmlFormatter` class and it's interface i.e. `generate`. Report right
now is dependent on a detail, not on abstraction. It knows that there must be a class `XmlFormatter` so it can
work. Also, another question - what would happen if we wanted an CSV report? Or a JSON report? We'd have to
have multiple methods like `print_xml`, `print_csv` or `print_json`. This means that our class right now is
very tied to the details, it knows about the formatter class type instead of knowing just how to use it
(abstraction).

Let's refactor it:

```ruby
class Report
  def initialize
    @body = 'whatever'
  end

  def print(formatter)
    formatter.generate(@body)
  end
end

class XmlFormatter
  def generate(body)
    # Convert the body argument into XML.
  end
end
```

Look at the `print` method now. It knows that it needs a formatter, but it only cares about it's interface.
To be more specific, it only cares that that formatter has a method called `generate`. How is this better?
Well, if we wanted CSV reports, all we would need is to add the following class:

```ruby
class CSVFormatter
  def generate(body)
    # Convert the body argument into CSV.
  end
end
```

The `Report#print` method would accept a `CSVFormatter` object as a parameter which would convert the
report body into a CSV string.
