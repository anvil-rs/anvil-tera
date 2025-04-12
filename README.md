# anvil-tera

An [Anvil](https://github.com/anvil-rs/anvil) integration for [Tera](https://github.com/keats/tera) templates.

## Installation

```toml
[dependencies]
anvil-tera = "0.2.1"
anvil = "0.3.0"
tera = "1.20.0"
serde = { version = "1.0", features = ["derive"] }
```

## Usage

```rust
use anvil::Forge;
use anvil_tera::prelude::*;  // Import extension traits and functions
use serde::Serialize;
use tera::Tera;
use std::sync::LazyLock;

// Define a Tera instance for your templates
static TEMPLATES: LazyLock<Tera> = LazyLock::new(|| {
    let mut tera = Tera::default();
    tera.add_raw_template("greeting.html", "Hello, {{ name }}!").unwrap();
    tera
});

// Define a serializable template context
#[derive(Serialize)]
struct MyTemplate {
    name: String,
}

// Use macro to implement the Earth trait with a template
make_tera_template!(MyTemplate, "greeting.html", TEMPLATES);

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let template = MyTemplate { name: "World".to_string() };

    // Generate a new file
    generate(&template).forge("hello.txt")?;

    // Append to an existing file
    append(&template).forge("log.txt")?;

    Ok(())
}
```
