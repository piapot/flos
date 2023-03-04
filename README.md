# Flos language

A simple script language.

## Comment

### Single-line comment

Single-line comment start with two forward slashes.

```flos
// This is a single-line comment.
```

### File comment

File comment start with two forward slashes and an exclamation mark, and file comment can only be placed in front of the file.

```flos
//! This is a file comment.
//! You can write more lines.
```

### Document comment

Document comment start with two forward slashes and a question mark, and document comment can only be placed in front of the api (such as structure, enumeration or function, etc).

```flos
//? This is a document comment.
//? You can write more lines.
```

## Data Type

### Primitive type

| Type             |                                                                 |
| :--------------- | :-------------------------------------------------------------- |
| Integer          | `Int8`, `Int16`, `Int32`, `Int64`, `Int128`, `Int`              |
| Unsigned integer | `UInt8`, `UInt16`, `UInt32`, `UInt64`, `UInt128`, `UInt`        |
| Float            | `Float16`, `Float32`, `Float64`, `Float80`, `Float128`, `Float` |

### Complex type

- Boolean
- Vector
- Array
- Tuple
- Map
- Set
- Symbol
- Void
- Unknown

### Custom type

- enum
- struct

### Literal

- Integer literal

```flos
-0_9_8_7 // Decimal int
-0xf_e_d // Hex int
-0o7_6_5 // Octal int
-0b1_0_1 // Binary int
```

- Float literal

```flos
-9_8_7.6_5   // Float
-9_8_7.0e+65 // Floating point
```

- String literal

```flos
"string \"\t\r\n\xff\u{ffffff}\\" // Single-line string
r"\t\r\n"                         // Raw string
f"$(1 + 1)"                       // Format string
```

- Vector literal

```flos
[1, 2, 3] // Vector[Int, 3]
```

- Array literal

```flos
[1, 2, 3] // Array[Int]
```

- Tuple literal

```flos
[1, 2, "3"] // Tuple[Int, Int, String]
```

- Map literal

```flos
{ "name": "map", value: 0 } // Map[String, String or Int]
```

- Set literal

```flos
{ "success", "failure", 1, 0 } // Set[String or Int]
```

- Symbol literal

```flos
'symbol' // Symbol
```

## Variables

- Bind immutable variables

```flos
let value = 0
value = 1 // failure!
```

- Bind mutable variables

```flos
mut value = 0
value = 1 // success!
```

## Function

### Example

```flos
fn main(): Int
  return 0
fn;
```

## Enum

### Example

```flos
enum Boolean
  false = 0
  true = 1
enum;

let boolean = Boolean.true // 1

enum Ip
  v4(String) = "v4"
  v6(String) = "v6"
enum;

Ip.v4 // "v4"
Ip.v6 // "v6"

let ip_v4 = Ip.v4("127.0.0.1") // "127.0.0.1"
let ip_v6 = Ip.v6("::1")       // "::1"

mut ip: Ip = .v4 // "v4"
ip = .v6("::1")  // "::1"
```

## Struct

### example

```flos
struct User
  name: String = "user"
  email: String = "user@example.com"
  active: Boolean = .false
struct;

let user_1 = User.{
  name = "user_1"
  email = "user_1@example.com"
  active = .true
}

mut user_2: User = .{}
user_2.active = .true
```
