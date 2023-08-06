# Rust Web App

- Diesel
- Rocket

## Diesel

### Install diesel cli

```sh
cargo install diesel_cli --no-default-features --features postgres
```

Add `DATABASE_URL` in `.env` file.

Setup and create database

```sh
diesel setup
```

will create `diesel.toml` and a pointer to `src/schema.rs` (maintained by diesel).

Create (empty) migration files

```sh
diesel migration generate create_posts
```

which updates `src/schema.rs`.

Populate `up.sql` and `down.sql` manually

Apply migration

```sh
diesel migration run
```

Roll back migration

```sh
diesel migration redo
```

---

Create connection

```rust
use diesel::pg::PgConnection;
use diesel::prelude::*;
use dotenvy::dotenv;
use std::env;

pub fn establish_connection() -> PgConnection {
    dotenv().ok();

    let database_url = env::var("DATABASE_URL").expect("DATABASE_URL must be set");
    PgConnection::establish(&database_url)
        .unwrap_or_else(|_| panic!("Error connecting to {}", database_url))
}
```

Create model, eg

```rust
use diesel::prelude::*;

#[derive(Queryable, Selectable)]
#[diesel(table_name = crate::schema::posts)]
#[diesel(check_for_backend(diesel::pg::Pg))]
pub struct Post {
    pub id: i32,
    pub title: String,
    pub body: String,
    pub published: bool,
}
```

- `#[derive(Queryable)]` will generate all of the code needed to load a Post struct from a SQL query.
- `#[derive(Selectable)]` will generate code to construct a matching select clause based on your model type based on the table defined via `#[diesel(table_name = crate::schema::posts)]`
- `#[diesel(check_for_backend(diesel::pg::Pg)]` adds additional compile time checks to verify that all field types in your struct are compatible with their corresponding SQL side expressions. Optional.
