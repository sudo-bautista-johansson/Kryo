# Kryo CLI Reference

Complete reference for all Kryo command-line interface commands and options.

## Installation

```bash
# Install via cargo
cargo install kryo

# Or build from source
cargo build --release
cargo install --path .
```

## Commands

### `kryo build`

Compile a Kryo source file to an executable.

```bash
kryo build <file> [options]
```

**Options:**
- `-o, --output <path>`: Specify output file path
- `-t, --target <target>`: Specify compilation target (e.g., x86_64-marcus-os)

**Examples:**
```bash
# Basic compilation
kryo build main.kyo

# Specify output
kryo build main.kyo -o myapp.exe

# Specific target
kryo build main.kyo -t x86_64-marcus-os
```

**Output:**
```
Compiling: main.kyo
✓ Lexical analysis complete (X tokens)
✓ Parsing complete
✓ Name resolution complete
✓ Type checking complete
✓ Borrow checking complete
✓ Constant propagation complete
✓ Dead code elimination complete
✓ HIR generation complete (X modules)
✓ MIR generation complete (X functions)
✓ LLVM code generation complete
✓ LLVM IR written to: main.ll
✓ Object file written to: main.o
✓ Executable written to: main.exe
✅ Build complete
```

### `kryo run`

Compile and immediately run a Kryo source file.

```bash
kryo run <file>
```

**Examples:**
```bash
# Run program
kryo run main.kyo

# Run with absolute path
kryo run "C:\Users\User\project\main.kyo"
```

**Output:**
```
Running: main.kyo
✓ Lexical analysis complete (X tokens)
✓ Parsing complete
✓ Name resolution complete
✓ Type checking complete
✓ Borrow checking complete
✓ Constant propagation complete
✓ Dead code elimination complete
[Program output here]
✓ Execution complete
Result: 0
```

### `kryo check`

Type check a Kryo source file without compilation.

```bash
kryo check <file>
```

**Examples:**
```bash
# Type check only
kryo check main.kyo
```

**Output:**
```
Checking: main.kyo
✓ Lexical analysis complete
✓ Parsing complete
✓ Name resolution complete
✓ Type checking complete
✓ Borrow checking complete
✓ Constant propagation complete
✓ Dead code elimination complete
✓ HIR generation complete (X modules)
✓ MIR generation complete (X functions)
✓ LLVM code generation complete
✓ Type check successful
```

### `kryo fmt`

Format Kryo source code according to style guidelines.

```bash
kryo fmt [file]
```

**Examples:**
```bash
# Format specific file
kryo fmt main.kyo

# Format all files in directory (if implemented)
kryo fmt .
```

### `kryo lint`

Run linter on Kryo source code to detect potential issues.

```bash
kryo lint <file>
```

**Examples:**
```bash
# Lint file
kryo lint main.kyo
```

### `kryo docs`

Generate documentation for Kryo source code.

```bash
kryo docs <file> [options]
```

**Options:**
- `-f, --format <format>`: Output format (markdown, html)
- `--default`: markdown

**Examples:**
```bash
# Generate markdown documentation
kryo docs main.kyo

# Generate HTML documentation
kryo docs main.kyo -f html
```

### `kryo benchmark`

Run performance benchmarks on Kryo source code.

```bash
kryo benchmark <file> [options]
```

**Options:**
- `-w, --warmup <iterations>`: Warmup iterations (default: 10)
- `-i, --iterations <iterations>`: Benchmark iterations (default: 100)

**Examples:**
```bash
# Run with default settings
kryo benchmark main.kyo

# Custom warmup and iterations
kryo benchmark main.kyo -w 20 -i 1000
```

### `kryo test`

Run test suite.

```bash
kryo test [options]
```

**Options:**
- `-t, --test-type <type>`: Test type (all, unit, integration, property)

**Examples:**
```bash
# Run all tests
kryo test

# Run only unit tests
kryo test -t unit

# Run integration tests
kryo test -t integration
```

### `kryo clean`

Clean build artifacts.

```bash
kryo clean
```

