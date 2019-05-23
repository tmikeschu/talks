footer: @tmikeschu (36 slides)
build-lists: true
slidenumbers: true

# Where There's a `nil`,

# There's a (safer) Way

Detroit.rb
Mike Schutte
May 22, 2019

^ Hello and thank you for coming out tonight to the return of Detroit.rb! Special
thanks to Jake for leading the charge on bringing Detroit Rubyists back
together.

---

- :house_with_garden:

- :sunrise_over_mountains:

- :guitar: :microphone: :musical_keyboard:

- :runner: :bike:

- ![inline](https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/TED_three_letter_logo.svg/1200px-TED_three_letter_logo.svg.png)

^ My name is Mike Schutte. I live three miles east of here in West Village where
my partner Hannah and I look after our lovely 119 year old baby made of bricks,
wood, and a basement that takes in water. I wrote my first line of Ruby in the
summer of 2016 and have loved it ever since. I am retired at the moment, but
joining the tech team TED conferences as a front-end engineer in June.
Enough about me!

---

![](https://stemcenter.utexas.edu/sites/default/files/styles/utexas_hero_photo_image/public/TEALS%20Logo_news.jpg?itok=g2gu02Ei)

---

# Victory Conditions

- Know how/when to use `fetch` and `dig`

- Understand nothingness as a concept more generally

- Feel more suspect of `nil` entering your Ruby programs

---

![inline](https://media.giphy.com/media/U6Qzze3Mt90Ig/giphy.gif)

`nil` :eyes:

---

# Roadmap

- API of fetch and dig

- Maybe values (as a concept)

- Message Passing

- Type Coercion

- Review

---

#[fit] `fetch` and `dig`

- Array

- Hash

^ Tonight I'm going to talk about two methods near and dear to my heart: fetch and
dig. These are messages you can send to both hash and array instances, and they
bring me a ton of joy.

---

# Basic API

^ At a high level, you swap bracket accessor syntax for `fetch` and chained or
nested bracket accessor syntax for `dig`.
Here are some examples:

```rb
lost = [4, 8, 15, 16, [23, 42]]

lost[1] == lost.fetch(1)

lost[4][1] == lost.dig(4, 1)
```

---

```rb
user = {
  name: "John Locke",
  physical_attributes: {
    hair: :bald,
    height: :tall
  },
  bio: "looking for meaning",
}

user[:name] == user.fetch(:name)

user[:physical_attributes][:hair] == user.dig(:physical_attributes, :hair)
```

^ Here you can see an ominous numerical reference to the hit show Lost along with
a potentially controversial use of a trailing comma on John Locke's
`looking_for` attribute.

---

# Defaults for `fetch`

```rb
lost = [4, 8, 15, 16, [23, 42]]

# lost[10] || 0
(10 < lost.length ? lost[10] : 0) == lost.fetch(10, 0)

user = {
  name: "John Locke",
  physical_attributes: {
    hair: :bald,
    height: :tall
  },
  bio: "looking for meaning",
}

# user[:nickname] || "no nickname"
(user.has_key?(:nickname) ? user[:nickname] : "no nickname") == user.fetch(:nickname, "no nickname")
```

---

# Index and Key Errors for `fetch`

```rb
lost = [4, 8, 15, 16, [23, 42]]

lost.fetch(10)
# => IndexError (index 10 outside of array bounds: -5...5)

user = {
  name: "John Locke",
  physical_attributes: {
    hair: :bald,
    height: :tall
  },
  bio: "looking for meaning",
}

user.fetch(:height)
# => KeyError (key not found: :height)
```

^ Sometimes it is useful to intentionally raise an exception when an expected
key is not found. If `fetch` is not provided a default, a KeyError exception is
raised.

---

# What about `dig`'s defaults and errors?

- :cry:

^ If one of the keys in your dig chain returns a non-hash, runtime will blow
up.

---

```rb
user = {
  name: "John Locke",
  physical_attributes: {
    hair: :bald,
    height: :tall
  },
  bio: "looking for meaning",
}

user.fetch(:height)
# => KeyError (key not found: :height)

user.dig(:height)
# => nil

user.dig(:bio, :about)
# => TypeError: String does not have #dig method

user.dig(:physical_attributes, :eye_color) || "no eyes"
user.dig(:physical_attributes, :eye_color).to_s
```

---

# Performance

- `fetch`: negligible difference

  - https://keepthecodesimple.com/ruby-fetch/

- `dig`: 1.5x slower (still fast)

  - https://tiagoamaro.com.br/2016/08/27/ruby-2-3-dig/

---

# When is this useful?

- Rails controllers or services parsing `params`
- Getting the first item in an array
- Parsing config and option hashes

---

# So what?

![](https://media.giphy.com/media/xUPGcF7dv8YbW6WOli/giphy.gif)

^ This might sound like a trivial trade off, but I believe there is a wealth of
benefit in making the jump from square brackets to these instance methods.

^ My goal here is to convince you that `nil` as a concept should be treated with
diligence and intention. `fetch` and `dig` are two tools you can use right away
to start on the path of being "woke" about "nil".

^ In most cases, you can do better than a raw `nil` value. `nil` has no context.
It communicates nothing about what kind of nothing it represents. In other
words, what were we expecting when we got `nil`? A number? An array? A string?

^ To kick things off, I want to explain how I got on this track of being mindful
of `nil`.

---

# Maybe

```elm
find : (a -> Bool) -> List a -> Maybe a
find predicate xs =
    xs
    |> List.filter predicate
    |> List.head


ages : List Int
ages = [10, 15, 21, 22]

firstAdult =
  find (\age -> age >= 18) ages
-- => Just 21

firstBaby =
  find (\age -> age < 3) ages
-- => Nothing
```

^ Here is a snippet of some Elm code. Elm is a functional, statically typed,
compile-to-JavaScript language used for client-side work.

^ So what do we have here? Let's go through it line by line.

^ First we have a function signature that defines the name of the function and the
types of its inputs and output. Specifically, we have a function bound to the
name `find`. Its first parameter is a function that takes any type `a` and
returns a boolean (true or false). Its second parameter is a list of any one type
`a`. It returns a `Maybe a`. We'll get to that in a minute.

^ Next we have the function implementation itself. We have the function name
`find` followed by named parameters `predicate`, a function that returns true or
false, and `xs`, to represent a collection. In Elm, and in functional
programming in general, it is more common to have generic variable names like
`xs` because the goal is to keep functions generic so they work on multiple
types of data.

^ On the next line we have our `xs` value which is a list (or array) of `a`.

^ Next we have a strange symbol that looks like a flag. This is called the `pipe`
operator. It takes the value on the left (in this case above) and "pipes" it
into the function on the right. `List.filter` takes a predicate function and a
list of values and returns the values that return true in the predicate
function. We are partially applying `List.filter` with `predicate`, so it
evaluates to a function that is preloaded with the predicate and just awaiting
the list of values.

^ Next we pipe the resulting filtered list into `List.head`. In most functional
languages, Getting the head, or first element, of a list is not as trivial as it
is in languages like Ruby and JavaScript.

^ Because a list can be empty, `List.head` cannot guarantee that it will return a
real value. That is where this mysterious `Maybe` thing comes in. You can think
of `Maybe` as a class that has two child classes: `Just` and `Nothing`. In the
case of `Just`, it wraps a value up, kind of like a Promise in JavaScript. By
wrapping a value like this, we can communicate special instructions based on the
wrapper. It's like putting glasses into a box with a "FRAGILE" label. Without
needing to open the box, we know to handle the box, and therefore its contents,
with special care.

^ So in the event that the list is not empty, `List.head` returns `Just` the first
element. If the list _is_ empty, `List.head` returns nothing.

---

```elm
find : (a -> Bool) -> List a -> Maybe a
find predicate xs =
    xs -- List a
    |> List.filter predicate -- List a (elements that pass predicate)
    |> List.head -- Maybe a
```

---

```elm
ages : List Int
ages = [10, 15, 21, 22]

firstAdult =
  find (\age -> age >= 18) ages
-- => Just 21

firstBaby =
  find (\age -> age < 3) ages
-- => Nothing
```

^ This is different from returning the unwrapped value or `nil` like we would in
Ruby. By wrapping up a value, we have to take explicit, defined steps to
_unwrap_ the value when it comes time to do something with it (e.g., present it
on the DOM, update a value in a data store, etc). `Just` and `Nothing` have the
same interface, but different behavior. For example, to _unwrap_ a Maybe value,
there is a `withDefault` function. For a `Just` value, `Maybe.withDefault`
returns the wrapped value and ignores the default. For a `Nothing` value,
`Maybe.withDefault` returns the default value since it doesn't have a value of
its own.

^ So back to our Elm code, we define a list of integers representing various ages.
`firstAdult` is a `Maybe` value of the first age found that is greater than 18.
We use an anonymous function wrapped in parens as our predicate function and
then pass the list of ages. `firstBaby` is a `Maybe` value of the first age
found that is less than 3.

---

```elm
case firstBaby of
  Just age ->
    age

  Nothing ->
    0

-- OR

Maybe.withDefault 0 firstBaby
-- => 0

Maybe.withDefault 0 firstAdult
-- => 21
```

---

# Overcomplicated?

![inline](https://media.giphy.com/media/iHe7mA9M9SsyQ/giphy.gif)

^ You might be thinking, "Wow. Elm adds a lot of complexity to simple task." It
might seem like overboard. In my first Elm project, I was dealing with `Maybes`
all over the place. It was annoying at first, but I eventually realized how
rampant nothingness is as a concept. It's all over our applications. Every
single time you use bracket syntax, for example, to access an element of a hash
or array, for example, you are opening a door for `nil` to enter and blow up
your runtime. Because Elm forces you to explicitly handle each and every case
that a value could be nothing, it is able tout accolades such as zero runtime
errors. Period.

---

#[fit] Back to Ruby...

^ So after having to face these concerns of nothingness all the time, you can
imagine how it felt to hop back into Ruby.

^ I saw `nil` vulnerabilities everywhere!

^ I don't remember exactly how I learned about `fetch`, but it became my post-Elm
go-to for accessing elements in hashes and arrays. For all those cases where
I'm concerned about `nil`, I can pass `fetch` an optional second argument that
serves as the default.

---

![](https://media.giphy.com/media/51Uiuy5QBZNkoF3b2Z/giphy.gif)

---

```rb
user.fetch(:please_dont_blow_up, "🙏")
```

---

# Sending Messages

> I’m sorry that I long ago coined the term "objects" for this topic because it gets many people to focus on the lesser idea. The big idea is messaging.

-- Alan Kay

^ The foundation of object-oriented programming was not about classes and
inheritance, but about message passing and data encapsulation. Objects emit
messages and know how to respond to messages. Like cells in an organ
collaborating to achieve a greater collective function.

^ It's kind of strange that most of our programs use dot-method syntax for sending
messages to objects, but we have special cases where new syntax is introduced.

^ This oddity extends even further when we show that bracket syntax is in fact
still a good old message.

---

```rb
lost = [4, 8, 15, 16, [23, 42]]

lost[0] == lost.[](0) # => true

lost.[](0) == lost.send(:[], 0) # => true
```

^ Is there a good reason for this? Why do arrays and hashes need to access
elements using square brackets when most of their other methods are the more
common dot-method syntax?

^ The answer is probably related to familiarity bias for standards defined by other
languages. I argue that is not worthy of cluttering what could otherwise be
a more consistent API.

^ Using `fetch` and `dig` allow your programs to express a more uniform API by
sending messages.

^ In Sandi Metz's talk "Nothing is Something", she notes the relative scarcity of
reserved keywords in Smalltalk compare to Ruby. Granted, Ruby is probably a more
broadly representative case than Smalltalk, the point is worth relaying.

---

```rb
100[0]
# => ?
```

---

```rb
100[0]
# => 0
```

---

# Interact with `[]` and `{}` with a more consistent pattern.

- i.e., messages with explicit names

---

# Safe Navigation Operators

A test might belong to a classroom
A classroom might belong to a teacher
A teacher might belong to a school

```rb
test&.classroom&.teacher
```

^ As of Ruby 2.3.0, you may have seen the lonely (aka safe navigation)
operator `&.`. I used this when I first learned about it, but I'm less sanguine
with this tool for dealing with `nil` now.

^ If we think of nothingness in an application as a river, it's much more
effective to try to manipulate a river further upstream, closer to the source.
The lonely operator encourages one-off handling of nil values, probably on a
runtime-explosion by runtime-explosion basis.

^ It too introduces new syntax that can throw off the readability flow of the
program. 90% of your method calls have just a dot, but there exists a rebellious
10% that prepends the dot with an ampersand.

^ While `dig` and chained use of lonely operators are functionally equivalent,
`dig` introduces less novelty to your program by relying on more basic message
passing.

^ When I say `some_hash.dig(:a, :b, :c)`, I might not know how `dig` works, but I
know it is a public method for the Hash API. My strategies for understanding it
are similar to any other message. With the lonely operator, the semantics of the
syntax are more obscure and unnecessarily novel.

^ This also encourages us to break the Law of Demeter.

---

# Meaningful Nothingness

```rb
coercers = (NilClass.instance_methods - Object.methods).
  select { |method| method.to_s.match(/^to_/) }.
  concat([:to_s])
# => [:to_a, :to_f, :to_i, :to_h, :to_r, :to_c]

coercers.map { |coercer| nil.send(coercer) }
# => [[], 0.0, 0, {}, (0/1), (0+0i), ""]
```

---

```rb
# => [[], 0.0, 0, {}, (0/1), (0+0i), ""]

0.round => 0 # => 0

"".upcase # => ""

[].map { |x| x + x } # => []

{}.keys # => []

10 + 0 # => 10

"Harry Potter" + "" # => "Harry Potter"

[1, 2, 3] + [] # => [1, 2, 3]

{ a: 1 }.merge({}) # => { a: 1 }
```

---

# What about other classes?

^ I've been focusing on primitive data types or collection types included in the
standard library. What about your custom classes? It's still useful to use this
idea of meaningful nothingness. As Sandi Metz says, nothing is something, so
ideally, every custom class we introduce to our application should also model
nothingness.

^ This pattern is known as the null object pattern. Basically, for any object you
have, you have a "null" object that has the same public API as your "real"
object. Typically you wrap both of these object constructors (most often
classes) in one constructor than guarantees an object with a certain contract or
API.

^ Here is an example from Sandi Metz's talk:

```rb
class Animal
  def self.find(id)
    # fetch animal from from data store
  end

  # ...
end

class MissingAnimal
  def name
    "missing animal"
  end

  # ... any other public methods on the Animal class
end

class GuaranteedAnimal
  def self.find(id)
    Animal.find(id) || MissingAnimal.new
  end
end
```

---

# Review

- `fetch`: defaults, errors

- `dig`

- `Maybe`

- Message passing

- Type coercion
  - Null object pattern

---

# What You Can Do Tomorrow

- Use `fetch` with defaults

  - `params.fetch(:username, "")`

- Use `dig` with trailing type coercion

  - `params.dig(:user, :password).to_s`

---

#[fit] Thank you!

---

# Resources

- Nothing is Something by Sandi Metz: https://www.youtube.com/watch?v=29MAL8pJImQ
- Elm Maybe: https://package.elm-lang.org/packages/elm/core/latest/Maybe
- Hash#fetch: https://apidock.com/ruby/Hash/fetch
- Hash#dig: https://apidock.com/ruby/Hash/dig
- Array#fetch: https://apidock.com/ruby/Array/fetch
- Array#dig: https://apidock.com/ruby/Array/dig
