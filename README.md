# hubris-userlib

This is a task library for interfacing with the Hubris kernel, extracted from
Cliff Biffle's [exhubris](https://github.com/cbiffle/exhubris) and modified to
allow for cleaner integration into other libraries or custom environments.

## Changes

### Startup code

By default, userlib provides the `_start` routine that sets up the C/Rust
runtime environment (initializing `.data`, zeroing `.bss`) and calls the task's
`main`. This is controlled by one feature:

- `startup` (enabled by default) provides `_start`. Most tasks want this.
  Disable it (turn off default features) if the task, or another crate, needs to
  provide its own `_start` and entry sequence.

### Panic handling

userlib provides the task's `panic_handler`. Which handler gets registered is
chosen by one of three *mutually exclusive* features. If none of them are
enabled, userlib will not register a panic handler.

- `panic-messages` (enabled by default) registers the full handler, which
  formats the panic message together with its file and line and reports them to
  the supervisor.
- `panic-no-messages` registers a minimal handler that reports the fixed string
  `"PANIC"` instead of the real message, omitting the message-formatting
  machinery to save flash at the cost of debuggability.
- `panic-never` disallows panics outright: any `panic!` the compiler cannot
  prove unreachable becomes a link-time error.
