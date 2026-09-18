<!-- converted from Cursor rules -->

## Cursor rule: `.cursor/rules/agent-commits.mdc`

# Agent Commit Rule

After completing any task or set of related changes, the agent MUST create a git commit before finishing.

## When to Commit

The agent should commit after:

1. **Completing a feature or enhancement**
2. **Fixing a bug**
3. **Adding documentation**
4. **Refactoring code**
5. **Adding tests**
6. **Updating dependencies**
7. **Creating or modifying configuration files**

## Commit Process

### Step 1: Stage Changes

```bash
# Stage all changes
git add -A

# Or stage specific files
git add src/lexer.rs src/parser.rs
```

### Step 2: Create Commit

Follow the Conventional Commits format (per `commitlint.yaml`):

```bash
git commit -m "<type>(<scope>): <description>"
```

### Step 3: Verify Commit

```bash
git log -1 --oneline
```

## Commit Message Format

```
<type>(<scope>): <short description>

[optional body with more details]

[optional footer]
```

### Types

| Type | Use For |
|------|---------|
| `feat` | New features |
| `fix` | Bug fixes |
| `docs` | Documentation changes |
| `style` | Formatting, whitespace |
| `refactor` | Code restructuring |
| `perf` | Performance improvements |
| `test` | Adding or fixing tests |
| `build` | Build system, dependencies |
| `ci` | CI/CD configuration |
| `chore` | Maintenance tasks |

### Scopes (for this project)

**Core Shell Components:**
- `lexer` - Lexer/tokenizer
- `parser` - Parser
- `ast` - AST definitions
- `interpreter` - Interpreter/executor
- `builtins` - Built-in commands
- `shell` - REPL/shell interface
- `theme` - Theme system
- `error` - Error handling

**Compatibility Layers:**
- `bash` - Bash compatibility
- `zsh` - ZSH compatibility
- `fish` - Fish compatibility
- `ash` - Ash/POSIX compatibility
- `posix` - POSIX compliance

**Build & Packaging:**
- `deps` - Dependencies
- `build` - Build system
- `packaging` - Package builds (deb, rpm, arch)
- `deb` - Debian packaging
- `rpm` - RPM packaging
- `arch` - Arch Linux packaging

**Development & Tooling:**
- `docs` - Documentation site
- `bench` - Benchmarks
- `ci` - CI/CD pipelines
- `test` - Test infrastructure
- `lint` - Linting configuration

**Cursor/IDE Integration:**
- `cursor` - Cursor IDE rules and config
- `rules` - Cursor rules specifically
- `editor` - Editor configuration

**Project Management:**
- `config` - Configuration files
- `git` - Git hooks, gitignore, etc.
- `release` - Release process

## Examples

### Single Feature

```bash
git add -A
git commit -m "feat(parser): add support for named function parameters"
```

### Bug Fix

```bash
git add src/lexer.rs
git commit -m "fix(lexer): handle escaped quotes in double-quoted strings"
```

### Multiple Related Changes

```bash
git add -A
git commit -m "feat(shell): add system-wide config file support

- Source /etc/jshenv for all shells
- Source /etc/jsh_profile for login shells
- Source /etc/jshrc for interactive shells
- Update README with documentation"
```

### Documentation

```bash
git add README.md docs/
git commit -m "docs: add installation instructions for Arch Linux"
```

### Build/Packaging

```bash
git add packaging/ Cargo.toml
git commit -m "build(deps): add Debian and RPM packaging support"
```

## Exceptions

Do NOT commit if:

1. **Build fails** - Fix errors first
2. **Tests fail** - Fix failing tests first
3. **Work is incomplete** - Only commit working code
4. **User explicitly asks not to commit**

## Verification Before Commit

Before committing, the agent should verify:

```bash
# Ensure code compiles
cargo build --release

# Run quick test (with timeout per shell-testing rule)
timeout 5 ./target/release/jsh -c 'echo "Build successful"'
```

## Branch Awareness

- Check current branch before committing
- Follow git-flow rules (per `git-flow.mdc`)
- Don't commit directly to `main` or `develop` without PR

```bash
# Check current branch
git branch --show-current

# If on main/develop, create feature branch first
git checkout -b feature/my-changes
```


## Cursor rule: `.cursor/rules/git-flow.mdc`

# Git-Flow Branching Model

