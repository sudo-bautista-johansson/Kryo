# Kryo

**A systems programming language where the type system knows more than the type.**

Kryo tracks *how much* a value may hold, *where* it lives and *what state* it is in — so whole classes of
bugs are rejected before the program runs: an index that walks off an array, a buffer pushed past its
capacity, a file used after it was closed, a number that silently wraps.

One front end, two engines: a reference interpreter (`kryo run`) that executes the whole language, and a
native backend (`kryo build`) that lowers it to LLVM IR and produces a real executable. A differential test
suite requires both to print exactly the same output.

```kryo
fn main() -> i32 {
    let mut total: i32 = 0;

    for value in range(0, 4) {
        total = total + value;
    }

    println("total = {}", total);
    return total;      // 6
}
```

---

## Why Kryo

| Idea | What it means when you write code |
|---|---|
| **Capacity is part of the type** | `BoundedVec<i32, 4>` can never hold five elements; `Bounded<i32, 0, 100>` can never leave its range. A literal outside the range fails to compile, a computed value is checked when it is produced. |
| **Memory domains are types** | `Stack<T>`, `Heap<T>`, `GPU<T>`, `Shared<T>`, `Persistent<T>`, `Distributed<T>`, `Transfer<T>` each carry a lifetime model. Moving data between them is explicit. |
| **Resources are state machines** | `File<Open>` becomes `File<Closed>` when you close it. Reading a closed file, closing it twice, or forgetting to close it are compile-time errors. |
| **Contracts live in the signature** | `requires` and `ensures` sit next to the parameters and run before and after the body. |
| **Borrows end at their last use** | Non-lexical lifetimes: a reference freezes its owner only while it is still needed, so ordinary code compiles without lifetime annotations. |
| **Concurrency is built in** | `Thread::spawn`, `Atomic<i32>`, `async fn` and `.await` are part of the language, with the handle tracked until it is joined. |
| **No silent arithmetic** | `+ - * / %` are checked in both engines; overflow is an error, not a wrap. |
| **Diagnostics that teach** | Every rejection carries a code, a span, a label under the offending text and usually a suggestion. |

---

## Quick start

### Build the compiler

```console
$ cargo build --release --workspace
$ ./target/release/kryo --help
```

> `--workspace` matters: the root package declares no targets, so a plain `cargo build --release` succeeds
> without producing a binary.

### Hello world

```kryo
// hello.kyo
fn main() -> i32 {
    println("Hello, Kryo!");
    return 0;
}
```

```console
$ kryo run hello.kyo
Hello, Kryo!

✓ Execution complete
Result: 0
```

### Compile it to a native binary

```console
$ kryo build hello.kyo
$ ./hello
Hello, Kryo!
```

The native backend needs `clang`. Kryo looks for it in the `KRYO_CLANG` environment variable, then on `PATH`,
then in the usual LLVM install locations. `kryo run` never needs it.

---

## A tour of the language

### Capacity types

```kryo
fn main() -> i32 {
    let mut buffer: BoundedVec<i32, 4> = BoundedVec::new(4);
    buffer = buffer.push(10);
    buffer = buffer.push(20);

    // A computed value is checked where it is created.
    let temperature: Bounded<i32, 0, 100> = 42;

    println("{} items, temperature {}", buffer.len(), temperature);
    return 0;
}
```

### Memory domains

```kryo
fn main() -> i32 {
    let counter = Heap::alloc(0);
    counter.write(10);
    counter.increment();
    println("heap = {}", counter.get());

    let staged = Transfer::new(7);
    staged.write(8);
    return staged.get();
}
```

### Resource state machines

```kryo
fn main() -> i32 {
    let file = File::open("data.bin");
    let size = File::read(file);
    let closed = File::close(file);
    return size;
}
```

Leaving the file open, reading after closing, or closing twice is rejected with `E0011`.

### Contracts

```kryo
fn clamp(value: i32, low: i32, high: i32) -> i32
    requires low <= high
    ensures result >= low
    ensures result <= high
{
    if value < low { return low; }
    if value > high { return high; }
    return value;
}

fn main() -> i32 {
    return clamp(150, 0, 100);   // 100
}
```

### Structs, methods and operator overloading

