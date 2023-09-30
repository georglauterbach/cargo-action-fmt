# Visualize Issues with Rust Code in GitHub Actions

This action provides you with the ability to visualize issues in Rust code found with checks and linters like `cargo check` or `cargo clippy`.

## Examples

This tool can be used with a variety of `cargo` commands:

```console
$ cargo clippy
error: missing documentation for a struct field
  --> src/messages.rs:90:3
   |
90 |   spans:    Vec<CompilerMessageSpan>,
   |   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#missing_docs_in_private_items
note: the lint level is defined here
  --> src/main.rs:26:9
   |
26 | #![deny(clippy::missing_docs_in_private_items)]
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

$ cargo check -q --message-format=json | cargo-action-fmt
::error file=src/messages.rs,line=90,endLine=90,col=3,endColumn=37::error: missing documentation for a struct field%0ASee https://rust-lang.github.io/rust-clippy/master/index.html#missing_docs_in_private_items%0A%0AThe lint level is defined in src/main.rs:26:9 by #![deny(clippy::missing_docs_in_private_items)]

$ cargo doc --message-format=json | cargo-action-fmt
...
```

**Note** that this tool does **not** currently support `cargo fmt` or `cargo test` output. However, you can invoke `cargo test` so that test compilation errors are annotated properly:

```console
$ cargo test --no-run --message-format=json | cargo-action-fmt
...
```

### GitHub Action

It's primarily intended to be used in GitHub Actions workflows:

```yaml
jobs:
  clippy:
    runs-on: ubuntu-latest
    container: rust:slim
    steps:
      - uses: georglauterbach/cargo-action-fmt@v0.1.0
      - uses: actions/checkout@v4
      - run: cargo clippy -q --message-format=json | cargo-action-fmt
```
