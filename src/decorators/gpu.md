# @gpu

The `@gpu` decorator enables methods to interact with your system's graphics card. Functions annotated with `@gpu` are implemented as kernels, allowing parallel execution on the GPU.

```gyro
@gpu fn VectorAdd(A: float*, B: float*, C: float*) {
  var i: int = threadIdx.x

  C[i] = A[i] + B[i]
}

fn main() {
  var A = [1.0, 2.0, 3.0]
  var B = [4.0, 5.0, 6.0]
  var C: float[3]

  VectorAdd<1, 8>(A, B, C)

  return 0
}
```

In this example, `VectorAdd` performs element-wise addition on arrays `A` and `B`, storing the result in array `C`. The `VectorAdd<1, 8>(A, B, C)` call specifies GPU execution parameters—here, `1` block with `8` threads, allowing the operation to utilize the GPU's parallel processing capabilities.

## threadIdx

`threadIdx` is a built-in variable used in GPU programming to identify the index of a specific thread within a block. It allows each thread in a block to independently access elements in data arrays, making parallel processing possible.

For example:

```gyro
var i: int = threadIdx.x
```

Here, `threadIdx.x` gives the unique x-coordinate of the current thread within its block, allowing each thread to process a specific element in an array (such as `A[i]` and `B[i]` in the `VectorAdd` function). In multi-dimensional grids, `threadIdx` can also have `y` and `z` components (e.g., `threadIdx.y`, `threadIdx.z`) for more complex parallel operations.