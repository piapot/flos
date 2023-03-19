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

|                  | Name                                                            |
| :--------------- | :-------------------------------------------------------------- |
| Integer          | `Int8`, `Int16`, `Int32`, `Int64`, `Int128`, `Int`              |
| Unsigned integer | `UInt8`, `UInt16`, `UInt32`, `UInt64`, `UInt128`, `UInt`        |
| Float            | `Float16`, `Float32`, `Float64`, `Float80`, `Float128`, `Float` |
| Boolean          | `Bool`                                                          |

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

- Symbol literal

```flos
'symbol # Symbol
```

- String literal

```flos
"string \"\t\r\n\xff\u{10ffff}\\" # `[UInt8: 19]` Single-line string
#"\t\r\n"#                        # `[UInt8: 3]` Raw string
"#(props.item)"                  # `[[UInt8: 0], [Unknown...]]` Template string
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

# Enumeration

## Example

```flos
Ip = {
  v4(String) = "v4",
  v6(String) = "v6",
}

Ip.v4 # "v4"
Ip.v6 # "v6"

ip_v4 = Ip.v4("127.0.0.1") # "127.0.0.1"
ip_v6 = Ip.v6("::1")       # "::1"

$ip: Ip = .v4 # "v4"
$ip = .v6("::1")  # "::1"
```

---

# Structure

## Example

```flos
String = {
  value: [UInt...] = [],
  length: UInt = 0,
}

User = {
  name: String = "user",
  email: String = "user@example.com",
  active: Bool = false,
}

user_1 = User.{
  name: "user_1",
  email: "user_1@example.com",
  active: true,
}

$user_2: User = .{}
$user_2.active = true
```

---

# Function

## Example

```flos
{Io, pointer} = Global

#[pointer]
fn main()
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
  fn double_fn_2(x: Int): Int return x * x fn;
fn;
```

---

# Extend

## Example

```flos
{Io, self} = Global

String = {
  value: Array[UInt8...] = [],
  length: UInt = 0,
}

extend String
  $self = @self()

  fn new(value: Array[UInt8...]): String
    return String.{ value: value }
  fn;

  fn toBool(): Bool
    return when $self
      case "" false
      else true
    when;
  fn;

  fn toInt(): Int
    return $self.length
  fn;
extend;

string = String.new("ok")
bool = string.toBool()
int = string.toInt()

Io.output(string) # "ok"
Io.output(bool)   # `true`
Io.output(int)    # `2`
```
