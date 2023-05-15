# Flos language

A simple script language.

## Comment

### Single-line comment

A single line comment begins with a `;` sign.

- Grammar

```rs
single_line_comment = {
  ";" ~ (ANY ! "\n")*
}
```

- Example

```flos
; This is a single-line comment.
```

## Data Type

### Primitive type

1. Integer
2. Unsigned integer
3. Float
4. Boolean

### Integer Literal

- Grammar

```rs
decimal_integer = ("+" | "-")? ~ "0" | NONZERO_DIGIT ~ ("_"? ~ DIGIT)*;
decimal_scientific_notation = decimal_integer ~ ("e" | "E") ~ ("+" | "-")? ~ DIGIT;
hex_integer = ("+" | "-")? ~ "0x" ~ HEX_DIGIT ~ "_"? ~ (HEX_DIGIT)*;
octonary_integer = ("+" | "-")? ~ "0o" ~ OCTONARY_DIGIT ~ ("_"? ~ OCTONARY_DIGIT)*;
binary_integer = ("+" | "-")? ~ "0b" ~ BINARY_DIGIT ~ ("_"? ~ BINARY_DIGIT)*;
```

- Example

```flos
-9_8_7; Decimal integer
-9_8_7e+12; Decimal scientific notation
-0xf_e_d; Hex integer
-0o7_6_5; Octonary integer
-0b1_0_1; Binary integer
```

### Float literal

- Grammar

```rs
float = ("+" | "-")? ~ _integer ~ "." ~ _integer;
float_scientific_notation = float ~ ("e" | "E") ~ ("+" | "-") ~ _integer;
_integer = DIGIT ~ ("_"? ~ DIGIT)*;
```

- Example

```flos
-9_8_7.6_5; Float
-9_8_7.0e+65; Float scientific notation
```

### Array literal

- Grammar

```rs
array = "[" ~ "]" | "[" ~ expression ~ ("," ~ expression)* ~ ","? ~"]";
```

- Example

```flos
[1, 2, 3]; `[Int: 3]`
```

### Vector literal

- Grammar

```rs
vector = "[" ~ "]" | "[" ~ expression ~ ("," ~ expression)* ~ ","? ~"]";
```

- Example

```flos
[1, 2, 3]; `[Int...]`
```

### Tuple literal

- Grammar

```rs
tuple = "[" ~ "]" | "[" ~ expression ~ ("," ~ expression)* ~ ","? ~ "]";
```

- Example

```flos
[1, 2, "3"]; `[Int: 2, String]`
```

### Set literal

- Grammar

```rs
set = "{" ~ "}" | "{" ~ expression ~ ("," ~ expression)* ~ ","? ~ "}";
```

- Example

```flos
{"success", "failure", 1, 0}; `{String or Int}`
```

### Map literal

- Grammar

```rs
map = "{" ~ "}" | "{" ~ _pair ~ ("," ~ _pair)* ~ ","? ~ "}";
_pair = string ~ ":" ~ expression;
```

- Example

```flos
{"name": "map", "value": 0}; `{String: String or Int}`
```

### String literal

- Grammar

```rs
string = "\"" ~ _inner ~ "\"";
_inner =
  (ANY ! "\"" | "\\") |
  "\\" ~ ("\"" | "\\" | "/" | "b" | "f" | "t" | "r" | "n") |
  "\\" ~ ("u" ~ HEX_DIGIT{4}) |
  "\\" ~ "(" ~ expression ~ ")"
;
```

- Example

```flos
"string \(props.item)\"\t\r\n\xff\u{10ffff}\\"; `[Uint8: _]` String
```

## Assignment

### Bind immutable variable

- Grammar

```rs
const_declare = "const" ~ "=" ~ expression ~ comment;
```

- Example

```flos
const value = false;
value = true; failure!
```

### Bind mutable variable

- Grammar

```rs
let_declare = "let" ~ "=" ~ expression ~ comment;
```

- Example

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
};

Ip.{v4}; "v4"
Ip.{v6}; "v6"

const ip_v4 = Ip.{v4}("127.0.0.1"); "127.0.0.1"
const ip_v6 = Ip.{v6}("::1"); "::1"

let ip: Ip = .{v4}; "v4"
ip = .{v6}("::1"); "::1"
```

## Struct

### Struct Example

```flos
type String = {
  value: [Uint8: _] = [],
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
type StringAble = {
  fromUint8(value: String) -> String,
  toString() -> String,
};

expand StringAble:
  @export function from(value: Unknown) -> String:
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
  new_value_pointer_value = new_value_pointer.*; `true`

  const {output} = Io;

  output(value); `false`;
  output(value_pointer); `0x7ffd0d8e29fc`
  output(value_pointer_value); `false`

  output(new_value); `true`;
  output(new_value_pointer); `0x6b3f1a8d6c0f`
  output(new_value_pointer_value); `true`

  const x = 1;
  const closureFn() = x;
  const doubleFn(x: Int) -> Int = x * x;
function;
```

## Expand

### Expand Example

```flos
type {Io} = Global;

@export type MyBoolean = {
  false = 0,
  true = 1,
};

@export type MyBooleanAble = {
  toString() -> String,
};

expand MyBoolean:
  combine MyBooleanAble;

  type It = @It();
  const it = @it();

  @export function new(value: Boolean = false) -> It:
    return match value:
    => false: .false;
    => true: .true;
    match;
  function;

  @export function toString() -> String:
    return match it:
    => .false: "false";
    => .true: "true";
    match;
  function;
expand;

const myBoolean = MyBoolean.new(true);
const str = myBoolean.toString();

Io.output(myBoolean); `MyBoolean.{true}`
Io.output(str); `true`
```
