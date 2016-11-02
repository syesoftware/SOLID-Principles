## Single Responsibility principle

Probably the most well known principle, and one that you should try to adhere to most of the time.

Let's say you have this code:

```ruby
class AuthenticatesUser
  def authenticate(email, password)
    if matches?(email, password)
      do_some_authentication
    else
      raise NotAllowedError
    end
  end

  private

  def matches?(email, password)
    user = find_from_db(:user, email)
    user.encrypted_password == encrypt(password)
  end
end
```

The `AuthenticatesUser` class is responsible for authenticating the user as well as knowing if the
email and password match the ones in the database. It has two responsibilities, and according to
the principle it should only have one. Let's extract one:

```ruby
class AuthenticatesUser
  def authenticate(email, password)
    if MatchesPasswords.new(email, password).matches?
      do_some_authentication
    else
      raise NotAllowedError
    end
  end
end

class MatchesPasswords
  def initialize(email, password)
    @email = email
    @password = password
  end

  def matches?
    user = find_from_db(:user, @email)
    user.encrypted_password == encrypt(@password)
  end
end
```

I've used a refactoring technique called Extract Class and then used it on the original class I
already had. This is called sharing behaviour through composition.
