# Kryo Cheatsheet

Quick reference for Kryo programming language syntax and features.

## Table of Contents
- [CLI Commands](#cli-commands)
- [Basic Syntax](#basic-syntax)
- [Types](#types)
- [Operators](#operators)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Built-in Functions](#built-in-functions)
- [Memory Domains](#memory-domains)
- [Examples](#examples)

---

## CLI Commands

```bash
# Compile and run
kryo run archivo.kyo

# Compile to executable
kryo build archivo.kyo

# Type check only
kryo check archivo.kyo

# Format code
kryo fmt archivo.kyo

# Run linter
kryo lint archivo.kyo

# Generate documentation
kryo docs archivo.kyo

# Run benchmarks
kryo benchmark archivo.kyo

# Run tests
kryo test

# Clean build artifacts
kryo clean

# Build with optimization
kryo build-opt archivo.kyo -l 3
```

---

## Basic Syntax

### Hello World

```kryo
fn main() -> i32 {
    println("Hello, World!");
    return 0;
}
```

### Variables

```kryo
// Immutable
let x: i32 = 10;

// Mutable
let mut y: i32 = 20;
y = 30;
```

### Formatted Output

```kryo
let nombre: String = "Ana";
let edad: i32 = 25;
println("Nombre: {}, Edad: {}", nombre, edad);
```

---

## Types

### Primitive Types

```kryo
let a: i32 = 42;          // Signed 32-bit integer
let b: u64 = 100;         // Unsigned 64-bit integer
let c: f64 = 3.14;        // 64-bit float
let d: bool = true;       // Boolean
let e: char = 'A';        // Character
let f: String = "text";   // String
```

### Memory Domain Types

```kryo
let stack_val: Stack<i32> = Stack::new(42);
let heap_val: Heap<i32> = Heap::new(100);
let gpu_val: GPU<i32> = GPU::new(100);
let shared_val: Shared<i32> = Shared::new(100);
let dist_val: Distributed<i32> = Distributed::new(100);
```

### Capacity Types

```kryo
let arr: [i32; 5] = [1, 2, 3, 4, 5];
let bvec: BoundedVec<i32, 100> = BoundedVec::new();
let bounded: Bounded<i32, 0, 100> = Bounded::new(50);
```

---

## Operators

### Arithmetic

```kryo
let a: i32 = 10;
let b: i32 = 3;

a + b   // Addition: 13
a - b   // Subtraction: 7
a * b   // Multiplication: 30
a / b   // Division: 3
a % b   // Modulo: 1
```

### Comparison

```kryo
a == b  // Equal
a != b  // Not equal
a < b   // Less than
a > b   // Greater than
a <= b  // Less or equal
a >= b  // Greater or equal
```

### Logical

```kryo
let t: bool = true;
let f: bool = false;

t && f  // AND: false
t || f  // OR: true
!t      // NOT: false
```

### Assignment

```kryo
let mut x: i32 = 10;
x = 20;        // Assignment
x += 5;        // Add and assign
x -= 3;        // Subtract and assign
x *= 2;        // Multiply and assign
x /= 4;        // Divide and assign
x %= 3;        // Modulo and assign
```

---

## Control Flow

### If-Else

```kryo
if x > 10 {
    println("x is greater than 10");
} else {
    println("x is not greater than 10");
}
```

### While Loop

```kryo
let mut i: i32 = 0;
while i < 5 {
    println("Iteration: {}", i);
    i = i + 1;
}
```

### Break and Continue

```kryo
while true {
    if condition {
        break;    // Exit loop
    }
    if skip {
        continue; // Skip to next iteration
    }
}
```

---

## Functions

### Function Definition

```kryo
fn add(a: i32, b: i32) -> i32 {
    return a + b;
}
```

### Function Call

```kryo
let result = add(10, 20);
```

### No Return Value

```kryo
fn print_hello() -> () {
    println("Hello!");
}
```

---

## Built-in Functions

### I/O

```kryo
// Output
print("No newline");
println("With newline");

// Formatted output
let name = "Ana";
println("Hello, {}", name);

// Input
let input = read_line();
let num = read_int();
let float = read_float();
```

### Collections

```kryo
let vec = Vec::new();
let bvec = BoundedVec::new(10);
let rango = range(0, 5);
```

### Memory

```kryo
let stack_alloc = Stack::alloc(100);
let heap_alloc = Heap::alloc(100);
let gpu_alloc = GPU::alloc(100);
let shared_alloc = Shared::alloc(100);
let dist_alloc = Distributed::alloc(100);
```

### Debugging

```kryo
assert(x > 0);
panic("Error message");
```

---

## Memory Domains

### Stack

```kryo
let stack_val: Stack<i32> = Stack::new(42);
let result = stack_val.get();
```

### Heap

```kryo
let heap_val: Heap<i32> = Heap::new(100);
let value = heap_val.get();
```

### Persistent

```kryo
let filename: String = "data.bin";
let data: i32 = 42;
let persistent = Persistent::map(filename, data);
let value = persistent.read();
```

### GPU

```kryo
let gpu_val: GPU<i32> = GPU::allocate(1024);
gpu_val.write(42);
let result = gpu_val.read();
```

### Shared

```kryo
let shared_val: Shared<i32> = Shared::open("shared_mem");
shared_val.increment();
let result = shared_val.get();
```

### Distributed

```kryo
let dist_val: Distributed<i32> = Distributed::connect("cluster.node1");
let value = dist_val.read();
```

---

## Examples

### Arithmetic Example

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

### Control Flow Example

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

### Comparisons Example

```kryo
fn main() -> i32 {
    let a: i32 = 10;
    let b: i32 = 20;
    
    println("{} > {} = {}", a, b, a > b);
    println("{} < {} = {}", a, b, a < b);
    println("{} == {} = {}", a, b, a == b);
    println("{} != {} = {}", a, b, a != b);
    
    let t: bool = true;
    let f: bool = false;
    println("true && false = {}", t && f);
    println("true || false = {}", t || f);
    println("!true = {}", !t);
    
    return 0;
}
```

### Formatted Output Example

```kryo
fn main() -> i32 {
    let nombre: String = "Ana";
    let edad: i32 = 25;
    let activo: bool = true;
    
    println("Nombre: {}", nombre);
    println("Edad: {}", edad);
    println("Activo: {}", activo);
    
    println("Multiple: {} + {} = {}", 10, 20, 30);
    
    return 0;
}
```

### Memory Domains Example

```kryo
fn main() -> i32 {
    let valor: i32 = 100;
    
    let stack_alloc = Stack::alloc(valor);
    let heap_alloc = Heap::alloc(valor);
    let gpu_alloc = GPU::alloc(valor);
    let shared_alloc = Shared::alloc(valor);
    let dist_alloc = Distributed::alloc(valor);
    
    println("Todos los dominios de memoria funcionan");
    return 0;
}
```

### Collections Example

```kryo
fn main() -> i32 {
    let vec = Vec::new();
    println("Vec creado");
    
    let bounded_vec = BoundedVec::new(10);
    println("BoundedVec con capacidad 10 creado");
    
    let rango = range(0, 5);
    println("Range de 0 a 5 creado");
    
    return 0;
}
```

---

## Error Codes

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

---

## File Extension

Kryo source files use the `.kyo` extension.

---

## Comments

```kryo
// Single line comment

/* Multi-line comment */

/// Documentation comment
```

---

## Compiler Pipeline

```
Source (.kyo)
    ↓
Lexer (Tokenization)
    ↓
Parser (AST)
    ↓
Name Resolution
    ↓
Type Checking
    ↓
Borrow Checking
    ↓
HIR (High-level IR)
    ↓
MIR (Mid-level IR)
    ↓
LLVM IR Generation
    ↓
LLVM Backend
    ↓
Executable
```

---

## Quick Tips

1. Use `println` for formatted output with `{}` placeholders
2. Declare variables with `let` for immutable, `let mut` for mutable
3. All functions must have explicit return types
4. Use `return` to return values from functions
5. Memory domains require explicit allocation functions
6. Bounded types prevent overflow at compile time
7. `assert` is useful for debugging
8. `unsafe` blocks are required for FFI
9. Use `kryo check` to type-check without compilation
10. Use `kryo build-opt -l 3` for optimized release builds