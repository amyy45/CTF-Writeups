# 🧩 CTF Challenge Writeup  
**Challenge Name:** Rust fixme 2  
**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF 2025  
**Author:** TAYLOR MCCAMPBELL  
**Date:** 03-03-26  

---

## 🔍 1. Challenge Description

The challenge presents a Rust source file with the following description:

> *"The Rust saga continues? I ask you, can I borrow that, pleeeeeaaaasseeeee?"*

A Rust Cargo project is provided for download as a `.tar.gz` archive.  
The hint points to: **https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html** — directly hinting at Rust's **references and borrowing** system.

---

## 🧠 2. Initial Thoughts / Approach

Given:
- **General Skills** category  
- **Easy** difficulty  
- The description explicitly mentions **borrowing** — a core Rust concept
- Hint links to the Rust book chapter on **References and Borrowing**

Key observations:
- The source code has helpful comments pointing to the bugs:
  - `"How do we pass values to a function that we want to change?"`
  - `"Is this variable changeable?"`
  - `"Is this the correct way to pass a value to a function so that it can be changed?"`
- The function takes `&String` (immutable reference) but calls `push_str()` which **modifies** the string
- In Rust, to modify a value through a reference, you need a **mutable reference** (`&mut String`)
- The variable `party_foul` is declared without `mut` — making it immutable by default
- Two fixes are needed: add `mut` to the variable declaration and change `&String` to `&mut String`

The intended approach is to **fix the mutability errors** so the string can be modified through the function.

---

## 🛠️ 3. Steps to Solve

### 3.1 Download and Extract Locally
```bash
cd ~/Downloads
wget <challenge-url> -O fixme2.tar.gz
tar -xzf fixme2.tar.gz
cd fixme2
```

### 3.2 Read the Source Code
```bash
cat src/main.rs
```

The bugs are clearly highlighted by comments in the code:

**Bug 1** — Function signature takes immutable reference:
```rust
fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &String) {
    // ...
    borrowed_string.push_str("PARTY FOUL! Here is your flag: "); // ERROR: cannot mutate
```

**Bug 2** — Variable declared without `mut`:
```rust
let party_foul = String::from("Using memory unsafe languages is a: "); // not mutable!
decrypt(encrypted_buffer, &party_foul); // passing immutable reference
```

### 3.3 Fix the Code

Two fixes needed:

**Fix 1:** Change `&String` to `&mut String` in the function signature:
```rust
fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
```

**Fix 2:** Declare `party_foul` as `mut` and pass as `&mut`:
```rust
let mut party_foul = String::from("Using memory unsafe languages is a: ");
decrypt(encrypted_buffer, &mut party_foul);
```

### 3.4 Apply the Fix
```bash
cat > src/main.rs << 'EOF'
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
    let key = String::from("CSUCKS");
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;
    }
    let xrc = res.unwrap();
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}

fn main() {
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "0d", "c4", "60", "f2", "12", "a0", "18", "03", "51", "03", "36", "05", "0e", "f9", "42", "5b"];
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();
    let mut party_foul = String::from("Using memory unsafe languages is a: ");
    decrypt(encrypted_buffer, &mut party_foul);
}
EOF
```

### 3.5 Build and Run
```bash
cargo run
```

Output:
```
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{4r3_y0u_h4v1n5_fun_y31?}
```

---

## 🧩 4. Final Flag

```
picoCTF{4r3_y0u_h4v1n5_fun_y31?}
```

---

## 📚 5. Key Learnings

- In Rust, **all variables are immutable by default** — you must explicitly add `mut` to make them mutable
- To pass a value to a function so it can be **modified**, you must pass a **mutable reference** (`&mut T`)
- Passing `&String` gives an immutable reference — you can read but not modify the value
- Passing `&mut String` gives a mutable reference — the function can modify the original value
- Rust enforces these rules at **compile time** — memory safety bugs are caught before the program runs
- The comments in the code were direct hints — always read comments carefully in fixme challenges

---

## 🚀 6. Improvements for Next Time

- When you see `push_str()`, `push()`, or any mutating method on a borrowed value — check if the reference is `&mut`
- Look for the `mut` keyword on both the **variable declaration** and the **function parameter**
- The Rust compiler error messages are very helpful — read them carefully, they tell you exactly what to fix
- Rust borrowing rules: `&T` = immutable borrow, `&mut T` = mutable borrow, `T` = ownership transfer
- For Rust fixme challenges, always read the inline comments — they're intentional hints

---

## 🔗 7. References

- [Rust Book – References and Borrowing](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html)
- [Rust Book – Mutable References](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html#mutable-references)
- [xor_cryptor crate](https://crates.io/crates/xor_cryptor)
- [Cargo Documentation](https://doc.rust-lang.org/cargo/)
- [picoCTF General Skills Challenges](https://picoctf.org)