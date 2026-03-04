# 🧩 CTF Challenge Writeup  
**Challenge Name:** Rust fixme 3  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2025  
**Author:** TAYLOR MCCAMPBELL  
**Date:** *03-03-26*  

---

## 🔍 1. Challenge Description

The challenge presents a Rust source file with the following description:

> *"Have you heard of Rust? Fix the syntax errors in this Rust file to print the flag!"*

A Rust Cargo project is provided for download as a `.tar.gz` archive.  
The challenge warns: **"This problem is not solvable with the webshell"** — meaning it requires a local Rust installation to compile and run.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category  
- **Easy** difficulty  
- A Rust project with intentional syntax/compilation errors
- Must fix the errors, compile, and run to get the flag

Key observations:
- The file uses `xor_cryptor` crate to decrypt the flag at runtime
- The decryption uses `std::slice::from_raw_parts()` — an **unsafe** Rust operation
- The `unsafe {}` block is **commented out** — this is the bug
- In Rust, any code that uses raw pointers or unsafe functions **must** be wrapped in an `unsafe {}` block
- Uncommenting the `unsafe` block makes the code valid and allows it to compile

The intended approach is to **identify the commented-out `unsafe` block**, uncomment it, and run the program.

---

## 🛠️ 3. Steps to Solve

### 3.1 Install Rust Locally
The webshell runs out of memory compiling Rust dependencies, so this must be done locally:
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

### 3.2 Download and Extract the Project
```bash
wget https://challenge-files.picoctf.net/.../fixme3.tar.gz
tar -xzf fixme3.tar.gz
cd fixme3
```

### 3.3 Read the Source Code
```bash
cat src/main.rs
```

The bug is immediately visible — the `unsafe {}` block is commented out:

```rust
// unsafe {
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();
    
    // Unsafe operation: calling an unsafe function that dereferences a raw pointer
    let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);
    borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
// }
```

`std::slice::from_raw_parts()` dereferences a raw pointer — this is an **unsafe operation** in Rust and must be inside an `unsafe {}` block.

### 3.4 Fix the Code
Uncomment the `unsafe` block:

```rust
unsafe {
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    let decrypted_ptr = decrypted_buffer.as_ptr();
    let decrypted_len = decrypted_buffer.len();
    let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);
    borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
}
```

### 3.5 Build and Run
```bash
cargo run
```

Output:
```
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
```

---

## 🧩 4. Final Flag

```
picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
```

---

## 📚 5. Key Learnings

- **Rust requires `unsafe {}` blocks** for any operation that bypasses its memory safety guarantees — including raw pointer dereferencing, calling unsafe functions, and accessing mutable static variables
- `std::slice::from_raw_parts()` creates a slice from a raw pointer — inherently unsafe because Rust cannot verify the pointer is valid
- Commenting out an `unsafe {}` block doesn't remove the unsafe operations inside — it just causes a **compile error**
- The webshell has memory limitations — heavy compilation tasks like Rust projects require a **local environment**
- `cargo run` handles both building and running in one command
- The flag was XOR-encrypted at rest and decrypted at runtime — the key `"CSUCKS"` is a hint about C language memory unsafety

---

## 🚀 6. Improvements for Next Time

- When a challenge says **"not solvable with the webshell"**, immediately set up a local environment
- For Rust challenges, look for **commented-out code blocks** — especially `unsafe {}`, `impl`, or `fn` blocks
- Always read the comments in the source carefully — they often explain exactly what the fix should be
- `cargo run` is faster than `rustc` for projects with dependencies — use it directly
- Know common Rust unsafe operations: `from_raw_parts`, `offset`, `write`, `read`, and FFI calls all require `unsafe {}`

---

## 🔗 7. References

- [Rust Unsafe Code – The Rust Book](https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html)
- [std::slice::from_raw_parts Documentation](https://doc.rust-lang.org/std/slice/fn.from_raw_parts.html)
- [xor_cryptor crate](https://crates.io/crates/xor_cryptor)
- [Cargo Documentation](https://doc.rust-lang.org/cargo/)
- [picoCTF General Skills Challenges](https://picoctf.org)