# API Design

This document outlines the API design for the Keyring library.

## Core Types

### `Keyring`

The main interface for interacting with the keyring system.

```rust
pub struct Keyring {
    // Private implementation details
}

impl Keyring {
    /// Create a new Keyring instance
    pub fn new() -> Result<Self>;
    
    /// Create a Keyring with a specific service name
    pub fn new_with_service(service: &str) -> Result<Self>;
    
    /// Set a secret value
    pub fn set(&self, key: &str, value: &str) -> Result<()>;
    
    /// Get a secret value
    pub fn get(&self, key: &str) -> Result<String>;
    
    /// Delete a secret
    pub fn delete(&self, key: &str) -> Result<()>;
    
    /// List all keys in this keyring
    pub fn list(&self) -> Result<Vec<String>>;
}
```

### `Error`

Comprehensive error handling:

```rust
pub enum KeyringError {
    /// I/O operation failed
    Io(std::io::Error),
    
    /// Serialization error
    Serialization(serde_json::Error),
    
    /// Cryptographic operation failed
    Crypto(String),
    
    /// Requested key not found
    NotFound(String),
    
    /// Authentication failed (wrong password)
    AuthenticationFailed,
    
    /// Other unexpected errors
    Other(String),
}

pub type Result<T> = std::result::Result<T, KeyringError>;
```

## Usage Examples

### Basic Usage

```rust
use keyring::Keyring;

fn main() -> keyring::Result<()> {
    // Create a keyring for a specific service
    let keyring = Keyring::new_with_service("my_app")?;
    
    // Store a secret
    keyring.set("database_password", "very_secret_value")?;
    
    // Retrieve the secret
    let password = keyring.get("database_password")?;
    println!("Retrieved: {}", password);
    
    // List all keys
    let keys = keyring.list()?;
    println!("All keys: {:?}", keys);
    
    Ok(())
}
```

### Integration with Config Files

```rust
use keyring::Keyring;
use serde::{Deserialize, Serialize};

#[derive(Serialize, Deserialize)]
struct AppConfig {
    server_url: String,
    #[serde(skip)]
    api_key: Option<String>,
}

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Load config
    let mut config: AppConfig = /* load from file */;
    
    // Get API key from keyring
    let keyring = Keyring::new_with_service("my_app")?;
    if let Ok(api_key) = keyring.get("api_key") {
        config.api_key = Some(api_key);
    }
    
    Ok(())
}
```

## Password Handling

The keyring will prompt for a master password:

1. First attempt to use any OS-specific credential storage
2. Then try environment variables (for CI/CD environments)
3. Finally, prompt the user interactively

Applications can also provide their own password handling:

```rust
use keyring::{Keyring, KeyringBuilder};

let keyring = KeyringBuilder::new()
    .service("my_app")
    .password_callback(|| {
        // Custom password retrieval logic
        Ok("master_password".to_string())
    })
    .build()?;
```
