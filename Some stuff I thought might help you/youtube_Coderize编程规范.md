title: every principled coder should know
date: 2023-5-25
tags:

- something you should know

categories: 
- something you should know

---

- ```
  Welcome the 7 deadly sins of programming
  Hi there, there are what I would consider to be seven deadly sins when it comes to programming.
  Things that will grind your code quality into dust if you don't heed their lessons.
  Now I hope you've never had to hear someone tell you that you write bad code, but if you
  have there's really nothing to be embarrassed about.
  We all write flawed code as we learn, and the good news is it's fairly straightforward
  to make improvements if you're willing.
  In essence, that's why I've created this video to guide you to becoming a better coder.
  So let's look at the mistakes you might be making and see if we can fix them.
  You should pick and use a standard, always
  The first thing I see people doing is not using programming standards.
  They give us rules to follow, like whitespacing, file structure and other philosophies that
  we apply to make our code consistent and therefore easily readable.
  It's also especially important when working with other people, because it allows everyone
  to rely on shared expectations.
  If you don't keep standards, it's kind of like switching fonts all the time.
  We can read it, sure, but the subtle differences throw us off and slow us down a bit.
  As for choosing a standard, if you aren't given one by your team, then there's plenty
  to choose from in the wild.
  Principles are the lifeblood of programmers
  The second thing, programming design principles, are probably some of my favourite ways to
  improve.
  You can think of principles like a general guide into becoming a better programmer.
  They're the raw philosophies of code.
  Now there's a lot of principles out there, too many to cover in this video, but I will
  quickly show five very important ones which go under the acronym of SOLID.
  The S in SOLID stands for Single Responsibility, and it teaches us that we should aim to break
  our code down into modules of one responsibility each.
  So if we have a big class that is performing unrelated jobs, for example, we should split
  that up into separate classes to avoid violating the principle.
  It's more code, yeah, but we can now easily identify what the class is trying to do, test
  it more cleanly and we can reuse parts of it elsewhere without having to worry about
  irrelevant methods at the same time.
  This actually becomes more important as we progress.
  The oh-so-open-closed principle suggests that we design our modules to be able to add
  new functionality in the future without having to actually make changes to them.
  Instead, we should extend a module to add to it, be that wrapping it or something else,
  but we should never modify it directly.
  Once a module is in use, it's locked, and this now reduces the chances of any new additions
  breaking your code.
  Luckily, the Luminous Lyskov leaves us a lesson that loosely limits how we leverage our legacy
  code.
  Okay, that's enough of that.
  Lyskov wants to tell us that we should only extend modules when we're absolutely sure
  that it's still the same type at heart.
  For example, we can probably extend a hexagon into a six-pointed star, because that still
  makes sense as a six-sided shape, but we can't really do the same if we wanted a five or
  seven-pointed star, because that would no longer be compatible with what a hexagon represents.
  It just wouldn't fit cleanly anymore into parts of your code that expect hexagons, and
  so it will have failed the principle.
  If that's the case, it should extend something that fits its design or become its own type
  instead.
  Almost at the end now, and we have I for Interface Segregation.
  It says that our modules shouldn't need to know about functionality that they don't use.
  We need to split our modules into smaller abstractions, like interfaces, which we can
  then compose to form an exact set of functionality that the module requires.
  This becomes especially useful when testing, as it allows us to mock out only the functionality
  that each module needs.
  Which brings me to Big D, which stands for Dependency Inversion.
  This one is pretty simple to explain, because it says that instead of talking to other parts
  of your code directly, we should always communicate abstractly, typically via the interfaces we
  define.
  Dependency Inversion breaks down any direct relationships between our code, and isolates
  our modules completely from one another, meaning we can swap out parts as we need to.
  Because they communicate with interfaces now, they don't need to know what implementation
  they are getting, only that they take certain inputs and return a valid output.
  The cool thing about SOLID is that when we combine all these principles together, it
  ends up decoupling our code, giving us modules that are independent of each other and making
  our code more maintainable, scalable, reusable and testable.
  Patterns let us learn from our programmer ancestors
  Programming design patterns, not to be confused with principles from the last chapter, are
  also something I see underused a lot, when they really should have more of a place in
  your mind.
  Patterns give us some real solutions to our code problems, but they aren't fixed implementations,
  so they're still kind of open to interpretation.
  We use them to architect our software systems, matching the right shapes to fit the needs
  our software has.
  First up, we have creational patterns, which are there to help us make and control new
  object instances, such as the factory method pattern, which turns a bunch of requirements
  into different modules that follow the same interface, but aren't necessarily the same
  type.
  Then there's structural patterns, which are concerned with how we organise and manipulate
  our objects, such as the adapter pattern, to wrap a module and adapt its interface to
  one that another module needs.
  Finally, there's behavioural patterns, which focus on how code functions and how it handles
  communication with other parts of the code, such as using the observer pattern to publish
  and subscribe to a stream of messages in an event-based sort of architecture.
  Now there's a bunch of different design patterns under each category, some of them
  you might have used before.
  A lot of these patterns are used by frameworks and by professionals in huge corporations,
  and that kind of ties in with another pretty unique benefit.
  The benefit of creating a universal vocabulary of programming.
  Have you ever written some code that seemed great at the time, but later you had trouble
  Names are often badly... named?
  understanding it?
  Well, it often comes down to the way you name things.
  Take a look at this snippet.
  It's quite short, but it's hard to interpret what's going on without some thoughtful analysis.
  Let's fix this in steps.
  First up, we should avoid unnecessary encodings, such as type information, which make it harder
  to read code in a natural way.
  Next we have many abbreviations and acronyms that don't really explain well enough what
  they are.
  We should expand them to their full names to avoid any miscommunication.
  Something better, but it's not really clear what some of these variables hold.
  They're quite vague and ambiguous.
  We should aim to use names that more accurately represent the nuances of the code we're working with.
  We also want to replace magic values, such as the organ index or trigger word string,
  with named constants.
  By doing so, we clarify their significance, and we help to keep things in sync if they
  used elsewhere.
  Okay, we can now read this code and understand what it's doing without having to think
  too hard about it.
  However, we can still improve it further.
  If you have any compassion for your future self, or other unfortunate developers who
  might have to read your code, be descriptive with your names.
  Ideally, we want to find a balance between being clear enough to quickly understand what
  the code does, without being so verbose that it becomes an information overload.
  Remember that no matter how good you think your code is, if it's not easy to read,
  I would argue that it's not actually good code at all.
  Tests give us confidence
  Many people don't test their code, and that's understandable, I think.
  Writing tests can be very difficult when code is not properly architected.
  At the high level, we have end-to-end testing, which lets you test the system as if you were
  the end user.
  It's useful because literally any spaghetti code can be end-to-end tested, assuming you
  can simulate the inputs, as it never actually touches the code itself, only what it delivers
  to us.
  They can be tricky to set up, though, due to the need to always have a fully functional
  application running, but they are surprisingly valuable.
  At the lower level, we have unit tests to verify the operation of our modules in isolation,
  and integration tests to examine the interaction between those modules.
  These provide validation that our code is doing what we expect it to, rather than focusing
  more on behaviour, like end-to-end tests.
  If the thought of writing tests seems overwhelming right now, try applying the solid principles
  you learned earlier.
  You might be surprised at how much easier it becomes when your code is modular and decoupled.
  Time, the impossible enemy
  Number six is…
  Time.
  And how you manage it.
  Time estimation is a process I still fail in sometimes.
  A common rule of thumb is to double or even triple your initial time estimate for a task.
  It feels excessive, but it's impossible to predict the unknown problems we will encounter
  along the way, so we need to account for those.
  For example, this video took three times as long to make as I had predicted.
  I really should take my own advice sometimes.
  Remember that creation goes hand-in-hand with problems, so in the end, it's better to
  overestimate and deliver ahead of schedule than it is to miss a deadline.
  Speed vs. productivity, what's better?
  Alright, this one is a bit cheeky, as it's still kind of related to time, but I needed
  seven, not six points to make the Deadly Sins reference, so here we are.
  All I wanted to say here is that you should try not to rush.
  Things can feel great when we're blazing through a project, and there's definitely
  a place for that in prototypes.
  But remember that if this is going to be a long-term project, take your time and think
  things through from day one.
  We'll never get all our decisions right first time, but we can at least give ourselves
  a better foundation to work from, and hopefully avoid some of that nasty code debt later.
  Anyway, you're likely to finish in less time regardless if you're not constantly battling
  decisions you can't easily fix because of poor architecture.
  We want projects that get easier with time, not harder.
  Leveling up
  And that's it.
  Wow, you leveled up.
  Go forth and be the programmer you were always meant to be.
  I'd like to end with a quote from Martin Fowler.
  Any fool can write code that a computer can understand.
  Good programmers write code that humans can understand.
  Thank you so much for watching.
  It's been many weeks making this video, but it's all worth it to see that smile of yours
  ```



















































