**Examples:**
```bash
# Clean all build artifacts
kryo clean
```

### `kryo new`

Create a new Kryo package.

```bash
kryo new <name> [options]
```

**Options:**
- `-p, --path <path>`: Directory path for the package

**Examples:**
```bash
# Create package in current directory
kryo new myproject

# Create package in specific directory
kryo new myproject -p /path/to/projects
```

### `kryo init`

Initialize a package in the current directory.

```bash
kryo init
```

**Examples:**
```bash
# Initialize current directory as Kryo package
kryo init
```

### `kryo install`

Install dependencies.

```bash
kryo install
```

### `kryo update`

Update dependencies.

```bash
kryo update
```

### `kryo build-opt`

Build with optimization level.

```bash
kryo build-opt <file> [options]
```

**Options:**
- `-o, --output <path>`: Specify output file path
- `-l, --level <level>`: Optimization level (0-3, default: 2)

**Examples:**
```bash
# Build with default optimization (O2)
kryo build-opt main.kyo

# Build with maximum optimization
kryo build-opt main.kyo -l 3

# Build with no optimization (for debugging)
kryo build-opt main.kyo -l 0
```

### `kryo lsp`

Start LSP (Language Server Protocol) server for IDE integration.

```bash
kryo lsp
```

**Examples:**
```bash
# Start LSP server
kryo lsp
```

## Language Features

### Types

**Primitive Types:**
- `i8, i16, i32, i64, i128`: Signed integers
- `u8, u16, u32, u64, u128`: Unsigned integers
- `f32, f64`: Floating point numbers
- `bool`: Boolean
- `char`: Character
- `str`: String slice
- `String`: Owned string

**Memory Domain Types:**
- `Stack<T>`: Stack-allocated memory
- `Heap<T>`: Heap-allocated memory
- `Persistent<T>`: File-backed persistent memory
- `GPU<T>`: GPU memory
- `Shared<T>`: Shared memory
- `Distributed<T>`: Distributed memory

**Capacity Types:**
- `Array<T, N>`: Fixed-size array
- `BoundedVec<T, C>`: Bounded vector
- `Bounded<T, MIN, MAX>`: Bounded integer

### Operators

**Arithmetic:**
- `+`, `-`, `*`, `/`, `%`

**Comparison:**
- `==`, `!=`, `<`, `>`, `<=`, `>=`

**Logical:**
- `&&`, `||`, `!`

**Bitwise:**
- `&`, `|`, `^`, `<<`, `>>`

**Assignment:**
- `=`, `+=`, `-=`, `*=`, `/=`, `%=`

### Control Flow

**If-Else:**
```kryo
if condition {
    // then block
} else {
    // else block
}
```

**While Loop:**
```kryo
while condition {
    // loop body
}
```

**Loop Control:**
```kryo
break;  // Exit loop
continue;  // Skip to next iteration
```

### Functions

**Definition:**
```kryo
fn function_name(param1: Type, param2: Type) -> ReturnType {
    // function body
    return value;
}
```

**Calling:**
```kryo
let result = function_name(arg1, arg2);
```

### Variables

**Declaration:**
```kryo
let x: i32 = 10;        // Immutable
let mut y: i32 = 20;     // Mutable
```

### Built-in Functions

**I/O:**
- `print(value)`: Print without newline
- `println(value)`: Print with newline
- `read_line()`: Read string from stdin
- `read_int()`: Read integer from stdin
- `read_float()`: Read float from stdin

**Collections:**
- `Vec::new()`: Create empty vector
- `BoundedVec::new(cap)`: Create bounded vector
- `range(start, end)`: Create range

**Memory:**
- `Stack::alloc(value)`: Stack allocation
- `Heap::alloc(value)`: Heap allocation
- `GPU::alloc(value)`: GPU allocation
- `Shared::alloc(value)`: Shared memory allocation
- `Distributed::alloc(value)`: Distributed allocation
- `Persistent::map(file, value)`: Persistent memory mapping
- `Persistent::unmap(value)`: Unmap persistent memory
- `Domain::transfer(value)`: Transfer between domains
- `Domain::copy(value)`: Copy between domains

