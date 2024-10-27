# Arrays

The type `T[n]` denotes an array containing `n` elements of type `T`.

Gyro also includes built-in support for dynamic arrays with various configurations:

- `T[n,m]` specifies a dynamic array with an initial length of `n` (allocated with a size of `n * |T|`) and a maximum length of `m`. If the length surpasses `m` or drops below `n`, the program will panic.

- `T[n,]` denotes a dynamic array with an initial length of `n` and no upper limit on its length.

- `T[,m]` represents a dynamic array with a maximum length of `m` but no initial length requirement.

- `T[]` defines a fully dynamic array without any restrictions on length.

```gyro
import { println } from "@gyro/stdlib"

fn main() {
  var a: string[2]

  a[0] = "Hello"
  a[1] = "World"

  println(a[0], a[1])
}
```