This project follows the Git-Flow branching model for version control and release management.

## Branch Structure

### Main Branches

| Branch | Purpose |
|--------|---------|
| `main` | Production-ready code. Only receives merges from `release/*` and `hotfix/*` branches. |
| `develop` | Integration branch for features. Contains the latest delivered development changes. |

### Supporting Branches

| Branch Type | Naming Convention | Branch From | Merge Into |
|-------------|-------------------|-------------|------------|
| Feature | `feature/<name>` | `develop` | `develop` |
| Release | `release/<version>` | `develop` | `main` and `develop` |
| Hotfix | `hotfix/<name>` | `main` | `main` and `develop` |
| Bugfix | `bugfix/<name>` | `develop` | `develop` |

## Workflow Rules

### Creating Feature Branches

```bash
# Start a new feature
git checkout develop
git pull origin develop
git checkout -b feature/my-feature-name

# When complete, merge back to develop
git checkout develop
git merge --no-ff feature/my-feature-name
git push origin develop
git branch -d feature/my-feature-name
```

### Creating Release Branches

```bash
# Start a release (from develop)
git checkout develop
git checkout -b release/1.0.0

# Bump version in Cargo.toml, update CHANGELOG.md
# Only bug fixes allowed on release branches

# Finish release
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

git checkout develop
git merge --no-ff release/1.0.0
git push origin develop

git branch -d release/1.0.0
```

### Creating Hotfix Branches

```bash
# Start a hotfix (from main)
git checkout main
git checkout -b hotfix/critical-bug-fix

# Fix the bug, bump patch version

# Finish hotfix
git checkout main
git merge --no-ff hotfix/critical-bug-fix
git tag -a v1.0.1 -m "Hotfix version 1.0.1"
git push origin main --tags

git checkout develop
git merge --no-ff hotfix/critical-bug-fix
git push origin develop

git branch -d hotfix/critical-bug-fix
```

## Branch Naming Conventions

### Feature Branches
- `feature/add-fish-compatibility`
- `feature/lexer-improvements`
- `feature/theme-support`

### Release Branches
- `release/0.1.0`
- `release/1.0.0`
- `release/2.0.0-beta`

### Hotfix Branches
- `hotfix/fix-crash-on-startup`
- `hotfix/security-patch`
- `hotfix/memory-leak`

### Bugfix Branches
- `bugfix/parser-edge-case`
- `bugfix/variable-expansion`

## Commit Message Format

Follow the Conventional Commits specification (see `commitlint.yaml`):

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style (formatting, whitespace)
- `refactor`: Code change that neither fixes nor adds
- `perf`: Performance improvement
- `test`: Adding or correcting tests
- `build`: Build system or dependencies
- `ci`: CI configuration
- `chore`: Other changes

## Version Tagging

- Tags follow semantic versioning: `v<major>.<minor>.<patch>`
- Tags are only created on the `main` branch
- Tags are annotated with release notes

```bash
git tag -a v1.0.0 -m "Release version 1.0.0

Features:
- Feature 1
- Feature 2

Bug fixes:
- Fix 1
- Fix 2"
```

## Pull Request Guidelines

1. **Feature PRs**: `feature/*` → `develop`
2. **Release PRs**: `release/*` → `main` (then also merge to `develop`)
3. **Hotfix PRs**: `hotfix/*` → `main` (then also merge to `develop`)
4. **Bugfix PRs**: `bugfix/*` → `develop`

### PR Title Format
```
[TYPE] Brief description

Examples:
[Feature] Add Fish shell compatibility
[Release] Version 1.0.0
[Hotfix] Fix crash when parsing empty scripts
[Bugfix] Correct variable expansion in quoted strings
```

## Protected Branches

The following branches should be protected:

- `main`: Require PR reviews, no direct pushes
- `develop`: Require PR reviews for external contributors

## CI/CD Integration

- **Feature branches**: Run tests, linting
- **Develop branch**: Run tests, linting, build packages
- **Release branches**: Run full test suite, build packages, generate changelog
- **Main branch**: Deploy releases, publish packages


## Cursor rule: `.cursor/rules/rust-guidelines.mdc`

# Rust Programming Guidelines

This project follows idiomatic Rust practices and best practices for safe, performant, and maintainable code.

## Code Style

### Formatting

