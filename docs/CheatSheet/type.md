# Type

A type is like a variable, but it defines the type.

- Array
- Vector
- Tuple
- List
- Map
- Set
- Union
- Format
- Enumeration
- Structure
- Interface

```flos
Array = [Number...]         # [1, 2, 3, ...]
Vector = [Number: 3]        # [1, 2, 3]
Tuple = [Number: 3, String] # [1, 2, 3, "ok"]
List = [Number..., String]  # [1, 2, ..., "ok"]
Map = {String: Number}      # { "length": 1 }
Set = {String or Number}      # { "ok", "good", 1, 2, 3 }

typeUnion = "ok" or "good" or "other"
typeFormat = "#(Union)_#(Union)" # "ok_ok", "ok_good", ...,

Enumeration = {
  name(String) = "enum",
  length(Number) = 2,
}

Structure = {
  name: String = "struct",
  length: Number = 2,
}

Interface = {
  getNumber(): Number = 1,
  getString(): String = "ok",
}
```
