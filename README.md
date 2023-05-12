# Flos language

A simple script language.

## Comment

### Single-line comment

A single line comment begins with a `;` sign.

```flos
; This is a single-line comment.
```

## Data Type

### Primitive type

1. Integer
2. Unsigned integer
3. Float
4. Boolean

### Literal

- Integer literal

```flos
-0_9_8_7; Decimal int
-0xf_e_d; Hex int
-0o7_6_5; Octal int
-0b1_0_1; Binary int
```

- Float literal

```flos
-9_8_7.6_5; Float
-9_8_7.0e+65; Floating point
```

- Array literal

```flos
[1, 2, 3]; `[...Int]`;
```

- Vector literal

```flos
[1, 2, 3]; `[Int: 3]`;
```

- Tuple literal

```flos
[1, 2, "3"]; `[Int: 2, String]`;
```

- Map literal

```flos
{"name": "map", "value": 0}; `{String: String or Int}`
```

- Set literal

```flos
{"success", "failure", 1, 0}; `{String or Int}`
```

- String literal

```flos
"string \(props.item)\"\t\r\n\xff\u{10ffff}\\"; `[Uint8: _]` String
```

## Assignment

- Bind immutable variable

```flos
const value = false;
value = true; failure!
```

- Bind mutable variable

```flos
let value = false;
value = true; success!
```

## Enum

### Enum Example

```flos
type Ip = {
  v4(String) = "v4",
  v6(String) = "v6",
}

Ip.v4; "v4"
Ip.v6; "v6"

const ip_v4 = Ip.v4("127.0.0.1"); "127.0.0.1"
const ip_v6 = Ip.v6("::1"); "::1"

let ip: Ip = .v4; "v4"
ip = .v6("::1"); "::1"
```

## Struct

### Struct Example

```flos
type String = {
  value: [...Uint] = [],
  length: Uint = 0,
};

type User = {
  name: String = "user",
  email: String = "user@example.com",
  active: Bool = false,
};

const user_1 = User.{
  name: "user_1",
  email: "user_1@example.com",
  active: true,
};

let user_2: User = .{};
user_2.active = true;
```

## Interface

### Interface Example

```flos
type String = [...Uint8];

type StringAble = {
  fromUint8(value: String) -> String,
  toString() -> String,
};

expand StringAble:
  @export function fromUint8(value: String) -> String:
    ; ...
  function;

  @export function toString() -> String:
    ; ...
  function;
expand;
```

## Function

### Function Example

```flos
type {Io} = Global;

@export function main():
  const value = false;
  const value_pointer: Pointer(Boolean) = value.&; `0x7ffd0d8e29fc`
  const value_pointer_value: Boolean = value_pointer.*; `false`

  let failure_value_pointer_1 = value.&; failure!
  let failure_value_pointer_2 = value_pointer; failure!

  let success_value_pointer_value_1 = value; clone success!
  let success_value_pointer_value_2 = value_pointer_value; clone success!

  let new_value = value_pointer_value; `false`
  new_value_pointer.* = true
  new_value_pointer_value = $new_value_pointer.*; `true`

  const {output} = Io;

  output(value); `false`;
  output(value_pointer); `0x7ffd0d8e29fc`
  output(value_pointer_value); `false`

  output($new_value); `true`;
  output($new_value_pointer); `0x6b3f1a8d6c0f`
  output($new_value_pointer_value); `true`

  const x = 1;
  const closureFn() = x;
  const doubleFn(x: Int) -> Int = x * x;
function;
```

## Expand

### Expand Example

```flos
type {Io} = Global;

@export type String = {
  value: Array[...Uint8] = [],
  length: Uint = 0,
};

expand String:
  const it = @it();

  @export function new(value: Array[...Uint8]) -> String:
    return String.{ value: value };
  function;

  @export function toBool() -> Boolean:
    return match it:
    => "": false;
    => _: true;
    match;
  function;

  @export function toInt() -> Int:
    return it.length;
  function;
expand;

const string = String.new("ok")
const bool = string.toBool()
const int = string.toInt()

Io.output(string) # "ok"
Io.output(bool)   # `true`
Io.output(int)    # `2`
```
