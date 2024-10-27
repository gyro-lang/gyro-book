# Functions

A function can accept zero or more arguments.

In this example, `add` takes two parameters of type `int`:

```gyro
fn add(a: int, b: int): int {
  return a + b
}
```

Notice that each variable's type follows the variable name, separated by a colon `:`.

(For more details on why types are arranged this way, see the article on Gyro's declaration syntax.)