# CLI Design

This document outlines the design of the command-line interface (CLI) for Keyring.

## Command Structure

The keyring CLI follows a command-based structure with subcommands for different operations:

```
keyring [OPTIONS] <COMMAND> [ARGS]...
```

## Global Options

- `--service <SERVICE>`: Specify the service name (default: "default")
- `--no-interactive`: Disable interactive prompts
- `--debug`: Enable debug output
- `--help`: Show help information
- `--version`: Show version information

## Commands

### Set

Store a new secret or update an existing one:

```
keyring set <KEY> [VALUE]
```

If `VALUE` is not provided, it will be prompted for securely.

Examples:
```bash
# Provide value directly (not recommended for sensitive data)
keyring set api_key 1234567890abcdef

# Prompt for value
keyring set database_password

# Use a different service
keyring --service my_project set database_url
```

### Get

Retrieve a secret value:

```
keyring get <KEY>
```

Examples:
```bash
# Basic retrieval
keyring get api_key

# Specify service
keyring --service my_project get database_url

# Pipe to another command (won't show in terminal)
keyring get api_key | some_api_tool
```

### Delete

Remove a secret:

```
keyring delete <KEY>
```

Examples:
```bash
keyring delete api_key
keyring --service my_project delete database_url
```

### List

List all keys for a service:

```
keyring list [--service <SERVICE>]
```

Examples:
```bash
# List all keys in default service
keyring list

# List keys for specific service
keyring list --service my_project
```

### Services

List all services:

```
keyring services
```

## Interactive Mode

When running in interactive mode (default), the CLI will:

1. Prompt for the master password if needed
2. Confirm before destructive operations
3. Mask sensitive input (passwords, etc.)

Example interaction:
```
$ keyring set database_password
Enter master password: [hidden input]
Enter value for 'database_password': [hidden input]
Secret stored successfully.
```

## Scripting Mode

For use in scripts, the `--no-interactive` flag can be used. In this mode:

1. Master password must be provided via environment variable (`KEYRING_PASSWORD`)
2. No confirmation prompts will be shown
3. Commands will fail if additional input is required

Example:
```bash
export KEYRING_PASSWORD=my_master_pass
keyring --no-interactive set api_key "$(cat api_key.txt)"
```

## Error Handling

The CLI will:

1. Return appropriate exit codes (0 for success, non-zero for errors)
2. Print error messages to stderr
3. Provide clear error messages with potential solutions
