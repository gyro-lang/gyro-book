# Variables and Constants

Gyro is a strongly typed language by default but it also offers many features
that allow for rapid development and automatic type inference.

Declarations of any named entity are automatically propagated to the child
scopes within that scope.

```gyro
var x: i64 = 0

fn main() {
  std::println("x = %d", x) // x = 0

  x = 4
}
```

Constants are declared using the `const` keyword and are inherently immutable.
This means that neither the constant itself nor any of its properties or members
can be modified once it has been defined.

```gyro
const y = 24

fn main() {
  std::println("y = %d", y) // y = 24

  y = 12 // Compilation error: cannot redeclare constant 'y', consider using 'var' instead.
}
```