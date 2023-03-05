# Flos language

A simple script language.

---

# Comment

## Single-line comment

Single-line comment start with two forward slashes.

```flos
// This is a single-line comment.
```

## File comment

File comment start with two forward slashes and an exclamation mark, and file comment can only be placed in front of the file.

```flos
//! This is a file comment.
//! You can write more lines.
```

## Document comment

Document comment start with two forward slashes and a question mark, and document comment can only be placed in front of the api (such as structure, enumeration or function, etc).

```flos
//? This is a document comment.
//? You can write more lines.
```

---

# Data Type

## Primitive type

| Type             |                                                                 |
| :--------------- | :-------------------------------------------------------------- |
| Integer          | `Int8`, `Int16`, `Int32`, `Int64`, `Int128`, `Int`              |
| Unsigned integer | `UInt8`, `UInt16`, `UInt32`, `UInt64`, `UInt128`, `UInt`        |
| Float            | `Float16`, `Float32`, `Float64`, `Float80`, `Float128`, `Float` |
| Boolean          | `false`,`true`                                                  |

## Complex type

- Array
- Map
- Symbol

## Custom type

- enum
- struct

## Literal

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

- Array literal

```flos
[1, 2, 3] // `Array[Int]`
```

- Vector literal

```flos
[1, 2, 3] // `Array[Int: 3]`
```

- Tuple literal

```flos
[1, 2, "3"] // `Array[Int: 2, String]`
```

- Map literal

```flos
{ "name": "map", "value": 0 } // `Map[String, String or Int]`
```

- Set literal

```flos
{ "success", "failure", 1, 0 } // `Map[String or Int]`
```

- Symbol literal

```flos
'symbol' // Symbol
```

- String literal

```flos
"string \"\t\r\n\xff\u{10ffff}\\" // `Array[UInt8: 15]` Single-line string
r"\t\r\n"                         // `Array[UInt8: 3]` Raw string
f"$(1 + 1)"                       // `Array[UInt8: 1]` Format string
t"$(props.item)"                  // `Array[Array[UInt8: 8], Array[Unknown]]` Template string
```

---

# Assignment

- Bind immutable variable

```flos
let value = false
value = true // failure!
```

- Bind mutable variable

```flos
mut value = false
value = true // success!
```

---

# Enum

## Example

```flos
enum Ip
  v4(String) = "v4",
  v6(String) = "v6",
enum;

Ip.v4 // "v4"
Ip.v6 // "v6"

let ip_v4 = Ip.v4("127.0.0.1") // "127.0.0.1"
let ip_v6 = Ip.v6("::1")       // "::1"

mut ip: Ip = .v4 // "v4"
ip = .v6("::1")  // "::1"
```

---

# Struct

## Example

```flos
struct User
  name: String = "user",
  email: String = "user@example.com",
  active: Boolean = .false,
struct;

let user_1 = User.{
  name = "user_1",
  email = "user_1@example.com",
  active = .true,
}

mut user_2: User = .{}
user_2.active = .true
```

---

# Function

## Example

```flos
let { usePointer } = @use("flos:pointer")

@[usePointer]
pub fn main(): Int
  type Io = @use("flos:io")

  let output: Io = .output

  let value = false
  let value_pointer: Pointer[Boolean] = value.&      // `0x7ffd0d8e29fc`
  let value_pointer_value: Boolean = value_pointer.* // `false`

  mut failure_value_pointer_1 = value.&       // failure!
  mut failure_value_pointer_2 = value_pointer // failure!

  mut success_value_pointer_value_1 = value               // clone success!
  mut success_value_pointer_value_2 = value_pointer_value // clone success!

  mut new_value = value_pointer_value // `false`
  new_value_pointer.* = true
  mut new_value_pointer_value = new_value_pointer.* // `true`

  output(value)                   // `false`
  output(value_pointer)           // `0x7ffd0d8e29fc`
  output(value_pointer_value)     // `false`

  output(new_value)               // `true`
  output(new_value_pointer)       // `0x6b3f1a8d6c0f`
  output(new_value_pointer_value) // `true`

  return 0
fn;

fn double(x) = x * x

let anonymousDoubleFn1 = fn(x) = x * x
let anonymousDoubleFn2 = fn(x) return x fn;
```
