# Structs

A struct is a collection of fields accessible via dot notation. When working with a struct pointer, you have the option to manually dereference it with `(*p).x`. However, Gyro allows you to simply write `p.x` without explicitly dereferencing, as the manual approach can often be tedious.

```gyro
import { println } from "@gyro/stdlib"

struct Vertex {
  public x: int,
  public y: int
}

fn main(): int {
  var v = Vertex{1, 2}
  var p = &v
  p.x = 1e9
  println(v)
}
```

## Struct Literals

A struct literal represents a newly allocated struct, with its fields initialized by listing their values. You can specify only a subset of fields using the Name: syntax, and the order of these named fields doesn’t matter. The special & prefix returns a pointer to the struct value.

```gyro
import { println } from "@gyro/stdlib"

struct Vertex {
  public x: int,
  public y: int
}

fn main(): int {
  var v1 = Vertex{1, 2}  // has type Vertex
  var v2 = Vertex{x: 1}  // y:0 is implicit
  var v3 = Vertex{}      // x:0 and y:0
  var p  = &Vertex{1, 2} // has type *Vertex

  println(v1, p, v2, v3)

  return 0
}

```