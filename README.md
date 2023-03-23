# Flos language

A simple script language.

---

# Comment

## Single-line comment

A single line comment begins with a `#` sign.

```flos
# This is a single-line comment.
```

## File comment

File comment start with a `#!` mark, and file comment can only be placed in front of the file.

```flos
#! This is a file comment.
#! You can write more lines.
```

## Document comment

Document comment start with a `#?`, and document comment can only be placed in front of the api (such as structure, enumeration or fn, etc).

```flos
#? This is a document comment.
#? You can write more lines.
```

---

# Data Type

## Primitive type

| Type             | Name      |
| :--------------- | :-------- |
| Integer          | `Int`     |
| Unsigned integer | `Uint`    |
| Float            | `Float`   |
| Boolean          | `Boolean` |

## Literal

- Integer literal

```flos
-0_9_8_7 # Decimal int
-0xf_e_d # Hex int
-0o7_6_5 # Octal int
-0b1_0_1 # Binary int
```

- Float literal

```flos
-9_8_7.6_5   # Float
-9_8_7.0e+65 # Floating point
```

- Array literal

```flos
[1, 2, 3] # `[Int...]`
```

- Vector literal

```flos
[1, 2, 3] # `[Int: 3]`
```

- Tuple literal

```flos
[1, 2, "3"] # `[Int: 2, String]`
```

- Map literal

```flos
{ "name": "map", "value": 0 } # `{String: String or Int}`
```

- Set literal

```flos
{ "success", "failure", 1, 0 } # `{String or Int}`
```

- String literal

```flos
"string \"\t\r\n\xff\u{10ffff}\\" # `[Uint8: 19]` Single-line string
#"\t\r\n"#                        # `[Uint8: 3]` Raw string
"#(props.item)"                   # `[[Uint8: 0], [Unknown...]]` Template string
```

---

# Assignment

- Bind immutable variable

```flos
value = false
value = true # failure!
```

- Bind mutable variable

```flos
$value = false
$value = true # success!
```

---

# Enum

## Example

```flos
enum Ip
  v4(String) = "v4",
  v6(String) = "v6",
enum;

Ip.v4 # "v4"
Ip.v6 # "v6"

ip_v4 = Ip.v4("127.0.0.1") # "127.0.0.1"
ip_v6 = Ip.v6("::1")       # "::1"

$ip: Ip = .v4     # "v4"
$ip = .v6("::1")  # "::1"
```

---

# Struct

## Example

```flos
struct String
  value: [Uint...] = [],
  length: Uint = 0,
struct;

struct User
  name: String = "user",
  email: String = "user@example.com",
  active: Bool = false,
struct;

user_1 = User.{
  name: "user_1",
  email: "user_1@example.com",
  active: true,
}

$user_2: User = .{}
$user_2.active = true
```

---

# Interface

## Example

```flos
{public} = import.Lib

String = [Uint8...]

interface StringAble
  fromUint8(value: String): String,
  toString(): String,
interface;

extend StringAble
  @[public]
  function fromUint8(value: String): String
    # ...
  function;

  @[public]
  function toString(): String
    # ...
  function;
extend;
```

---

# Function

## Example

```flos
{Io, public, pointer} = import.Lib

@[public, pointer]
function main()
  value = false
  value_pointer: Pointer[Bool] = value.&      # `0x7ffd0d8e29fc`
  value_pointer_value: Bool = value_pointer.* # `false`

  $failure_value_pointer_1 = value.&       # failure!
  $failure_value_pointer_2 = value_pointer # failure!

  $success_value_pointer_value_1 = value               # clone success!
  $success_value_pointer_value_2 = value_pointer_value # clone success!

  $new_value = value_pointer_value # `false`
  $new_value_pointer.* = true
  $new_value_pointer_value = $new_value_pointer.* # `true`

  { output } = Io

  output(value)               # `false`
  output(value_pointer)       # `0x7ffd0d8e29fc`
  output(value_pointer_value) # `false`

  output($new_value)               # `true`
  output($new_value_pointer)       # `0x6b3f1a8d6c0f`
  output($new_value_pointer_value) # `true`

  x = 1
  closureFn() = x
  double_fn_1(x: Int): Int = x * x
function;
```

---

# Extend

## Example

```flos
{Io, public, self} = import.Lib

@[public]
struct String
  value: Array[Uint8...] = [],
  length: Uint = 0,
struct;

extend String
  Self = @self()

  @[public]
  function new(value: Array[Uint8...]): String
    return String.{ value: value }
  function;

  @[public]
  function toBool(self: Self): Bool
    return when self
      case "" false
      else true
    when;
  function;

  @[public]
  function toInt(self: Self): Int
    return self.length
  function;
extend;

string = String.new("ok")
bool = string.toBool()
int = string.toInt()

Io.output(string) # "ok"
Io.output(bool)   # `true`
Io.output(int)    # `2`
```
