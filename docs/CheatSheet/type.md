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
Union = "ok" or "good" or "rst"
Format = "#(Union)_#(Union)" # "ok_ok", "ok_good", "ok_rst", ...

enum Enum
  name(String) = "enum",
  length(Number) = 2,
enum;

struct Struct
  name: String = "struct",
  length: Number = 2,
struct;

interface Interface
  getNumber(): Number = 1,
  getString(): String = "ok",
interface;
```
