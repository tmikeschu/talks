footer: /30 <br /> **@tmikeschu**
build-lists: true
slidenumbers: true

# Stop Testing.

# [fit] Start Storytelling.

Mike Schutte
RailsConf 2018

^ Webpacker thank you
^ Today I'm going to show you very little code, and tell you quite a lot about how to write it.

---

| Time | Topic             |
| ---- | ----------------- |
| 5    | About Me          |
| 1    | Goals             |
| 4    | Testing Paradigms |
| 20   | Storytelling      |
| 8    | Demo              |
| 2    | Wrap-up           |

---

```rb
[5,1,4,20,8,2].reduce(0, &:+) # => 40
```

---

```rb
[5,1,4,20,8,2].reduce(0, &:+) # => 40
[5,1,4,20,8,2].sum # => 40
```

---

# About Me

Detroit

Quikly (Rails, React, Node.js, Go)

environmental science -> sociology -> software development
University of Denver -> Indiana University -> Turing -> Quikly

runner, musician, reader, writer

^ I just want you to know that I'm going to spend a bit longer on my bio than most talks. I promise I'm not wasting your time and just blabbering about myself.

^ I live in Detroit's West Village. I just bought my first home, a beautiful 1900 Victorian.

^ I work for a digital marketing start up called Quikly. We get to solve some interesting technical challenges using Rails, React, Node.js, and Go for our high traffic endpoints.

^ I studied environmental science and sociology in undergrad at the University of Denver. After college I served in a 5th grade classroom as an City Year AmeriCorps member for a year in Denver. I then tried my hand at a PhD in sociology at Indiana University (the first time I'd ever lived east of Colorado). I decided a masters in sociology was enough for me, so I moved back to Denver. Just as I was about to get a job at a coffee shop and pursue music, my partner Hannah brought me along to an info session for Turing School in Denver. She was interested in learning some coding basics on nights and weekends. I was just tagging along for moral support. We quickly found out that Turing asked for seven months of 70 to 80 hour weeks. Full-time working Hannah was out, but I was unexpectedly in. One thing led to another and all the sudden I cared about things like skinny controllers and lining up my equals signs.

^ I also love spending time outside running unreasonably long distances, I play guitar and piano and write music, and I love reading fiction and writing poetry.

^ So now you have a lot of context about who I am and how I got here. We're going to come back to this idea of context.

---

# Goals

---

# Goals

# [fit] perspective

^ My goals are A) for you all to leave thinking about testing from a different perspective than you're used to

---

# Goals

# [fit] perspective

# [fit] confidence

^ and B) for all of you to leave this talk with either newfound or reinforced confidence in your ability to write meaningful tests.

---

# Testing Paradigms

bonus word: _para-dig-MAT-ic (adj)_

* a framework containing the basic assumptions, ways of thinking, and methodolog[ies] \[for discovering new ideas\] that are commonly accepted by members of a ~~scientific~~ community.[^1]

[^1]: http://www.dictionary.com/browse/paradigm

---

## Test last

* The thing that's supposed to happen is the the thing that happens.

* ~> It's blue because it's blue 🤔.

^ Test last is what it sounds like. Write your code, then write some tests that verifies it works. It's a bit deceitful because you're writing tests to fit the code instead of the other way around. The process might include debugging your test to figure out a return value or a side effect, then you make your assertion based on that. The problem is: what if what you found was wrong? You're accepting the existing implementation as the right way, which isn't always true. The code could be riddled with hidden bugs, and if you write a passing test, you have false assurance and you now have a flawed implementation that is backed by a passing test. It does make sense for certain types of tests. Have you heard of snapshots in the context of React/Jest/Enzyme?

---

## Test last

