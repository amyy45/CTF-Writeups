# 🧩 CTF Challenge Writeup  
**Challenge Name:** Rust fixme 1  
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
The hint says **`println!`** — hinting that one of the bugs is in the print macro.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category  
- **Easy** difficulty  
- A Rust file with intentional syntax errors to fix
- Hint points to `println!` macro

Key observations:
- The source code has **3 comments** that directly point to each bug:
  - `"How do we end statements in Rust?"` → missing semicolon
  - `"How do we return in rust?"` → `ret` should be `return`
  - `"How do we print out a variable in the println function?"` → wrong format string
- These are all **basic Rust syntax** errors — no complex logic needed
- Fix all 3 and `cargo run` prints the flag

The intended approach is to **read the inline comments**, identify all 3 bugs, fix them, and compile.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Extract Locally
```bash
cd ~/Downloads
wget <challenge-url> -O fixme1.tar.gz
tar -xzf fixme1.tar.gz
cd fixme1
cat src/main.rs
```

### 3.2 Identify the 3 Bugs

**Bug 1 — Missing semicolon:**
```rust
let key = String::from("CSUCKS") // ← missing ;
```
In Rust, all statements must end with a semicolon.

**Bug 2 — Wrong return keyword:**
```rust
ret; // ← should be return;
```
Rust uses `return` not `ret` to return early from a function.

**Bug 3 — Wrong println! format string:**
```rust
println!(
    ":?",  // ← should be "{:?}"
    String::from_utf8_lossy(&decrypted_buffer)
);
```
In Rust's `println!` macro, variables are printed using `{}` or `{:?}` (debug format) — curly braces are required.

### 3.3 Fix All 3 Bugs

```rust
use xor_cryptor::XORCryptor;

fn main() {
    let key = String::from("CSUCKS");   // Fix 1: added semicolon
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", "60", "e9", "62", "20", "0c", "e6", "50", "d3", "35"];
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;   // Fix 2: ret → return
    }
    let xrc = res.unwrap();
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    println!(
        "{:?}",   // Fix 3: ":?" → "{:?}"
        String::from_utf8_lossy(&decrypted_buffer)
    );
}
```

### 3.4 Build and Run
```bash
cargo run
```

Output:
```
"picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}"
```

---

## 🧩 4. Final Flag

```
picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}
```

---

## 📚 5. Key Learnings

- **Rust requires semicolons** at the end of every statement — forgetting one is a compile error
- The **`return` keyword** in Rust is `return` — unlike some languages that use `ret` or `ret()`
- **`println!` format strings** require curly braces: `{}` for Display and `{:?}` for Debug output — just `:?` without braces does nothing
- The inline comments in fixme challenges are **intentional hints** — always read them carefully
- Rust's compiler errors are extremely descriptive — even without the hints, the compiler would have told you exactly what to fix on each line
- `{:?}` is Rust's **debug format** — useful for printing types that implement `Debug` but not `Display`

---

## 🚀 6. Improvements for Next Time

- For Rust fixme challenges, **read every comment** — they're always hints pointing at the exact bug
- Remember the 3 most common beginner Rust mistakes: missing `;`, wrong keywords, wrong format strings
- Use `cargo check` before `cargo run` — it's faster and shows all compile errors at once
- Know Rust's `println!` format specifiers: `{}` = Display, `{:?}` = Debug, `{:#?}` = Pretty Debug
- When you see `ret`, `ret()`, or similar — it's always `return` in Rust

---

## 🔗 7. References

- [Rust Book – Hello World and println!](https://doc.rust-lang.org/book/ch01-02-hello-world.html)
- [Rust Book – Statements and Expressions](https://doc.rust-lang.org/book/ch03-03-how-functions-work.html)
- [Rust std::fmt Documentation](https://doc.rust-lang.org/std/fmt/)
- [xor_cryptor crate](https://crates.io/crates/xor_cryptor)
- [picoCTF General Skills Challenges](https://picoctf.org)