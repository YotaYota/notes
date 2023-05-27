# FlatBuffers

It represents hierarchical data in a flat binary buffer in such a way that it
can still be accessed directly without parsing/unpacking, while also still
supporting data structure evolution (forwards/backwards compatibility).

- `namespace` - determines the corresponding package/namespace for the generated
  code
- `enum`
- `union` - union of tables
- `struct` - structs are ideal for data structures that will not change, since
  they use less memory and have faster lookup
- `table`
- `root_type` - The root type declares what will be the root table for the
  serialized data.

Since you cannot delete fields from a table (to support backwards compatability)
, you can set fields as deprecated, which will prevent the generation of
accessors for this field in the generated code. Be careful when using deprecated
, however, as it may break legacy code that used this accessor.

##

1. Write fbs schema
2. Compile schema, eg
```sh
flatc --ts my_schema.fbs
```
3. Import compiled files and `FlatBufferBuilder`
```
let builder = new flatbuffers.Builder(1024);
```
