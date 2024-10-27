# Decorators

Decorators are a unique feature in Gyro, not commonly found in other languages. Although they may resemble Python’s decorators, in Gyro, they can be applied to function declarations, generic code blocks, or even variable declarations. Decorators modify or enhance the behavior of the associated code element.

For example:

```gyro
import { comptime } from "@gyro/stdlib"

fn fibonacci(n: int): int {
  if n <= 0 {
    return 0
  } elif n == 1 {
    return 1
  }
  
  return fibonacci(n - 1) + fibonacci(n - 2)
}

@comptime var result = fibonacci(10)
// This is equivalent to:
// var result = 34
```

In this case, the `@comptime` decorator ensures that the `fibonacci(10)` calculation is performed at compile time, so the variable `result` is directly assigned the value `34`. Decorators like `@comptime` can significantly optimize your code by allowing computations to happen during the compilation phase, leading to faster execution at runtime.

Decorators operate by defining functions that are executed during compilation, receiving the expression's abstract syntax tree (AST) as an argument. All decorator routines are run in a separate thread from the main program, allowing for parallel execution. These functions can modify the AST, compile source code, or execute additional logic—offering virtually limitless possibilities!

## Defining a Generic Compile-Time Decorator

In Gyro, you can create a custom decorator that evaluates expressions during compile time. Here's an example of a generic decorator that sums variables at compile time:

```gyro
import { ast } from "@gyro/std"

fn addition(decl: *ast::VariableDeclaration): *ast::Literal throws Error {
  if len(decl.declarations) == 0 || decl.declarations[0] isnot ast::BinaryExpression {
    throw Error{"Expected a binary expression as the variable declarator"}
  }

  var declaration = decl.declarations[0]

  if ast::castable<int>(declaration.left) && ast::castable<int>(declaration.right) && declaration.operator == "+" {
    return ast::Literal{
      value: ast::cast<int>(declaration.left) + ast::cast<int>(declaration.right)
    }
  }
}
```

This decorator checks if the variable is being initialized using a binary expression with the addition operator and integers on both sides. If so, it calculates the sum during compilation.

You can use the `@addition` decorator as follows:

```gyro
@addition var x = 4 + 2 // var x = 6
```

In this example, the `@addition` decorator computes `4 + 2` at compile time, so the variable `x` is directly initialized with the value `6`.

<div class="warning">
Important: All decorator functions must be defined and recognized by the compiler before being used in the source code. Failing to do so may cause parser extensions to malfunction and potentially break the entire parser.
</div>

## Syntax and Parser Extensions

One of Gyro’s most powerful features is the ability to define custom syntax
extensions using the META Compiler Toolchain and basic METALS intrinsics.

Here’s an example of how you can implement a custom syntax extension that
recognizes greetings:

```gyro
import { ast } from "@gyro/ast"
import { metals } from "@gyro/metals"

syntaxfn greeting(ast: *ast::BlockStatement): *(*metals::Literal)[] throws Error contributes {
  entry Program;

  rule Program = Greeting*;

  rule Greeting = _ "Hello" _ Name {
    return Node{
      type: "Greeting",
      name: Name[0]
    }
  };

  token Name = [A-Za-z]+ {
    return $
  };
} {
  // Ensure we've collected some greetings
  if len(ast.body) == 0 {
    throw Error{"No greetings found!"}
  }

  var arr: (*metals::Literal)[0,] = []
  for var i = 0; i < len(ast.body); i++ {
    var literal = metals::Literal{
      value: ast.body[i].name
    }

    arr = append(arr, &literal)
  }

  return &arr
}

var names = @greeting {
  Hello John
  Hello William
}
```

### Explanation

- The `syntaxfn` defines a custom syntax that detects the word "Hello" followed by a name. It then captures the names and compiles them into an array of literals.
- Inside the `greeting` syntax function, the grammar rules specify the structure of the program, where a greeting consists of "Hello" followed by a valid name (composed of alphabetic characters).
- The `greeting` block ensures that the captured names are processed and converted into literals during compilation.

By using the META Compiler Toolchain and METALS intrinsics, you can extend the syntax of Gyro to suit your needs, enabling highly customizable and expressive syntax tailored to your domain.

## Passing arguments to decorators

In case a decorator needs to configure things before executing, arguments can be passed to it like they can be to any other function or method.

```gyro
import { concurrent, Mutex } from "@gyro/stdlib"

@concurrent(threads = 8) fn calculate() {
  var x = Mutex{ 5 }

  for var i = 0; i < 100; i++ {
    // Get a lock on the mutex
    var guard = match x.lock() {
      Ok(guard): guard,
      Err(poisoned): poisoned.into_inner()
    }
  }
}
```