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

- Take time to consider direct and indirect inputs your method will rely on.
  This will effect clarity and also the surface area for future bugs - indirect
  inputs combined with other inputs to fetch a _required_ value being one of the
  most common sources for bugs in software.

- Keep defensive programming tactics (coercing input, checking input for
  correctness, etc) to the _boundaries, not the hinterlands_ of the program.
  Trust input once it passes those boundary checks. Improves readability,
  clarity, etc on the internals of your application code.


