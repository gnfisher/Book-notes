# Confident Ruby

## Performing Work

Summary: We should start by thinking about the messages we want to send, then
consider the role that would best receive it. Try not to start with the roles we
have in the existing system first (Macguyver Method), which might lead to less
optimal designs that consider the roles before the messages. Once we decide on
messages and roles, trust them; don't muddy code with type checks (including nil
checks).

- Think about the message you want to send, then the _role_ that would best
  receive that message, then consider the objects you actually have in the
  system.

- The goal is to avoid a "Macguyver" approach of making whatever tool (objects)
  you have available in the system work for the job, even if they aren't really
  designed for it.

- Trust your ducks; type checking muddies up the method code and makes it harder
  to understand and harder to test.

