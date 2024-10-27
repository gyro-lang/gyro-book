# Stack

Stack is the official package manager included with your Gyro installation. It’s language-agnostic, meaning it supports multiple programming languages, just like Gyro. For instance, to install the **leftpad** package from NPM, simply run:

```sh
stack npm i leftpad
```

This command fetches the latest version of **leftpad** from the npm registry, installs it in the local **.stack** directory, and adds it to the **dependencies** field in your `stack.json` file.

Once installed, you can import it into Gyro as follows:

```gyro
import * as leftpad from "leftpad"
import { println } from "@gyro/stdlib"

fn main() {
  var x = "Hello, World!"

  x = leftpad(x, 2, " ")

  println(x)
}
```



## Registries

Stack also supports the following package registries:

- **PyPI** for Python packages
- **Cargo** for Rust packages
- **RubyGems** for Ruby packages
- **Maven** for Java packages
- **NuGet** for .NET packages

For each registry, you can use the familiar syntax to install packages directly. For example:

```sh
stack pypi install requests  # for Python packages
stack cargo install serde    # for Rust packages
stack gem install rails      # for Ruby packages
stack mvn install log4j      # for Java packages
stack nuget install Newtonsoft.Json  # for .NET packages
```

These commands will fetch the latest versions from the respective registries, install them to the local **.stack** directory, and update the **dependencies** field in `stack.json`.

Stack also hosts its own registry, providing a centralized source for packages specifically curated for Gyro and other supported languages. This registry allows developers to publish and share packages directly compatible with Stack, streamlining dependency management across projects. You can easily search and install packages from the Stack registry using commands like:

```sh
stack install <package-name>
```

Packages from the Stack registry will be added to the local **.stack** directory and listed in the **dependencies** field of `stack.json`, just like packages from other registries.

For more information on Stack, please visit [the official stack website](https://pkgstack.com)