![inline](https://media.giphy.com/media/blPpTGDhn6hEI/giphy.gif)

---

![inline](http://www.barrygordon.co.uk/rediscovering-test-driven-development/img/red-green-refactor.png)

---

# Test first

* Writing a test to fit an implementation

^ Test first is when you write a test with an implementation already in mind. You're kind of writing it just to, I don't know, feel like a good Rails developer. This can be totally fine, but it falls short of the philosophical benefits of the next two types: test-driven and behavior driven. This can also lead to writing rigid tests, which are more harmful than helpful.

---

# Test driven

# Behavior driven

* Implementing code to fit a test

* _"Program to an interface, not an implementation"_
  > \- GOF

^ These two are very similar. Test driven is all about writing a test with only your testing hat on. Your implementation hat is hung on the wall during this period. Once your test is written, you do your red-green-refactor. Behavior driven is a similar process with a different state of mind. The tests you write describe the behavior or the interface of your software, not the implementation. A classic bit of Gang of Four wisdom is to program to an interface, not an implementation. I would say that the difference between TDD and BDD is essentially that: TDD writes test to an implementation, BDD to an interface and is therefore more flexible.

---

# Storytelling and Software

> _Software is **always** means to an end_

* Function to Feature
* (HINT: the end is to help users.)

^ So I've told you to stop testing and start storytelling.

^ Everyone here, regardless of experience or skill level in Rails land has an interest in software -- right? I am one of those people who loves the process of coding itself, but I have to be careful to never forget that software is ALWAYS a means to an end. Those ends are defined by our end users. They have a problem, and we write code to solve those problems. I remind you of this because when we get in the slog of cranking out features and converting requirements to working software, we can get lost in the haze of implementation and develop ignorance about flaws in the design of the codebase and end product. It's important to stay close to our inner user because it helps us write better software.

---

# [fit] Storytelling and Software

(minutes)

* communication (1)

* context (3)

* abstraction (1)

* encapsulation (3)

---

# Communication

<br/>
## Users communicate with software.
## Let's make good listeners.

^ Stories are all about communication. When I communicate, my goal is to get an idea from inside of my head to inside of your head and get us to agree on the meaning of that idea. Shared meaning gives us a sense of safety and comfort. CANCELLED FLIGHT SCENARIO. Good software, then, is essentially good communication. Users develop ideas of what should happen when they take certain actions. If expectations meet reality, we have successful communication between the user and the software.
I'm always so excited to sit down and write a test because it's my chance to think about what I ultimately want the software to do, and it's my responsibility to communicate that to the test suite from the perspective of a user.

---

# Context

<br/>

* `:cause && :effect`
* More context = more understanding

* More understanding = more predictive power

* More predictive power = more confident decision-making

* More confident decision-making = more happiness!

^ Storytelling is fantastic because it forces you to think contextually, not in isolation. When I told you extensively about my education background and I how I ended up in software development, each piece of detail helped you place me into a broader context. Some of you maybe took a mental note of my being from Denver, other people maybe feel sorry for all the work and surprises I'm going to find with my 118 year old house. With more context comes more understanding. Web applications are ALL. ABOUT. CAUSE. AND. EFFECT. User does this, do that. Many users do X, do Y. The more time we spend thinking about our functions and features in a broader context, we will write better software.

---

# Context

<br />

## When we tell stories about our software, we write better software.

---

# Abstraction

* Touching a stove
* Breathing
* Muscle memory
* Literally every language

^ Storytelling and testing are aided by abstraction. Guess what? You are REALLY good at abstraction. I guarantee it. Since you're good at abstraction, you already have the capacity to write great stories and tests.

^ Abstraction empowers us to perform more complex tasks with ease.

^ We also tend to be best at things that we do repeatedly. Muscle memory is a great example of abstraction. We have implemented a series of movements over and over again to the point of creating an abstract interface that is much easier to work with than working with each individual sub-unit of movement that makes of the overall abstraction.

---

# Encapsulation

## _We encapsulate our code, why not our thoughts?_

* Too many jobs makes you bad at all of them.

^ Just as collaborating objects don't need to know about each other's implementation details, we don't need to know about implementation details when writing tests.

---

# Encapsulation

<br />

## Code constrains clarity and imagination.

* Tests can be your compass home

^ Once we start coding, clarity of thought and imagination decline. When we are just thinking about what our programs should do, we don't get held up by syntax or the order of function arguments. If I want to convert a string to a hash of words pointing to their count in the string, I don't need to know how to use reduce off the top of my head in whatever language I'm in. I just need to write a test that specifies how I want to transform an input value, then I can implement a solution bit by bit with the test serving as a compass to keep me moving in the right direction. Most of the problems we solve are not impossible to implement, so I encourage you to have faith that if you document a clear goal via a test, you WILL find a solution.

---

# Encapsulation

**Meta alert!**
<br/>

* Programs solve problems

* Programming is a problem-solving process

* Maybe the principles that make better _programs_ can also result in better _programming_?

---

<br />
<br />
<br />
<br />
## User <---------------------> Programmer

^ We can never forget that as programmers, we are also users. We contain multitudes, as Uncle Walt Whitman might say. Because of this duality, we have to be mindful of which hats we are wearing and when.

---

# Implementing

<br />
<br />
## User <-------------------X> Programmer

^ When we are implementing, we are not thinking as much about whether or not the software as a whole makes sense. We are just trying to write code that doesn't break and lines up well enough with requirements.

---

# Storytelling/testing

<br />
<br />
## User <X-------------------> Programmer

^ When we are writing tests and describing what our application does, we are more like a user. We are not concerned with enumerable methods under the hood or whether or not we are using ActiveRecord or raw SQL. When you are writing tests, turn off your programmer as much as possible.

---

# Crowdsource Demo!

![inline](https://media.giphy.com/media/phJ6eMRFYI6CQ/giphy.gif)

* What's something you have to implement in the near future? Could be anything from function to feature.

^ Once our story is told and our test is written, the test suite holds us accountable to making that story become, and stay, true. The high level goal is documented, and our implementation is guided by this focus.

---

Wrap-up

* Testing is a _paradigmatic_ process, not a product.
* Communication: build good listeners
* Context: enable confident decision making
* Abstraction: you're good at it, it let's you do cool stuff.
* Encapsulation: apply the same discipline to your process as you do to your code.
* Storytelling keeps you closer to users

---

## Action Plan

![inline](https://media.giphy.com/media/13UazF4VlXZgqc/giphy.gif)

* Develop a pendulum workflow between storyteller and developer.

^ Obviously we still have to write some code to get anything working. It's also 100% okay and expected that it will be super concrete and rigid at times. Just remember that when you are in your \_spec or \_test file, put on your storyteller hat.

---

# [fit] Thank you!
