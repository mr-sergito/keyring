# Keyring

A Rust library for securely managing application secrets during local development.

## Purpose

Keyring provides a secure way to store and retrieve sensitive information such as:
- API keys
- Database credentials
- OAuth tokens
- Environment-specific configuration
- Other development secrets

Rather than hardcoding these values or storing them in plain text files, Keyring encrypts them locally and provides a simple interface for your applications to access them.

## Goals

- **Security**: Store secrets with strong encryption
- **Simplicity**: Provide an easy-to-use API for developers
- **Integration**: Work well with common development workflows
- **Cross-platform**: Support major operating systems

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.