```kryo
struct Vec2 { x: i32, y: i32 }

impl Vec2 {
    fn length_sq(self) -> i32 { return self.x * self.x + self.y * self.y; }
}

impl Add for Vec2 {
    fn add(self, other: Vec2) -> Vec2 {
        return Vec2 { x: self.x + other.x, y: self.y + other.y };
    }
}

impl PartialOrd for Vec2 {
    fn lt(self, other: Vec2) -> bool { return self.length_sq() < other.length_sq(); }
}

fn main() -> i32 {
    let a: Vec2 = Vec2 { x: 1, y: 2 };
    let b: Vec2 = Vec2 { x: 3, y: 4 };
    let sum: Vec2 = a + b;
    if a < b { println("a is shorter"); }
    return sum.length_sq();
}
```

`Add`, `Sub`, `Mul`, `Div`, `Rem`, `Eq` and `PartialOrd` are overloadable. Only `lt` is required for
ordering; `>`, `<=` and `>=` derive from it.

### Enums and pattern matching

```kryo
enum Shape { Circle(i32), Square(i32) }

fn area(shape: Shape) -> i32 {
    match shape {
        Shape::Circle(r) => return r * r * 3,
        Shape::Square(side) => return side * side,
    }
}

fn main() -> i32 {
    return area(Shape::Circle(2)) + area(Shape::Square(6));   // 48
}
```

### Threads, atomics and async

```kryo
fn bump() -> i32 {
    let mut i: i32 = 0;
    while i < 1000 { i = i + 1; }
    return 0;
}

async fn slow() -> i32 {
    Thread::sleep(100);
    return 1;
}

fn main() -> i32 {
    let a = Thread::spawn("bump");
    let b = Thread::spawn("bump");
    let x: i32 = Thread::join(a);
    let y: i32 = Thread::join(b);

    // Async tasks queue onto a worker pool and really overlap.
    let p = slow();
    let q = slow();
    println("async = {}", p.await + q.await);

    return x + y;
}
```

Shared state goes through `Atomic<i32>`: `load`, `store`, `fetch_add`, `fetch_sub`, `swap`,
`compare_exchange`. Ordinary variables are per-thread, so a data race has nowhere to hide.

### Modules

```kryo
mod math {
    pub fn double(n: i32) -> i32 { return n * 2; }
    pub struct Point { pub x: i32, pub y: i32 }
}

fn main() -> i32 {
    let p = math::Point { x: 3, y: 4 };
    return math::double(p.x) + p.y;
}
```

`mod name;` loads `name.kyo` from the same directory, so a program can be split across files. Visibility is
enforced for functions, types and fields — including reads.

---

## Command line

| Command | What it does |
|---|---|
| `kryo run <file>` | Compile in memory and execute |
| `kryo check <file>` | Front end only: no execution, no artifacts |
| `kryo build <file> [-o path]` | Emit LLVM IR, link an executable, run it |
| `kryo build-opt <file> [-l 0..3]` | The same pipeline at a chosen optimisation level |
| `kryo fmt <file>` | Canonical formatting, comments preserved |
| `kryo docs <file> [-f md\|html]` | Reference generated from declarations and `///` comments |
| `kryo lint <file>` | Unused variables, dead functions, style |
| `kryo test [-t unit\|integration\|property\|all]` | Run the selected suites |
| `kryo benchmark <file> [-w warmup] [-i iterations]` | Time a program |
| `kryo explain <CODE>` | Long-form explanation of any diagnostic |
| `kryo new <name>` / `kryo init` | Scaffold a package |
| `kryo install` / `kryo update` | Manage package dependencies |
| `kryo clean` | Remove build artifacts |
| `kryo lsp` | Language server: completion, hover, definition, references, rename |

Global flags: `--color auto\|always\|never` and `--message-format human\|short\|json`.

Exit codes: `0` success · `1` a diagnostic was reported · `2` a file or I/O problem · `3` an internal error.

---

## Diagnostics

```console
error[E0004]: undefined variable `totl`
 --> program.kyo:3:12
  |
3 |     return totl + 1;
  |            ^^^^ not found in this scope
  |
help: declare the name with `let` before using it, or check its spelling
suggestion: did you mean `total`?
```

