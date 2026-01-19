# Sindarin Language Support for VS Code

This extension provides comprehensive language support for the **Sindarin** programming language, a statically-typed procedural language that compiles to C.

## Development & Testing

### Quick Start (Extension Development Host)

1. Open this folder in VS Code:
   ```bash
   code /path/to/sindarin-vscode
   ```

2. Press `F5` to launch the Extension Development Host

3. In the new window, open any `.sn` file to test syntax highlighting and snippets

### Install Locally via VSIX

```bash
# Install packaging tool (once)
npm install -g @vscode/vsce

# Package the extension
cd /path/to/sindarin-vscode
vsce package

# Creates sindarin-0.1.0.vsix
# In VS Code: Extensions > ... > Install from VSIX...
```

### Symlink Installation (Alternative)

```bash
ln -s /path/to/sindarin-vscode ~/.vscode/extensions/sindarin
# Restart VS Code
```

## Features

### Syntax Highlighting

Full syntax highlighting support including:

- **Keywords**: `fn`, `native`, `var`, `struct`, `import`, `type`, `if`, `else`, `while`, `for`, `return`, `break`, `continue`, `panic`
- **Modifiers**: `shared`, `private`, `sync`, `as val`, `as ref`
- **Types**: Primitives (`int`, `long`, `double`, `str`, `bool`, `char`, `byte`, `void`, `any`) and built-in types (`TextFile`, `Date`, `Time`, `UUID`, etc.)
- **Operators**: Arrow blocks (`=>`), range (`..`), spread (`...`), thread spawn (`&`), sync (`!`), type check (`is`), type cast (`as`)
- **String interpolation**: `$"Hello, {name}!"` with format specifiers like `{pi:.2f}`, `{num:x}`, `{count:05d}`
- **Multi-line strings**: Both `$"..."` spanning lines and `$|` block syntax
- **Pragmas**: `#pragma include`, `#pragma link`, `#pragma source`, `#pragma pack`
- **Comments**: Single-line `//`, hash comments `#`, and block `/* */`
- **Shared loops**: `shared for`, `shared while` for arena-efficient iteration
- **Any type**: `typeof`, `is` type checking, `as` type casting
- **Decorators**: `@alias`, `@source`, `@include`, `@link` for C interop
- **Static methods**: `static fn` inside struct definitions
- **Special variables**: `self` (current instance), `arena` (memory arena)

### Code Snippets

Over 75 snippets for common patterns:

| Prefix | Description |
|--------|-------------|
| `main` | Main function entry point |
| `fn`, `fne`, `staticfn` | Function declarations |
| `fnshared`, `fnprivate` | Memory-annotated functions |
| `native`, `nativefn` | Native function declaration |
| `struct`, `structd` | Struct declaration |
| `nativestructref`, `sdkstruct` | SDK-style native structs |
| `var`, `varref`, `varval`, `varsize` | Variable declarations |
| `if`, `ife`, `ifeif` | Conditional statements |
| `for`, `foreach`, `while` | Loop statements |
| `sharedfor`, `sharedwhile` | Shared arena loops |
| `print`, `println`, `printf` | Print statements |
| `lambda`, `lambdam` | Lambda expressions |
| `spawn`, `spawnsync`, `lock` | Threading primitives |
| `import`, `importas` | Module imports |
| `@alias`, `@source`, `@include`, `@link` | Decorators for C interop |
| `anyis`, `typeof` | Any type operations |
| `self`, `arena` | Special variables |

Type `Ctrl+Space` after a snippet prefix to see all available snippets.

### Language Configuration

- Bracket matching and auto-closing
- Comment toggling (`Ctrl+/`)
- Code folding for functions, structs, and blocks
- Indentation rules for arrow blocks (`=>`)

### File Icons

Custom file icons for `.sn` files in both light and dark themes.

## Example Code

```sindarin
import "utils" as util

struct Point =>
    x: double
    y: double

fn distance(a: Point, b: Point) shared: double =>
    var dx: double = b.x - a.x
    var dy: double = b.y - a.y
    return sqrt(dx * dx + dy * dy)

fn main(): void =>
    var p1: Point = Point { x: 0.0, y: 0.0 }
    var p2: Point = Point { x: 3.0, y: 4.0 }

    // String interpolation with format specifiers
    var dist: double = distance(p1, p2)
    print($"Distance: {dist:.2f}\n")

    // Threading example
    var result: int = &compute(42)
    // ... do other work ...
    result!  // sync
    print($"Result: {result}\n")

    // Shared loop for efficiency
    var sum: int = 0
    shared for var i: int = 0; i < 100; i++ =>
        sum = sum + i

    // Any type with type checking
    var value: any = 42
    if value is int =>
        var n: int = value as int
        println($"Got integer: {n}")
```

### SDK-Style C Interop Example

```sindarin
# Include C headers and link libraries
@include <math.h>
@link m

# Bind to C math functions
native fn sqrt(x: double): double
native fn pow(base: double, exp: double): double

@alias "sinf"
native fn sinF(x: float): float

# SDK-style struct with static factory and methods
@alias "RtDate"
native struct Date as ref =>
    @alias "days"
    _days: int32

    static fn today(): Date =>
        return sn_date_today(arena)

    @alias "sn_date_get_year"
    native fn year(): int

    fn addDays(days: int): Date =>
        return sn_date_add_days(arena, self, days)
```

## Language Overview

Sindarin is a statically-typed procedural programming language with:

- **Arrow-based syntax** (`=>`) for clean, indentation-based blocks
- **String interpolation** with `$"..."` and multiline `$|` blocks
- **Arena-based memory management** with `shared` and `private` modifiers
- **First-class functions** and closures
- **OS-level threading** with `&` spawn and `!` sync operators
- **C interoperability** via `native` functions and pragma directives
- **Built-in types** for file I/O, networking, dates, and more

For full language documentation, see the [Sindarin Language Documentation](https://github.com/sindarin/sindarin-compiler/docs).

## Configuration

This extension automatically sets the following defaults for Sindarin files:

```json
{
  "editor.tabSize": 4,
  "editor.insertSpaces": true,
  "editor.detectIndentation": false
}
```

## Supported File Extensions

- `.sn` - Sindarin source files

## Contributing

Contributions are welcome! Please see the [GitHub repository](https://github.com/sindarin/sindarin-vscode) for details.

## License

MIT License

## Release Notes

### 0.1.0

- Initial release
- Syntax highlighting for all language features
- 50+ code snippets
- Language configuration with bracket matching and folding
- Custom file icons
