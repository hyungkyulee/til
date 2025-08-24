# Overview

# Setup
This will download and install the official compiler for the Rust programming language, and its package manager, Cargo.
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
> The cargo, rustc, rustup and other commands will be added to
  Cargo's bin directory, located at:

  /Users/kyu/.cargo/bin

  This path will then be added to your PATH environment variable by
  modifying the profile files located at:

  rust/Users/kyu/.profile
  /Users/kyu/.bash_profile
  /Users/kyu/.zshenv

```
 ~  rustc --version                                                                        ✔ │ 00:55:13
rustc 1.89.0 (29483883e 2025-08-04)
 ~  cargo --version                                                                        ✔ │ 00:55:19
cargo 1.89.0 (c24e10642 2025-06-23)
 ~  rustup --version                                                                       ✔ │ 00:55:23
rustup 1.28.2 (e4f3ad6f8 2025-04-28)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.89.0 (29483883e 2025-08-04)`
```

# Essential Syntax