**Debugging:**
- `assert(condition)`: Assert condition
- `panic(message)`: Panic with message

## Error Codes

The compiler uses structured error codes for better error reporting:

- **E0001**: Lexer error
- **E0002**: Parser error
- **E0003**: Type error
- **E0004**: Name error
- **E0005**: Ownership error
- **E0006**: Borrow error
- **E0007**: Contract error
- **E0008**: Resource error
- **E0009**: Runtime error
- **E0010**: FFI error
- **E0011**: Module error
- **E0012**: Package error

## Warnings

The compiler provides warnings for potential issues:

- **W0001**: Unused variable
- **W0002**: Dead code
- **W0003**: Unused import
- **W0004**: Shadowing
- **W0005**: Type inference warning
- **W0006**: Unsafe code warning
- **W0007**: Deprecated feature
- **W0008**: Performance warning

## Configuration

Kryo can be configured via:
- Command-line options
- Environment variables
- Project configuration files (Kryo.toml)

## IDE Integration

### VSCode / Windsurf

Install the Kryo extension for:
- Syntax highlighting
- Code completion
- Error diagnostics
- Go to definition
- Hover information
- Code formatting

### LSP Server

Start the LSP server for advanced IDE features:
```bash
kryo lsp
```

## Examples

### Hello World

```kryo
fn main() -> i32 {
    println("Hello, World!");
    return 0;
}
```

### Formatted Output

```kryo
fn main() -> i32 {
    let nombre: String = "Ana";
    let edad: i32 = 25;
    let activo: bool = true;

    println("Nombre: {}", nombre);
    println("Edad: {}", edad);
    println("Activo: {}", activo);
    return 0;
}
```

### Arithmetic

```kryo
fn main() -> i32 {
    let a: i32 = 10;
    let b: i32 = 3;
    
    println("Suma: {} + {} = {}", a, b, a + b);
    println("Resta: {} - {} = {}", a, b, a - b);
    println("Multiplicación: {} * {} = {}", a, b, a * b);
    println("División: {} / {} = {}", a, b, a / b);
    println("Módulo: {} % {} = {}", a, b, a % b);
    
    return 0;
}
```

### Control Flow

```kryo
fn main() -> i32 {
    let x: i32 = 15;
    
    if x > 10 {
        println("x es mayor que 10");
    } else {
        println("x no es mayor que 10");
    }
    
    let mut i: i32 = 0;
    while i < 3 {
        println("Iteración: {}", i);
        i = i + 1;
    }
    
    return 0;
}
```

### Memory Domains

```kryo
fn main() -> i32 {
    let valor: i32 = 100;
    
    let stack_alloc = Stack::alloc(valor);
    let heap_alloc = Heap::alloc(valor);
    let gpu_alloc = GPU::alloc(valor);
    
    println("Dominios de memoria funcionan");
    return 0;
}
```

## Troubleshooting

### Common Issues

**"kryo: command not found"**
- Ensure Kryo is installed and in your PATH
- Try `cargo install kryo` or build from source

**"Cannot find file"**
- Use absolute paths or ensure you're in the correct directory
- Check file extension is `.kyo`

**"Type error"**
- Check type annotations match expressions
- Verify function return types
- Ensure proper type conversions

**"Borrow checking failed"**
- Review ownership and borrowing rules
- Check for multiple mutable references
- Ensure proper lifetimes

## Performance Tips

1. **Use appropriate types**: Choose the right primitive types for your needs
2. **Enable optimizations**: Use `kryo build-opt -l 3` for release builds
3. **Avoid unnecessary allocations**: Stack allocation when possible
4. **Use bounded types**: Prevent runtime bounds checking overhead
5. **Profile code**: Use `kryo benchmark` to identify bottlenecks

## Next Steps

- Read the [Language Specification](language-spec.md) for complete language reference
- Check [Best Practices](best-practices.md) for idiomatic Kryo code
- Explore [Tutorial](tutorial.md) for step-by-step learning
- Review [Architecture](architecture.md) for compiler internals