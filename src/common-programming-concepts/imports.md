# Imports

Gyro natively supports imports using [Stack](/getting-started/stack.md) as its package manager.

With import statements, you can either bring specific variables or functions from another module into the current scope or import the entire module as a single named object. Here’s how:

```gyro
import { X, Y } from "./lib"       // Imports specific variables or functions

import * as lib from "./lib"        // Imports the whole module as 'lib'

import * as lib, { X } from "./lib" // Imports the module as 'lib' and also brings in 'X' separately
```

This flexibility allows you to organize and access modules efficiently within Gyro projects.