- Always run `cargo fmt` before committing
- Use `rustfmt.toml` configuration if present
- Maximum line length: 100 characters
- Use 4 spaces for indentation (Rust default)

### Naming Conventions

| Item | Convention | Example |
|------|------------|---------|
| Crates | `snake_case` | `my_crate` |
| Modules | `snake_case` | `my_module` |
| Types (structs, enums, traits) | `PascalCase` | `MyStruct`, `MyEnum` |
| Functions | `snake_case` | `my_function` |
| Methods | `snake_case` | `my_method` |
| Local variables | `snake_case` | `my_variable` |
| Constants | `SCREAMING_SNAKE_CASE` | `MAX_SIZE` |
| Static variables | `SCREAMING_SNAKE_CASE` | `GLOBAL_CONFIG` |
| Type parameters | `PascalCase`, single letter preferred | `T`, `E`, `K`, `V` |
| Lifetimes | Short lowercase | `'a`, `'de`, `'src` |

### Import Organization

Organize imports in this order, separated by blank lines:

```rust
// 1. Standard library
use std::collections::HashMap;
use std::io::{self, Read, Write};

// 2. External crates
use anyhow::Result;
use thiserror::Error;

// 3. Local crate modules
use crate::ast::Statement;
use crate::error::JshError;
```

## Error Handling

### Use `thiserror` for Library Errors

```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum JshError {
    #[error("Syntax error: {0}")]
    Syntax(String),

    #[error("Command not found: {0}")]
    CommandNotFound(String),

    #[error("IO error: {0}")]
    Io(#[from] std::io::Error),
}
```

### Use `anyhow` for Application Errors

```rust
use anyhow::{Context, Result};

fn read_config() -> Result<Config> {
    let content = std::fs::read_to_string("config.toml")
        .context("Failed to read config file")?;
    // ...
}
```

### Error Handling Rules

1. **Never use `.unwrap()` in library code** - Use `?` or handle errors explicitly
2. **`.unwrap()` is acceptable** in tests and when the invariant is guaranteed
3. **Use `.expect("reason")` over `.unwrap()`** when panicking is intentional
4. **Propagate errors** with `?` operator when possible
5. **Add context** to errors using `.context()` or `.with_context()`

```rust
// Good
let file = File::open(path).context("Failed to open config")?;

// Bad
let file = File::open(path).unwrap();
```

## Memory and Performance

### Ownership and Borrowing

1. **Prefer borrowing over ownership** when you don't need to own the data
2. **Use `&str` instead of `String`** in function parameters when possible
3. **Use `&[T]` instead of `Vec<T>`** in function parameters when possible
4. **Clone only when necessary** - prefer references

```rust
// Good - borrows the string
fn process(input: &str) -> Result<()> { ... }

// Bad - takes ownership unnecessarily
fn process(input: String) -> Result<()> { ... }
```

### Avoid Unnecessary Allocations

```rust
// Good - reuse buffer
let mut buffer = String::new();
for item in items {
    buffer.clear();
    write!(&mut buffer, "{}", item)?;
    process(&buffer)?;
}

// Bad - allocates on each iteration
for item in items {
    let buffer = format!("{}", item);
    process(&buffer)?;
}
```

### Use Iterators Over Loops

```rust
// Good - idiomatic, often optimized better
let sum: i32 = numbers.iter().filter(|n| **n > 0).sum();

// Less idiomatic
let mut sum = 0;
for n in &numbers {
    if *n > 0 {
        sum += n;
    }
}
```

### Prefer `collect()` with Type Annotation

```rust
// Good - clear intent
let names: Vec<String> = items.iter().map(|i| i.name.clone()).collect();

// Also good - turbofish syntax
let names = items.iter().map(|i| i.name.clone()).collect::<Vec<_>>();
```

## Safety

### Unsafe Code

1. **Minimize `unsafe` blocks** - use safe abstractions when possible
2. **Document all `unsafe` blocks** with `// SAFETY:` comments
3. **Isolate unsafe code** into small, well-tested functions
4. **Never use `unsafe` to bypass borrow checker** unless absolutely necessary

```rust
// Good - documented unsafe
// SAFETY: We have exclusive access to the buffer and the pointer is valid
unsafe {
    ptr::copy_nonoverlapping(src, dst, len);
}
```

### Environment Variables

