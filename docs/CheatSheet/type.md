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
List = [Number: _, String]  # [1, 2, ..., "ok"]
Map = {[String]: Number}  # { "length": 1 }
Set = {String, Number}     # { "ok", "good", 1, 2, 3 }

Union = "ok" or "good" or "other"
Format = t"#(Union)_#(Union)_f" # "ok_ok_f", "ok_good_f", ..., "other_other_f"

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
