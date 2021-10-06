# Confident Ruby

## Performing Work

Summary: We should start by thinking about the messages we want to send, then
consider the role that would best receive it. Try not to start with the roles we
have in the existing system first (Macguyver Method), which will take the focus
away from the story we want to tell and instead put it on the tools we currently
have at our disposal. Once we decide on messages and roles, trust them; don't
muddy code with type checks (including nil checks).

- Think about the message you want to send, then the _role_ that would best
  receive that message, then consider the objects you actually have in the
  system.

- The goal is to avoid a "Macguyver" approach of making whatever tool (objects)
  you have available in the system work for the job, even if they aren't really
  designed for it.

- Trust your ducks; type checking muddies up the method code and makes it harder
  to understand and harder to test.


## Collecting Input

### Introduction to collecting input

- Take time to consider direct and indirect inputs your method will rely on.
  This will effect clarity and also the surface area for future bugs - indirect
  inputs combined with other inputs to fetch a _required_ value being one of the
  most common sources for bugs in software. ENV variables, class names, too.

- Keep defensive programming tactics (coercing input, checking input for
  correctness, etc) to the _boundaries, not the hinterlands_ of the program.
  Trust input once it passes those boundary checks. Improves readability,
  clarity, etc on the internals of your application code.

### Use built-in conversion protocols

Summary: There are implicit conversion methods (the given type is expected to be
very similar to the converted type, i.e. `#to_str` for something very similar to
a String. There are explicit converstion methods (the given type is expected to
be representable and not necessarily similar to the converted type, i.e.
`#to_s`. If you want strictness, you can use the implicit conversion methods to
convert inputs to the expected type (as `String#to_str` retuns a String, too).
You can be less strict by using explicit conversion methods (like `#to_s`).

- Useful when you want to ensure the inputs are of a specific _core type_, such
  as `Integer`.

- Ruby has a number of defined converstion protocols: `#to_str, #to_i, #to_path`

- Ruby's core libraries often do implicit conversion using methods like
  `#to_str`, `#to_int`, and `#to_path`. e.g., `File#open` will call `#to_path`
  on the `filename` argument. If an object defines it and returns the expected
  `String`, it will work. No need to manually convert the object to `String` and
  pass that in.

- Implicit conversion methods are for converting classes that are mostly like
  the target class; explicit conversion methods are for things that are mostly
  unlike. Example, there are many ways to convert a `Time` object to a string,
  so you'd use explicit `#to_s`. But the `Path` class defines a `#to_path`
  implicit conversion method.

- String interpolation uses `#to_s`. Concatenating two strings with `+` will
  implicitly call `#to_str`.

- Takeaway: if you know what you want, ask for it! If you expect an integer,
  consider coercing the input to an int. Calling `1.to_int` returns `1` still.
  If the object passed in doesn't respond to `#to_int` an error is thrown at the
  boundary where bad data was introduced.

### Conditionally call conversion methods

- Use `#respond_to?` to make sure the object you're given can give you the kind
  of object you need.

- You aren't asking "Are you this type of object?" you are asking "Can you
  _give_ me this type of object?" Using `#respond_to?` for the former is frowned
  upon (can lead to clunky code quickly). The latter is focused on the
  _messages_ and doesn't care what you give so long as it can give the method
  what it needs.

### Define your own conversion protocols

- You can define your own conversion protocols. This allows you to take in plain
  old data and convert it when you need it. _Allows for easy extension later._

- The example given is the idea of `coordinates` that is an array of X,Y values.
  A `Point` class defines its `#to_coords` which converts the object into a two
  element array.

### Define conversions to user-defined types

- You can define your own conversion protocols on your user-defined types to
  ensure your working with something expected. For example, your `Meters` class
  can implemented a `#to_meters` method that returns self, and internally call
  `to_meters` on the passed in object used for calculations. If the passed in
  object doesn't implement a `#to_meters` conversion method, you'll get an
  error, which is what you want.
  
### Use built-in conversion functions

- Methods like `Array()`, `Integer()`.
- These are actually ordinary methods but break convention as they start with capital letters.
- Useful if you want more forgiving API as they can convert a wider array of types than the equivalent `#to_*` methods.

### Define conversion functions

- Objects that make fewer demands are easier to use. If our object can accomodate many different objects as helpers, it's making less demands, and conversion functions help us do this. *Me: I like the framing of "make fewer demands of collaborators"... does that fit for applying Open/Closed principle? You don't require new collaborators to reach into the guts of existing classes... maybe a reach?*
- Define your own indempotent conversion function that is applied to any incoming objects. The example used is defining a `Point()` method which yields a `Point` object when given two elemnt arrays, specially formatted strings, or `Point` objects.
- You can make this more flexible by calling library defined conversions (like `#to_point`) on objects that respond to it, or converting the object to an array with `#to_ary` if it responds to it.
- In short, make it easy to get along with!

### Replace "string typing" with classes

- An example of string typing is casing on "magic strings".
- Instead make the type system and polymorphic method dispatch do your work for you.
They'll let you avoid constantly checking and rechecking inputs for correctness.