| Code | Meaning |
|---|---|
| `E0001` | Lexical error |
| `E0002` | Syntax error |
| `E0003` | Type error |
| `E0004` | Undefined or private name |
| `E0005` | Borrow or ownership error |
| `E0006` | Construct the native backend does not lower |
| `E0007` | Package or toolchain problem |
| `E0008` | Usage error |
| `E0009` | Runtime error: bounds, overflow, assertion, panic |
| `E0010` | Contract violation |
| `E0011` | Resource state violation |
| `E0012` | `unsafe` rule violation |
| `E0013` | Formatting refused (the file was left untouched) |

`kryo explain E0005` prints the long form of any code.

---

## Editor support

Two extensions ship with the repository at version 1.2.0:

```console
$ code --install-extension kryo-vscode-extension\kryo-language-1.2.0.vsix
$ windsurf --install-extension kryo-windsurf-extension\kryo-language-windsurf-1.2.0.vsix
```

Syntax highlighting, language configuration, snippets for every construct in this README, and commands for
build, run, check, format, lint, restart server and documentation.

---

## Libraries

Nine self-contained modules in `lib/`, each with a demo that fails loudly if a check does not hold:

| Module | What it gives you |
|---|---|
| `bits.kyo` | Population count, leading and trailing zeros, rotate, byte swap, field extraction |
| `bitmap.kyo` | Frame allocator over a `[u32; N]`: set, clear, test, first free |
| `fixed.kyo` | Q16.16 fixed point: multiply, divide, saturate, convert — no FPU |
| `mmio.kyo` | Register access with shift and mask, plus a PL011-style UART with FIFOs |
| `ring.kyo` | Power-of-two byte ring buffer |
| `sched.kyo` | Round-robin task table with tick accounting |
| `text.kyo` | Allocation-free formatting: integers in any base, padding, hex dumps |
| `tui.kyo` | Boxes, headings, progress bars and sparklines for a text console |
| `vecmath.kyo` | Fixed-size vector math over `[i32; 3]` and `[i32; 4]`, integer sqrt, RGBA packing |

Each library is a plain file; combine it with your program by concatenation (or load it with `mod`):

```console
$ Get-Content lib\bits.kyo, lib\tests\bits_demo.kyo | Set-Content both.kyo
$ kryo run both.kyo
```

---

## Repository layout

```
compiler/    lexer, parser, resolver, type checker, ownership, optimiser, interpreter,
             LLVM backend, diagnostics, documentation generator, printer
cli/         the `kryo` binary, language server, test runners
lib/         standard-library modules and their demos
examples/    runnable programs, verified by the test suite
tests/       language, diagnostics, libraries, security and differential suites
docs/        the full reference as a single HTML page
```

---

## Testing

```console
$ cargo test --workspace
```

**647 tests**, none ignored. The suite covers the language surface feature by feature, the diagnostics
renderer, every library with its demo, the CLI end to end, a differential harness that requires the
interpreter and the native binary to print identical output, generated programs from a seeded PRNG, and a
security suite in which every case is an attack that must be rejected.

```console
# benchmarks (writes BENCHMARKS.md)
$ cargo test --release --test benchmarks_test -- --nocapture --test-threads=1
```

---

## Documentation

[`docs/index.html`](docs/index.html) is the complete reference: introduction, tutorial, syntax, operators,
types, memory and safety, functions and generics, data structures, modules, built-in library, command line,
diagnostics, testing, examples, libraries, internals, safety rules, project layout and an FAQ. It is
self-contained — open it in a browser, no server and no network needed.

Other material:

| Path | Contents |
|---|---|
| [`docs/index.html`](docs/index.html) | Complete reference (authoritative) |
| [`lib/`](lib) | Nine verified libraries with demos |
| [`examples/`](examples) | Runnable example programs |
| [`AUDIT_REPORT.md`](AUDIT_REPORT.md) | Source audit: defects, test suite, benchmarks |
| [`BENCHMARKS.md`](BENCHMARKS.md) | Measured front-end and interpreter performance |

---

## Requirements

- Rust 1.70+ (tested with 1.98)
- `clang` from LLVM for `kryo build` and `kryo build-opt` — not needed for `kryo run`

---

## License

Apache.