Environment variable operations are unsafe in Rust 2024. Wrap them properly:

```rust
// Setting environment variables requires unsafe
// SAFETY: No other threads are accessing environment variables
unsafe {
    std::env::set_var("KEY", "value");
}

// Reading is safe
let value = std::env::var("KEY").ok();
```

## Structs and Enums

### Derive Common Traits

```rust
#[derive(Debug, Clone, PartialEq, Eq, Hash)]
pub struct Token {
    pub kind: TokenKind,
    pub span: Span,
}
```

### Use Builder Pattern for Complex Construction

```rust
pub struct CommandBuilder {
    name: String,
    args: Vec<String>,
    env: HashMap<String, String>,
}

impl CommandBuilder {
    pub fn new(name: impl Into<String>) -> Self {
        Self {
            name: name.into(),
            args: Vec::new(),
            env: HashMap::new(),
        }
    }

    pub fn arg(mut self, arg: impl Into<String>) -> Self {
        self.args.push(arg.into());
        self
    }

    pub fn env(mut self, key: impl Into<String>, value: impl Into<String>) -> Self {
        self.env.insert(key.into(), value.into());
        self
    }

    pub fn build(self) -> Command {
        Command { ... }
    }
}
```

### Use `Default` Trait

```rust
#[derive(Debug, Default)]
pub struct Options {
    pub verbose: bool,
    pub output: Option<PathBuf>,
}

// Usage
let opts = Options::default();
let opts = Options { verbose: true, ..Default::default() };
```

## Pattern Matching

### Prefer Pattern Matching Over If-Let Chains

```rust
// Good
match result {
    Ok(value) if value > 0 => handle_positive(value),
    Ok(value) => handle_non_positive(value),
    Err(e) => handle_error(e),
}

// Less clear
if let Ok(value) = result {
    if value > 0 {
        handle_positive(value);
    } else {
        handle_non_positive(value);
    }
} else if let Err(e) = result {
    handle_error(e);
}
```

### Use `matches!` Macro for Boolean Checks

```rust
// Good
if matches!(token.kind, TokenKind::Word(_) | TokenKind::String(_)) {
    // ...
}

// Verbose
if let TokenKind::Word(_) | TokenKind::String(_) = token.kind {
    // ...
}
```

## Documentation

### Document Public API

```rust
/// Parses a shell script into an AST.
///
/// # Arguments
///
/// * `input` - The shell script source code
///
/// # Returns
///
/// Returns the parsed program AST or a syntax error.
///
/// # Examples
///
/// ```
/// use jsh::parser::Parser;
///
/// let mut parser = Parser::new("echo hello");
/// let program = parser.parse_program()?;
/// ```
///
/// # Errors
///
/// Returns `JshError::Syntax` if the input contains invalid syntax.
pub fn parse_program(&mut self) -> Result<Program, JshError> {
    // ...
}
```

### Use `#[must_use]` for Important Return Values

```rust
#[must_use]
pub fn is_empty(&self) -> bool {
    self.len() == 0
}
```

## Testing

### Unit Tests in Same File

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_lexer_basic() {
        let mut lexer = Lexer::new("echo hello");
        let tokens = lexer.tokenize().unwrap();
        assert_eq!(tokens.len(), 2);
    }
}
```

### Use Descriptive Test Names

```rust
#[test]
fn lexer_handles_quoted_strings_with_escapes() { ... }

#[test]
fn parser_rejects_unclosed_braces() { ... }

#[test]
fn interpreter_expands_variables_in_strings() { ... }
```

### Test Error Cases

```rust
#[test]
fn parser_returns_error_for_invalid_syntax() {
    let mut parser = Parser::new("if then");
    let result = parser.parse_program();
    assert!(result.is_err());
    assert!(matches!(result, Err(JshError::Syntax(_))));
}
```

## Clippy

### Run Clippy Regularly

```bash
cargo clippy -- -W clippy::all -W clippy::pedantic
```

### Common Clippy Lints to Address

- `clippy::unwrap_used` - Use `?` or `expect()`
- `clippy::clone_on_ref_ptr` - Avoid cloning Rc/Arc unnecessarily
- `clippy::inefficient_to_string` - Use `to_owned()` for `&str`
- `clippy::needless_collect` - Don't collect iterators unnecessarily
- `clippy::manual_map` - Use `.map()` instead of match

### Allow Specific Lints When Justified

```rust
#[allow(clippy::too_many_arguments)]
fn complex_function(a: i32, b: i32, c: i32, d: i32, e: i32, f: i32) {
    // Justified because: this maps directly to a system call
}
```

## Cargo.toml Best Practices

### Organize Dependencies

```toml
[dependencies]
# Core functionality
anyhow = "1"
thiserror = "2"

