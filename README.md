# monad.mq

A comprehensive monad library for [mq](https://github.com/harehare/mq) - bringing functional programming patterns to Markdown processing.

> [!NOTE]
> This is a hobby project exploring functional programming concepts within the mq.

## Overview

monad.mq provides a collection of monadic abstractions inspired by Clojure's algo.monads library, implemented for the mq language. It enables elegant composition of computations while working with Markdown and structured data.

## Features

- **Multiple Monad Implementations**
  - Identity Monad - Basic monadic structure
  - Maybe Monad - Null-safe computations
  - List Monad - Non-deterministic computations
  - State Monad - Stateful computations
  - Writer Monad - Computations with logging
  - Reader Monad - Environment-based computations
  - Either Monad - Error handling with Left/Right values
- **Unified Do Notation** - `domonad` syntax for cleaner monadic composition
- **Helper Functions** - Rich set of utilities including `m_sequence`, `m_map`, `m_comp`, `m_filter`, etc.
- **MonadPlus Support** - Zero and plus operations for applicable monads

## Quick Start

### Maybe Monad - Safe Division

```mq
include "monad"

def safe_divide(a, b):
  if (b == 0):
    None
  else:
    a / b
end

domonad(
  maybe_m(),
  [
    bind("x", safe_divide(10, 2)),
    bind("y", safe_divide(20, 4))
  ],
  fn(vars): maybe_return(vars["x"] + vars["y"]);
)
# Result: 10
```

### List Monad - Pythagorean Triples

```mq
include "monad"

pythagorean_triples(15)
# Result: [[3,4,5], [6,8,10], [5,12,13], [9,12,15]]
```

### Either Monad - Error Handling

```mq
include "monad"

def parse_int(str):
  let num = to_number(str)
  | if (is_none(num)):
      either_left("Invalid number: " + str)
    else:
      either_right(num)
end

domonad(
  either_m(),
  [
    bind("a", parse_int("42")),
    bind("b", parse_int("10"))
  ],
  fn(vars): either_return(vars["a"] + vars["b"]);
)
# Result: {"type": "right", "value": 52}
```

## Available Monads

| Monad    | Purpose              | Key Functions                                         |
| -------- | -------------------- | ----------------------------------------------------- |
| Identity | Basic composition    | `identity_return`, `identity_bind`                    |
| Maybe    | Null safety          | `maybe_return`, `maybe_bind`, `maybe_zero`            |
| List     | Non-determinism      | `list_return`, `list_bind`, `list_zero`               |
| State    | Stateful computation | `state_get`, `state_put`, `state_modify`, `run_state` |
| Writer   | Logging              | `writer_return`, `writer_tell`                        |
| Reader   | Environment access   | `reader_return`, `ask`, `asks`, `local`, `run_reader` |
| Either   | Error handling       | `either_left`, `either_right`, `is_left`, `is_right`  |

## Helper Functions

- `m_sequence(monad_def, ms)` - Convert list of monadic values to monadic list
- `m_map(monad_def, list, f)` - Map monadic function over list
- `m_comp(monad_def, f, g)` - Compose monadic functions
- `m_lift(monad_def, f)` - Lift regular function into monad
- `m_filter(monad_def, list, pred)` - Filter with monadic predicate
- `m_when(monad_def, condition, action)` - Conditional execution
- `m_unless(monad_def, condition, action)` - Negated conditional execution

## Examples

The library includes comprehensive examples demonstrating:

1. Safe division with Maybe monad
2. Non-deterministic pairs with List monad
3. Counter with State monad
4. Logging with Writer monad
5. Error handling with Either monad
6. Sequencing operations
7. Function composition
8. Pythagorean triples generation
9. Configuration management with Reader monad

See the source code for complete example implementations.

## Usage with mq

```sh
# Use the library in your mq scripts
mq 'include "monad" | example_maybe()' file.md

# Run with specific monad operations
mq 'include "monad" | pythagorean_triples(20)' file.md
```

## Requirements

- [mq](https://github.com/harehare/mq) - Command-line tool for processing Markdown

## License

This project is licensed under the MIT License.

## Related

- [mq](https://github.com/harehare/mq) - The mq language and CLI tool
- [Clojure algo.monads](https://github.com/clojure/algo.monads) - Inspiration for this library
