## Rust Principles
- Excellent compiler error messaging and debugging guidance.
- Rust is a systems programming language. However, it is also widely used in other applications, such as Internet server backend via several excellent frameworks.
	- [Rocket](https://rocket.rs/)
	- [Actix](https://actix.rs/)
	- [Axum](https://github.com/tokio-rs/axum)
	- [hyper.rs](https://hyper.rs/)
- Rust emphasizes program correctness and safety. The basic idea is that if your program compiles and runs, it is safe/secure.
- 

## Project Structure
- mod.rs
- main.rs
- lib.rs

## A Simple Example (with Simple Errors)
Before we get into the details of learning Rust, let's look at a simple example that goes a little beyond the typical "Hello, World!" program. In this small program, we'll display some simple information about ourselves. While very basic, it will help us understand some of the fundamental elements of all Rust programs. Likewise, we'll see how the compiler error messages look and 

For this example, we'll use the the [Rust Playground](https://play.rust-lang.org/), which is the official online Rust interactive compiler. It is an excellent resource for experimenting with simple Rust programs or trying out new ideas.

Enter the following program in the editor window of the Rust Playground. Feel free use your own information to make it more personal!

```rust
fn main() {
    let my_name = "Tim";
    let my_age = 57;
    let my_home = "Tulsa, OK"
    let sentence_termination = "!";
    sentence = ("Hi, I'm {} from {} and I'm {} years old" 
		my_name, my_home, my_age);
	println("{}{}", sentence, sentence_termination);
}
```

### Analyzing the Errors and Debugging
Press the **Run** button. After a few seconds, the _Execution_ window at the bottom of the display will show the following (or something _very_ similar). We will go through the each of the errors to learn how the Rust compiler displays the output and the useful help and guidance that it provides to quickly resolve them.

```rust
   Compiling playground v0.0.1 (/playground)
error: expected `;`, found keyword `let`
 --> src/main.rs:4:30
  |
4 |     let my_home = "Tulsa, OK"
  |                              ^ help: add `;` here
5 |     let sentence_termination = "!";
  |     --- unexpected token

error: expected one of `)`, `,`, `.`, `?`, or an operator, found `my_name`
 --> src/main.rs:7:3
  |
6 |     sentence = ("Hi, I'm {} from {} and I'm {} years old" 
  |                                                          -
  |                                                          |
  |                                                          expected one of `)`, `,`, `.`, `?`, or an operator
  |                                                          help: missing `,`
7 |         my_name, my_home, my_age);
  |         ^^^^^^^ unexpected token

error[E0425]: cannot find value `sentence` in this scope
 --> src/main.rs:6:5
  |
6 |     sentence = ("Hi, I'm {} from {} and I'm {} years old" 
  |     ^^^^^^^^
  |
help: you might have meant to introduce a new binding
  |
6 |     let sentence = ("Hi, I'm {} from {} and I'm {} years old" 
  |     +++

error[E0425]: cannot find value `sentence` in this scope
 --> src/main.rs:8:18
  |
8 |     println("{}{}", sentence, sentence_termination);
  |                     ^^^^^^^^ not found in this scope

error[E0423]: expected function, found macro `println`
 --> src/main.rs:8:2
  |
8 |     println("{}{}", sentence, sentence_termination);
  |     ^^^^^^^ not a function
  |
help: use `!` to invoke the macro
  |
8 |     println!("{}{}", sentence, sentence_termination);
  |            +

Some errors have detailed explanations: E0423, E0425.
For more information about an error, try `rustc --explain E0423`.
error: could not compile `playground` (bin "playground") due to 5 previous errors
```

The first thing to observe is that line number for each error is display in the left margin with pipe (`|`) characters to make it easy to identify which lines require correction. Each error begins with the label `error` (and a preceding blank line) and _sometimes_ includes an **error code**, such as `E0423`. You don't need to worry about the error codes for now; as you advance in your Rust learning journey, you'll gain experience with many of them. In the meantime, the Rust documentation contains a [complete index of the compiler error](https://doc.rust-lang.org/error_codes/error-index.html), if you want to learn more. Likewise, the error message shows the column (character) number on the specified line where the error begins and marks it with a caret (`^`).

As shown in the output, the first error, on line #4 (`let my_home = "Tulsa, OK"`) might be a little confusing, but the `help` message perfectly explains that we simply forget to end this **statement** (line) with a semi-colon (`;`). I included this error, because leaving off a semi-colon is probably the most common error of Rust programmers of _all_ levels of experience.

On the second error, on line #6, the compiler again guides us straight to the problem: a missing `,` after the **format string** for the `println!()` macro and before the `my_name` **parameter**.

The next error, _also_ on line #6, shows us that assigning a variable, in this case `sentence`, _requires_ us to use the `let` directive. We'll have more to say about variables soon, but for now, suffice it to say that we just need to add `let` to the beginning of this line. Likewise, note that, unlike some programming languages, Rust will detect multiple errors on the same statement (line) in many cases. However, as we'll soon see, it can't always find _all_ of the errors, especially when we have both **semantic** and **syntax** errors.

### Syntax versus Semantics
What do we mean when we talk about _syntax_ errors and _semantic_ errors? At the most basic level, a _syntax_ relates to the _language grammar_ and _rules_ enforced by the compiler. Accordingly, a syntax _error_ means that the construction of our program does not meet the guidelines required for the compiler to "understand" it and, therefore, to successfully compile the program. Thus, we sometimes refer to _syntax_ errors as _compile-time_ errors. Generally, we need to worry less about syntax errors, because the compiler will find them and, as we've seen in the case of Rust, even helps us resolve them.

In contrast, _semantics_ refers to the _meaning_ of the code. Therefore, a semantic _error_ is a mistake in the logic of the program. The program will compile and run fine, but it will not provide the desired result or output. These errors result from flaws in the thinking of the programmer or incorrect requirements and they tend to be harder to detect and correct, because we don't get help from the compiler!


## Install Rust Compiler and Development Tools

### Configure VS Code for Rust Development

## Rust Variables

### Integers

#### Signed Integers

#### Unsigned Integers


### Floating Point Numbers
Numbers with a decimal point that include a fractional part. Name comes from the computer science term [floating-point arithmetic](https://en.m.wikipedia.org/wiki/Floating-point_arithmetic), which is how computers represent these numbers accurately when all of the internal calculations are done with binary (base 2) numbers.

### Strings
Strings represent textual elements of more than one character in Rust. For historical reasons, programmers call a sequence of text (or other symbolic) characters with a particular order ["strings"](https://en.wikipedia.org/wiki/String_(computer_science)). (This naming _probably_ comes typesetting in which characters were "strung" together into words and lines for page layout.)

#### Characters
For individual (single) characters in Rust, we use single quotes to delimit (identify) them. (You can remember this by: Single character --> Single quotes!)
