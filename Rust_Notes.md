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
    let sentence_termination = '!';
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
5 |     let sentence_termination = '!';
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

As shown in the output, the _first error_, on line #4 (`let my_home = "Tulsa, OK"`) might be a little confusing, but the `help` message perfectly explains that we simply forget to end this **statement** (line) with a semi-colon (`;`). I included this error, because leaving off a semi-colon is probably the most common error of Rust programmers of _all_ levels of experience.

On the _second error_, on line #6, the compiler again guides us straight to the problem: a missing `,` after the **format string** for the `println!()` macro and before the `my_name` **parameter**.

The _third error_, _also_ on line #6, shows us that assigning a variable, in this case `sentence`, _requires_ us to use the `let` directive. We'll have more to say about variables soon, but for now, suffice it to say that we just need to add `let` to the beginning of this line. Likewise, note that, unlike some programming languages, Rust will detect multiple errors on the same statement (line) in many cases. However, as we'll soon see, it can't always find _all_ of the errors, especially when we have both **semantic** and **syntax** errors.

### Syntax versus Semantics
What do we mean when we talk about _syntax_ errors and _semantic_ errors? At the most basic level, a _syntax_ relates to the _language grammar_ and _rules_ enforced by the compiler. Accordingly, a syntax _error_ means that the construction of our program does not meet the guidelines required for the compiler to "understand" it and, therefore, to successfully compile the program. Thus, we sometimes refer to _syntax_ errors as _compile-time_ errors. Generally, we need to worry less about syntax errors, because the compiler will find them and, as we've seen in the case of Rust, even helps us resolve them.

In contrast, _semantics_ refers to the _meaning_ of the code. Therefore, a semantic _error_ is a mistake in the logic of the program. The program will compile and run fine, but it will not provide the desired result or output. These errors result from flaws in the thinking of the programmer or incorrect requirements and they tend to be harder to detect and correct, because we don't get help from the compiler!

The _fourth error_, on line #8, is essentially a result of the previous error. Since we did not define the `sentence` variable properly, the statement in line #8 is "confused" about what we mean when referencing `sentence`. Thus, we will _implicitly_ resolve this error when correcting the previous error!

Finally, we come to the _fifth error_, also on line #8 (remember we can have multiple errors on the same line!). This shows another common error for beginners to Rust: forgetting that that the "println" directive is a **macro**, which requires the `!`.

### The Second Try (and **More** Errors!)
So let's fix the errors that we've identified and explained above. We should now have the following code in our editor.

```rust
fn main() {
    let my_name = "Tim";
    let my_age = 57;
    let my_home = "Tulsa, OK";
    let sentence_termination = '!';
    let sentence = ("Hi, I'm {} from {} and I'm {} years old",
		my_name, my_home, my_age);
	println!("{}{}", sentence, sentence_termination);
}
```

Click the **Run** button again and now everything should be... Whoops! We still get another error:

```rust
   Compiling playground v0.0.1 (/playground)
error[E0277]: `(&str, &str, &str, {integer})` doesn't implement `std::fmt::Display`
 --> src/main.rs:8:19
  |
8 |     println!("{}{}", sentence, sentence_termination);
  |                      ^^^^^^^^ `(&str, &str, &str, {integer})` cannot be formatted with the default formatter
  |
  = help: the trait `std::fmt::Display` is not implemented for `(&str, &str, &str, {integer})`
  = note: in format strings you may be able to use `{:?}` (or {:#?} for pretty-print) instead
  = note: this error originates in the macro `$crate::format_args_nl` which comes from the expansion of the macro `println` (in Nightly builds, run with -Z macro-backtrace for more info)

For more information about this error, try `rustc --explain E0277`.
error: could not compile `playground` (bin "playground") due to previous error
```

Hmmm... The compiler tells us that the error happened on line #8, the line with the `println!()` macro. However, the explanation indicates that the _real_ problem is with the `sentence` variable. What the message means, essentially, is that we cannot have both strings and numbers together in `sentence` without providing some additional clarification about what we are trying to do. Unfortunately, the `help` in this case does not guide us _directly_ to the solution, but it does give us a little hint that we should look at [`std::fmt` library](https://doc.rust-lang.org/std/fmt/). 

#### The Rust `core` and `std` Crates

This gives us a chance to note that Rust has excellent documentation for the language itself and the built-in libraries, called **crates** in the Rust vernacular. Most of the time, you will use modules from the `[core](https://doc.rust-lang.org/core/)` crate, which is the base underlying language code, including the primitive types (see below), and the [standard (so-called `std`)](https://doc.rust-lang.org/std/) crate, which contains the most common functions needed for day-to-day development, such as string formatting, composite data structures (so-called "generics"), collections, and asynchronous programming features. Keep the link to the `std` library handy, as you'll refer to it often.

#### Namespaces in Rust

You probably noticed the double colon (`::`) separator between `std` and `fmt` when referencing the formatting (so-called `fmt`) library. What does it represent? Many programming languages, including Rust, use the concept of **[namespaces](https://en.m.wikipedia.org/wiki/Namespace)**. Without getting into all of the technical details of namespaces, the main thing to understand is that they define a context in which the names of things, such as modules like `fmt` and its children are unique. This allows us reference these items in such a way that we know which specific `fmt` module we are talking about in the case that another (different) crate or library also uses contains a module named `fmt`. To ensure that we use the one from the `std` library, we can always use the "long" name: `std::fmt`. 

The double colon (`::`) is called the _namespace delimiter_. Each use of the `::` defines another level of the hierarchy of element within the namespace. Thus, we can uniquely identify the basic formatter macro with the full name of `std::fmt::format!()` (or just `std::fmt::format!`). Most of the time, it is **not** necessary for us to use the "full name", because from our program, it is obvious (clear) that we are only using a single namespace. Moreover, the `std` and `core` namespaces are always implicitly available.

So now that we know we need to use something from the `std::fmt` library, what _specifically_ do we need? Well, it turns out that we need the generic `format!()` **macro**, which behaves in much the same way as `println!()` macro in allowing us to use **positional parameters**, represented by the pairs of curly braces (`{}`) of differing types (two strings and an integer in our case) in our `sentence` string variable. Thus, we simply need to update **line #6** _only_ as follows:

```rust
    let sentence = format!("Hi, I'm {} from {} and I'm {} years old",
```

Now, when we click **Run** the program should compile and execute successfully with the following (or similar with your personal details!) output:
```rust
   Compiling playground v0.0.1 (/playground)
    Finished dev [unoptimized + debuginfo] target(s) in 0.56s
     Running `target/debug/playground`

Hi, I'm Tim from Tulsa, OK and I'm 57 years old!

```

### What Have We Learned So Far?
Violá! We have completed our first program! That probably seemed like quite a bit of work for such a small program (just one line of output!), but you should have learned several important concepts already.
- Rust programs must have a `main` function designated with `fn` keyword and a set of parentheses (`()`) for the argument list.
- The **body** of a Rust function (in our case, the `main` function) must be contained in a pair of curly braces (`{}`).
- All variables in a Rust program must be declared (defined) using the `let` keyword.
- Rust has a powerful **macro** system with the `println!()` and `format!()` macros having common, consistent behavior for outputting and formatting strings and data using parameter substition via curly braces (`{}`).
- Each independent statement in Rust should end with a semi-colon (`;`). (We will see later that just as with functions themselves, we can also sub-divide code sections with curly braces (`{}`).)
- The Rust compiler output is easy to read and provides excellent hints and guidance about solutions to common coding errors.

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
