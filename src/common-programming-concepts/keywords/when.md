# when

The when keyword can be used to observe the value of a [variable](/common-programming-concepts/variables.md).

During compilation, the compiler will look at the full dependency list of the passed `condition` block and will observe any variable in it's hierarchy.

```gyro
import * as std from "std"

var x: i64 = 0

when x > 5 {
  std::printf("X equals %d", x)
}

x = 25
```

This example will output `X equals 25`