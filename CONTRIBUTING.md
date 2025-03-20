# Contributing to Keyring

Thank you for your interest in contributing to Keyring! This document provides guidelines and instructions for contributing.

## Development Setup

1. **Fork and clone the repository**:
   ```bash
   git clone https://github.com/yourusername/keyring.git
   cd keyring
   ```

2. **Install Rust** (if not already installed):
   - Follow the instructions at [rust-lang.org](https://www.rust-lang.org/tools/install)

3. **Build the project**:
   ```bash
   cargo build
   ```

4. **Run tests**:
   ```bash
   cargo test
   ```

## Project Structure

The project is organized as follows:

```
keyring/
├── src/
│   ├── cli/           # Command-line interface
│   ├── crypto/        # Cryptography module
│   ├── storage/       # Storage layer
│   ├── keyring.rs     # Core API
│   ├── error.rs       # Error handling
│   └── main.rs        # Entry point for CLI
├── docs/              # Documentation
├── tests/             # Integration tests
├── examples/          # Example usage
└── benches/           # Performance benchmarks
```

## Coding Standards

- Follow the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
- Use `rustfmt` to format your code
- Use `clippy` to catch common mistakes
- Write documentation for public API items
- Include unit tests for new functionality

## Pull Request Process

1. Create a new branch for your feature or bugfix
2. Write tests that demonstrate your changes work as expected
3. Ensure all tests pass with `cargo test`
4. Update documentation if needed
5. Submit a pull request with a clear description of the changes

## Code Review

All submissions require review. We use GitHub pull requests for this purpose:

1. Submit your PR
2. CI will run tests automatically
3. Maintainers will review your code
4. Address any feedback
5. Once approved, your PR will be merged

## Security Considerations

Since Keyring deals with sensitive information:

- Be extra careful when modifying cryptographic code
- Never log sensitive information
- Consider memory safety implications
- Properly handle and clear sensitive data in memory

## License

By contributing to Keyring, you agree that your contributions will be licensed under the project's [GPL-3.0 License](LICENSE).
