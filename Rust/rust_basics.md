# **Rust**
These tips are from "The Rust Programming Language" by Steve Klabnik and Carol Nichols

## **Baby steps**
### **Hello, World**
Hello world is done as follows:

```rust
fn main() {
    println!("Hello, world!");
}
```

This is then run by first compiling in the terminal and then running the program. 

```bash
$ rustc filename.rs; ./filename
```
The compiler creates a file named `filename`. This is the file that is run. Must specify that `filename` is in your current directory.

### **Cargo**
"Cargo is Rust's build system and package manager." Cargo will automatically handle creating a lot of the project files needed for your code. 

```bash
$ cargo new hello_cargo
$ cd hello_cargo
$ ls -a
OUTPUT: 
.  ..  Cargo.toml  .git  .gitignore  src
```

* Cargo.toml is the file to handle and add dependencies (libraries). 
* The hidden git files are obviously for git
* src is where the main.rs file is located. This the primary rust file. 

Once the Rust file(s) is altered as desired, the entire project can be built (compiled) using `cargo build` or `cargo run`. `cargo build` works just fine, but in order to see the resulting output you need to specify the path: `$ ./target/debug/hello_cargo`. `cargo run` compiles and returns the result. 

`cargo check` just checks to see if the code has compiled and will return errors if necessary. It will return the error if there are any. It is a quick way to see if there are any new errors after coding for a bit. 

### **Guessing Game**

```rust
use std::io; //from standard library

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new(); 

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```
Lot to take in here. 

* `use std::io;` -> lets rust know we are grabbing `io` library from the standard (`std`)library.
* `let mut guess...` -> `let` is used for variable, `const` for constants. `mut` is for a mutable variable. Sidenote: const only applies to the variable, not necessarily the object. Immutable is when objects can't be changed. 
* `String::new()`