# Shell-specific
rustyline = { version = "14", features = ["derive"] }
nix = { version = "0.29", features = ["process", "signal"] }

# Utilities
regex = "1"
glob = "0.3"

[dev-dependencies]
criterion = { version = "0.5", features = ["html_reports"] }
```

### Use Workspace for Multi-Crate Projects

### Specify Minimum Rust Version

```toml
[package]
rust-version = "1.85"
```

## Performance Profiling

### Use Criterion for Benchmarks

```rust
use criterion::{black_box, criterion_group, criterion_main, Criterion};

fn bench_lexer(c: &mut Criterion) {
    let input = "echo hello world";
    c.bench_function("lexer_simple", |b| {
        b.iter(|| {
            let mut lexer = Lexer::new(black_box(input));
            lexer.tokenize()
        })
    });
}

criterion_group!(benches, bench_lexer);
criterion_main!(benches);
```

### Profile with `cargo flamegraph`

```bash
cargo install flamegraph
cargo flamegraph --bin jsh -- -c 'echo hello'
```


## Cursor rule: `.cursor/rules/shell-testing.mdc`

_Rules for testing jsh shell functionality_

# Shell Testing Rules

When testing jsh shell functionality via command line, always use timeouts to prevent hanging.

## Command Line Testing

### Always Use Timeout

When running jsh commands for testing, wrap them with `timeout` to prevent hanging:

```bash
# Good - with timeout
timeout 5 ./target/debug/jsh -c 'echo hello'
timeout 3 sh -c 'echo "test" | ./target/debug/jsh'

# Bad - can hang indefinitely
./target/debug/jsh -c 'echo hello'
echo "test" | ./target/debug/jsh
```

### Recommended Timeout Values

- Simple commands (echo, variable expansion): 3 seconds
- File operations: 5 seconds
- Loop/control flow tests: 5 seconds
- Script execution: 10 seconds
- Complex operations: 15 seconds

### Testing Patterns

1. **For piped input**, use `sh -c` wrapper:
   ```bash
   timeout 3 sh -c 'echo "command" | ./target/release/jsh'
   ```

2. **For inline commands**, use `-c` flag:
   ```bash
   timeout 5 ./target/release/jsh -c 'echo "test"'
   ```

3. **For Rust-based tests**, create test binaries that use the library directly:
   ```rust
   use jsh::interpreter::Interpreter;
   let mut interp = Interpreter::new();
   interp.execute_string("echo hello").unwrap();
   ```

### Handling Test Output

- Redirect stderr when needed: `2>&1`
- Use `|| true` to prevent non-zero exit codes from stopping execution
- Capture output for verification: `result=$(timeout 3 ./target/debug/jsh -c 'echo test')`

### Example Test Commands

```bash
# Test basic echo
timeout 3 ./target/release/jsh -c 'echo "Hello, World!"'

# Test function definition and call
timeout 5 sh -c 'echo "greet(name) { echo Hello \$name; }; greet World" | ./target/release/jsh'

# Test environment variables
timeout 3 ./target/release/jsh -c 'echo "SHELL=$SHELL JSH=$JSH"'

# Test arithmetic
timeout 3 ./target/release/jsh -c 'echo $((2 + 3 * 4))'

# Test control flow
timeout 5 ./target/release/jsh -c 'for i in 1 2 3; do echo $i; done'
```

## Why Timeouts Are Important

1. **Interactive mode**: jsh may enter interactive mode if stdin detection fails
2. **Profile sourcing**: Sourcing user profiles may trigger unexpected prompts
3. **Infinite loops**: Bugs in loop handling can cause hangs
4. **Blocking operations**: Some operations may wait for input indefinitely

## CI/CD Considerations

In CI pipelines, always use timeouts with appropriate values:

```yaml
- name: Test jsh
  run: |
    timeout 30 cargo test
    timeout 5 ./target/release/jsh -c 'echo "CI test passed"'
```

