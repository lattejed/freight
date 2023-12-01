# freight

**A heavily opinionated scaffolding generator for Rust projects**

## What is Freight?

Freight builds out a project structure for large Rust projects. While `cargo` is great for simple projects, larger projects require a more complex structure. Freight is *highly opinionated*, it is the One True Way™ of large Rust projects in tool form. You may or may not agree with this structure, so this tool may or may not be useful to you.

## Goals

- Speed up the development of large Rust projects
- Enforce project structure and best practices 

## Non-goals

- Accommodate a variety of project structures
- Be extensible to accommodate the above 
- Be useful for things outside of Rust

## Project Structure

Running `add workspace test-project` generates the following structure:

```
<project-root>/
├─ .git/
├─ .github/
├─ crates/
├─ tests/
├─ .freight
├─ .gitignore
├─ CONTRIBUTING.md
├─ Cargo.toml
├─ LICENSE-APACHE
├─ LICENSE-MIT
├─ README.md
├─ rustfmt.toml
```

The top-level `Cargo.toml` file will have the following structure:

```toml
[workspace]
resolver = "2"
members = []

[workspace.package]
version = "0.1.0"
edition = "2021"
rust-version = "1.65"
license = "MIT OR Apache-2.0"
exclude = ["benches/", "tests/"]
authors = []
repository = ""

[workspace.dependencies]
# workspace dependenices

# external dependencies

# dev dependencies
```

The `members`, `authors`, `repository` and `dependencies` sections will be empty.

Running `add crate --bin --default cli` will add a `test-project-cli` crate:

```
<project-root>/
├─ .git/
├─ .github/
├─ crates/
│  ├─ cli/
│  │  ├─ src/
│  │  │  ├─ main.rs
│  │  ├─ CHANGELOG.md
│  │  ├─ Cargo.toml
│  │  ├─ README.md
├─ tests/
├─ .freight
├─ .gitignore
├─ CONTRIBUTING.md
├─ Cargo.toml
├─ LICENSE-APACHE
├─ LICENSE-MIT
├─ README.md
├─ rustfmt.toml
```

The top-level `Cargo.toml` file now look like:

```toml
[workspace]
resolver = "2"
members = [
    "crates/cli",
]

[workspace.package]
#...

[workspace.dependencies]
#...
```

The `Cargo.toml` for the `cli` crate will look like:

```toml
[package]
name = "test-project-cli"
description = ""

version.workspace = true
edition.workspace = true
rust-version.workspace = true
authors.workspace = true
license.workspace = true
repository.workspace = true
exclude.workspace = true

[dependencies]
# workspace dependenices

# external dependencies

# dev dependencies

[[bin]]
name = "test-project"
path = "src/main.rs"
doc = false
```

Running `add crate --lib parser` will add a `test-project-parser` crate:

```
<project-root>/
├─ .git/
├─ .github/
├─ crates/
│  ├─ cli/
│  │  ├─ src/
│  │  │  ├─ main.rs
│  │  ├─ CHANGELOG.md
│  │  ├─ Cargo.toml
│  │  ├─ README.md
│  ├─ parser/
│  │  ├─ src/
│  │  │  ├─ lib.rs
│  │  ├─ CHANGELOG.md
│  │  ├─ Cargo.toml
│  │  ├─ README.md
├─ tests/
├─ .freight
├─ .gitignore
├─ CONTRIBUTING.md
├─ Cargo.toml
├─ LICENSE-APACHE
├─ LICENSE-MIT
├─ README.md
├─ rustfmt.toml
```

The top-level `Cargo.toml` file now look like:

```toml
[workspace]
resolver = "2"
members = [
    "crates/cli",
    "crates/parser",
]

[workspace.package]
#...

[workspace.dependencies]
# workspace dependenices
test-project-parser = { path = "crates/parser" }
#...
```

