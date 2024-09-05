# Protocol Buffers

Protocol Buffers us defined by a *.proto* text file, eg

```proto3
syntax = "proto3";

message MyMessage {
  int32 id = 1;
  string first_name = 2;
  bool is_validated = 3;
}
```

- Fully typed data
- Data is compressed automatically
- Schema (*.proto* file) is needed to generate code and read data
- Documentation can be embedded in the schema
- Data can be read across many language
- Schema can evolve over time in a safe manner
- Fast and small
- Code generated automatically
