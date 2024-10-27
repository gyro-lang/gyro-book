# Types

Each value in Gyro must have a specific datatype. While the Gyro compiler
typically infers types from the functions you use or the literals provided as
initializers, there are cases where explicit type annotations are required.
These annotations help the compiler optimize your code more efficiently and
maintain consistency throughout your codebase.

Type annotations are mandatory when defining function parameters and are provided using the following syntax:

```gyro
fn main(argc: i32, argv: *char[]): i32 {
  var x = argc

  // Implement your program logic here

  return argc
}
```

In this example:

- The function `main` accepts two parameters: `argc` of type `i32` and `argv` which is a pointer to an array of `char` values.
- The return type of the function is explicitly set to `i32`.
- Inside the function, the variable `x` is automatically inferred as `i32` based on the type of `argc`.

Using type annotations in this way ensures that both the function’s parameters
and its return value are clearly defined, making the code easier to understand
and debug.

## Basic Types

Gyro's basic types include:

- **Booleans**: `bool`
- **Strings**: `string`
- **Integers**: `int`, `int8`, `int16`, `int32`, `int64`, `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr`
  - **Aliases**:
    - `byte` (alias for `uint8`)
    - `rune` (alias for `int32`, represents a Unicode code point)
- **Floating-point numbers**: `float32`, `float64`
- **Complex numbers**: `complex64`, `complex128`

Example code demonstrates variables of different types, as well as "factored" variable declarations grouped into blocks, similar to import statements.

The `int`, `uint`, and `uintptr` types adapt to system architecture, being 32 bits wide on 32-bit systems and 64 bits wide on 64-bit systems. When you need an integer, use `int` unless you specifically require a sized or unsigned integer.

## Zero Values

Variables declared without an explicit initial value are automatically assigned a default "zero value." The zero values are as follows:

- `0` for numeric types
- `false` for the boolean type
- `""` (an empty string) for strings

## Type Conversions

The expression `T(v)` converts the value `v` to the type `T`.

Here are some examples of numeric conversions:

```gyro
var i: int = 42
var f: float64 = float64(i)
var u: uint = uint(f)
```