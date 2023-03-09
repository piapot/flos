# Type

A type is like a variable, but it defines the type.

- Array
- Vector
- Tuple
- List
- Map
- Set
- enum
- struct

```flos
type Array = [Number: 3]         // [1, 2, 3]
type Vector = [Number: _]        // [1, 2, 3, ...]
type Tuple = [Number: 3, String] // [1, 2, 3, "ok"]
type List = [Number: _, String]  // [1, 2, ..., "ok"]
type Map = { [String]: Number }  // { "length": 1 }
type Set = { String, Number}     // { "ok", "good", 1, 2, 3 }

type Union = "ok" or "good" or "other"
type Format = t"#(Union)_#(Union)_f" // "ok_ok_f", "ok_good_f", ..., "other_other_f"

type Enum = {
  name(String) = "enum",
  length(Number) = 2,
}

type Struct = {
  name: String = "struct",
  length: Number = 2,
}

type Func = {
  
}
```
