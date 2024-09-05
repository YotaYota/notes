# Bevy engine

## Minimal Implementation

*Cargo.toml*

```toml
[dependencies]
bevy = { version = "0.8.1", features = ["dynamic"] }
```

**Note**: Remove `features = ["dynamic"]` before release.

*main.rs*

```rust
use bevy::prelude::*;

fn main() {
    App::new().run();
}
```

## ECS

### Entities

structs containing unique integer

```rust
struct Entity(u64);
```

### Components

structs implementing `Component` trait.

```rust
#[derive(Component)]
struct Position {
    x: f32,
    y: f32,
}
```

### Systems

functions.

```rust
fn print_position_system(query: Query<&Transform>) {
    for transform in qury.iter() {
        println!("position {:?}", transform.translation);
    }
}
```

`add_system()` adds to App's `Schedule`.

#### Startup Systems

Like **systems**, but run only once.

`add_startup_system()`

##

- `World`
- `Schedule`
- `Commands`
