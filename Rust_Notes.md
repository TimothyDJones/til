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

This gives us a chance to note that Rust has excellent documentation for the language itself and the built-in libraries, called **crates** in the Rust vernacular. Most of the time, you will use modules from the [`core`](https://doc.rust-lang.org/core/) crate, which is the base underlying language code, including the primitive types (see below), and the [standard (so-called `std`)](https://doc.rust-lang.org/std/) crate, which contains the most common functions needed for day-to-day development, such as string formatting, composite data structures (so-called "generics"), collections, and asynchronous programming features. Keep the link to the `std` library handy, as you'll refer to it often.

#### Namespaces in Rust

You probably noticed the double colon (`::`) separator between `std` and `fmt` when referencing the formatting (so-called `fmt`) library. What does it represent? Many programming languages, including Rust, use the concept of **[namespaces](https://en.m.wikipedia.org/wiki/Namespace)**. Without getting into all of the technical details of namespaces, the main thing to understand is that they define a context in which the names of things, such as modules like `fmt` and its children are unique. This allows us reference these items in such a way that we know which _specific_ `fmt` module we are talking about in the case that another (different) crate or library also uses contains a module named `fmt`. To ensure that we use the one from the `std` library, we can always use the "long" name: `std::fmt`. 

Think about an office or classroom that has more than one Sally. We need to use the last name (surname) of each to ensure that we know which particular one we are addressing, when speaking to them. Namespaces work precisely the same way in programming, except that we usually put the "surname" on the front (sort of like in most Asian cultures).

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
If you are new to programming, the concept of a **variable** may be confusing to you at first. The best way to think about variables in programming is to think of like variables in an algebraic equation from math (even if you didn't enjoy math!). Just like `x` in the equation `2x - 3 = 5` simply represents a value (in this case `x = 4`), a variable in a program is just a placeholder for a value.

### Mutable and Immutable Variables

If you have some programming experience, Rust treats variables a bit differently than in many languages. In most other programming languages, variables can be changed (called **mutable**) by default after they have set to an initial value (so-called **initialized**). However, in Rust, unless you _explicitly_ declare that a variable _can_ be changed, it is **immutable** (or a **constant**).

In our earlier example program, all of our variables (`my_name`, `my_age`, `my_home`, and even `sentence`) were defined as _immutable_ using the `let` directive. Try to change to change the `my_age` variable after you have created/initialized it (make yourself younger!) and click **Run**. What does the Rust compiler do? While you may get other errors, as well, you will certainly get the following error (or similar).
```rust
error[E0384]: cannot assign twice to immutable variable `my_age`
 --> src/main.rs:4:5
  |
3 |     let my_age = 57;
  |         ------
  |         |
  |         first assignment to `my_age`
  |         help: consider making this binding mutable: `mut my_age`
4 |     my_age = 37;
  |     ^^^^^^^^^^^ cannot assign twice to immutable variable
```

As you begin your journey to learn Rust, you will likely become quite familiar with [`E0384` error code](https://doc.rust-lang.org/error_codes/E0384.html). Basically, the compiler tells us that the default for the variable is that it is _immutable_ and, therefore, cannot be changed.

But variables are are _meant_ to be changed! That's what programs do. You're absolutely right. To ensure safety, Rust wants us to be intentional about making our variables _mutable_, so it requires the keyword `mut` following `let` to do this.

Let's update our earlier program so that we can _change_ our home (since we really can't make ourselves younger!) to a place that we'd _like_ to live. Note that we must make **both** `my_home` _and_ `sentence` mutable in this case.

```rust
fn main() {
    let my_name = "Tim";
    let my_age = 57;
    let mut my_home = "Tulsa, OK";
    let sentence_termination = '!';
    let mut sentence = format!("Hi, I'm {} from {} and I'm {} years old",
		my_name, my_home, my_age);
	println!("{}{}", sentence, sentence_termination);
	my_home = "an Alpine village";
	sentence = format!("Hi, I'm {0} and I'm {2} years old.\nI'd like to live in {1}",
	    my_name, my_home, my_age);
	println!("{}{}", sentence, sentence_termination)
}
```

When you run this in the **Rust Playground**, what does the output look like? Is it what you expected? It should look something like this:
```rust
   Compiling playground v0.0.1 (/playground)
    Finished dev [unoptimized + debuginfo] target(s) in 0.65s
     Running `target/debug/playground`

Hi, I'm Tim from Tulsa, OK and I'm 57 years old!
Hi, I'm Tim and I'm 57 years old.
I'd like to live in an Alpine village!
```

Just a few brief comments about some of the other items that you may have noticed in our new code.

In line #10, instead of the _empty_ curly braces (`{}`), we now have numbers in the braces. These numbers correspond to the _position_ of our parameters: `my_name`, `my_home`, and `my_age`. This allows us to change order that the parameters are displayed/output in `sentence` _without_ having to change the order of the parameters themselves in the `format!()` macro, helping us maintain consistency between the two definitions of `sentence` variable. 

In addition, note that the position IDs start with 0 (instead of 1). As with most programming languages, all **indexes** in Rust are _zero-based_. This may seem unnatural to new programmers, but as you gain experience, you will start to see the benefit when it comes to iterating over a list (collection) of items.

The second (last) `println!()` statement (line #12) is not terminated with a semi-colon (`;`). I noted earlier that each (and every!) independent status **must** end with a semi-colon. While this is the preferred practice, the final statement in a block (or function) does not require the semi-colon. Later, when we look at functions that return a value, we'll see why it's common to leave off the semi-colon on the last statement in Rust functions. For now, simply know that this is a common practice in the Rust community, so don't be surprised by it, if you see it when reading others' code!

The re-defintion of `sentence` (line #10) includes a "newline" (`\n`) character embedded in the text so that the two sentences are displayed on separate lines in the output. Just as you can put any _printable_ character, including any of the thousands of [Unicode characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters), such as emojis 😃, control characters can be included as well. Here are some common control characters and entities.
| Representation | Definition | Unicode code point |
|:--------------:|:----------:|:------------------:|
| \t             | TAB        |                    |
| \n             | LF         |                    |
| \r             | CR         |                    |
| –              | en dash    | U+02013            |
| –              | em dash    | U+02014            |
| ×              | multiplication | U+000D7        |
| ÷              | division   | U+000F7            |
| °              | degree     | U+000B0            |
| •              | bullet     | U+02022            |

### Comments in Rust
All good programmers know that _judicious_ use of comments in code is a best practice and an important part of writing good code. I have a whole philosophy around commenting, but the overarching principle is: when in doubt, put in a comment. To me, it's better to have a note about _why_ you did something than not. (The basic idea for comments is to explain **WHY** and **NOT** what! Your code should stand on it's own with respect what; if it doesn't, then you need to simplify it!)

Rust supports three basic comment types, all of which are derived from comment formats of C++.
1) Line comments: These comments begin with two slashes: `//`. They can encompass the entire line or be placed after code as a brief note. Either way, everything _on that line_ after the double-slash is ignored by the compiler and essentially treated as whitespace.
```rust
    // Initialize variables
    let top: i8 = 0
    let bottom: i8 = 40
    let mut current: i8 = 0   // Pointer to current line.
```
2) Block comments: These comments can extend over _multiple_ lines (unlike line comments) and begin with `/*` and end with the _next_ `*/`. Block comments can encompass only part of a line and can include code both _before_ and _after_ the comment, if desired.
```rust
fn count_balls(blue: u16, red: u16) -> u32 {
	/* Note: `blue` and `red` are u16, so we 
	   must cast to u32 for calculation! */
	
	return (red as u32) + (blue as u32)
}
```
3) Document (or "Doc") comments: These comments are used by external documentation tools to automatically generate documentation for your code. This is especially useful when you get to the point of developing your _own_ crates. For others to use crates effectively, good documentation is an absolute necessity. Doc comments start with _three_ slashes (`///`) and are otherwise just like line comments, except that they will typically be multiple lines to explain behavior, arguments, etc.
```rust
/// A function to count the number of balls
/// currently in play by both the blue and
/// red teams.
/// :param: blue -> u16
/// :param: red -> u16
/// :returns: u32
fn count_balls(blue: u16, red: u16) -> u32 {
	return (red as u32) + (blue as u32)
}
```

## Primitive Types in Rust
Now that we know a little about how variables work in Rust, we can start to consider what kinds of variables we can use. In programming, the standard terminology for these kinds of entities is **types**. Rust is:
(a) [**strongly typed**](https://www.techtarget.com/whatis/definition/strongly-typed): With strong typing, variables of _different_ types, such as integers and decimal numbers, cannot be _implicitly_ combined; the programmer must _explicitly_ **cast** the value from one type to the other. This is one of the well-known and key strengths of Rust, as it ensures no variables can be unwittingly overwritten or used incorrectly. 
(b) [statically typed]: Static (versus dynamic typing, which is used by languages like Python) is related to strong typing, but subtly different. Static typing means that the _types_ of all variables are defined (and known) at the time the program is compiled. This allows the compiler to ensure that all operations with these variables are appropriate (_syntactically_ correct) for each. Likewise, the compiler can validate that the types for all parameters passed as arguments to functions are the correct type.

Rust provides a rich, yet standard set of **primitive** (very basic) data types, including:
- Numbers
  - Integers
    - Signed (positive and negative values)
    - Unsigned (positive only values)
  - Decimal numbers called floating-point numbers
- Text
  - Strings (ordered sequences of text or other symbolic characters)
  - Characters



### Integers
Integers are just the same integers that you learned about in algebra class: the positive and negative whole numbers along with zero (0). Rust provides for two different classes of these: one that is _signed_, meaning that it includes both positive and negative values and the other that is _unsigned_ which has only positive values and 0. 

You might wonder why we need both. The benefit is that we can have a much larger "maximum" value with the _unsigned_ values for each of the different number of bits of precision. The bits of precision define the number of bits of memory that Rust must use to store the value and, accordingly, the maximum (and, in the case of the _signed_ integers, the minimum) values allowed.

#### Underscore (`_`) as Separator for Integers
One handy feature of integers in Rust is the ability to use the underscore (`_`) as a separator to make integer constant values easier to read. Rust makes no restriction on the number of consecutive underscores or where you can place them (except they cannot be the beginning or ending character), but the standard is to use them as separator for each three orders of magnitude. For example, all of the following are equivalent as far as Rust is concerned: 1234567890, 1_234_567_890, 1__234___567____890, and 12345______________67890.

#### Signed Integers
Rust has _six_ types of signed integers (as of the latest versions). Each of them are identified with the type prefix of `i`: `i8`, `i16`, `i32`, `i64`, `i128` and `isize`. If we create an integer variable (again, using `let`) without specifying the type explicitly the default type is is `i32`. The minimum and maximum values for each type are -2^(n-1) and 2^(n-1) - 1, respectively, where n is the number of bits in the type name (e.g., 8, 16, 32, 64, or 128).
| Type | Minimum Value | Maximum Value | Comments                 |
|:----:|--------------:|--------------:|:-------------------------|
| i8   | -128          | 127           |                          |
| i16  | -32768        | 32767         |                          |
| i32  | -2147483648   | 2147483647    | This is the size of `isize` for **32-bit** architecture machines. |
| i64  | -9223372036854775808   | 9223372036854775807    | This is the size of `isize` for **64-bit** architecture machines. |
| i128 | -170141183460469231731687303715884105728 | 170141183460469231731687303715884105727 | |

#### Unsigned Integers
Unsigned integers are very similar to the signed, but they use the type prefix of `u` and we have the same _six_ "subtypes". One of the main uses of _unsigned_ integers is in counting (or indexing) items or elements of lists or collections. Thus, the minimum value for each of these is 0 and the maximum value is 2^n - 1, where, again, n is the number of bits in the type name. (The maximum value is just one more than _twice_ the maximum of the corresponding _signed_ integer.)
| Type | Minimum Value | Maximum Value | Comments                 |
|:----:|--------------:|--------------:|:-------------------------|
| u8   | 0             | 255           |                          |
| u16  | 0             | 65535         |                          |
| u32  | 0             | 4294967295    | This is the size of `usize` for **32-bit** architecture machines. |
| u64  | 0             | 18446744073709551615   | This is the size of `usize` for **64-bit** architecture machines. |
| u128 | 0             | 340282366920938463463374607431768211456 | |

You may be wondering what happens if we need integers larger than i128/u128. The Rust community has us covered with several open-source crates for such needs. Just head over to the official Rust package archive, [crates.io](https://crates.io/), to look for what you might need.

To create variables with an _explicit_ type, we simply put a colon (`:`) after the variable name followed by the type designation and then the assignment operator (the equals sign). For example, we can make the following variable assignments:
```rust
  let my_i8: i8 = 25;
  let my_other_i8: i8 = -17;
  let my_u16: u16 = 12345;
  let mut my_mut_u32 = 8675309;		// No type designation!
```

Note that in the last declaration, we did not specify a type, so it will default to `u32` and `u32` is, in fact, large enough to hold this value. If we tried to 



#### Math with Different Integer Types
Rust supports the four common arithmetic operations plus the [modulus](https://en.m.wikipedia.org/wiki/Modulo) (or remainder) operation using integers.
- Addition: Using the `+` operator.
- Subtraction: Using the `-` operator.
- Multiplication: Using the `*` operator.
- Division: Using the `/` operator. Note that any remainder as a result of the division in lost (truncated).
- Modulus: Using the `%` operator. Returns the remainder of the division of the first number by the second number. (Modulus only makes sense for 

As we mentioned when discussing _strong typing_, Rust does not allow us implicitly mix types when combining variables, such as doing math operations with integers. Thus, we can't just add an i8 to i16 (or even an u8!). So how do we work with them, since certainly there will be times when we have incompatible types? Rust provides a clean way to do this called **type casting**. (No, this isn't the same as [Michael Shannon](https://en.m.wikipedia.org/wiki/Michael_Shannon) usually playing bad guys!) Type casting allows us to convert one type to another to make it compatible with another.

To use type casting we simply reference our variable name with a particular type followed by `as other_type` where other_type is the compatible type that we want convert the value to. For example, if we have `let balls: i16 = 12` and we want to use it as a `u32`, we would say `balls as u32`. We showed a simple example of this above when discussing comments, as well, where the variables were `u16` type and we cast them to `u32`. Often, we will "wrap" the type casts in parentheses for clarity, but this is not required; it simply indicates that the type cast should be done _before_ the other operations.

Now, let's look at some math operations as examples. We use comments to annotate what's happening for brevity (and because it helps us practice using them!).
```rust
fn main() {
    /* Some integer math examples. */
    let a: i16 = 12;
    let b: i16 = -3;
    let c = 23; 				// c is `u32`, the default integer type!
    let d: i32 = -17;			// d is _explicitly_ declared as `i32`.
    
    // addition
    let sum1 = a + b;			// 9 as `i16`, since both are `i16`.
    let sum2 = b + (c as i16);	// 20 as `i16`

	// subtraction
	let diff1 = c - (a as i32);	// 11 as `i32`
	let diff2 = a - 5;			// 7 as `i16`; Rust will automatically make a literal the same type.
	
	// multiplication
	let prod1 = a * b;			// -36 as `i16`
	let prod2 = (b as i32) * d;	// 51 as `i32`
	let prod3 = ((a * b) as i32) * c;		// -828 as `i32`; we can cast an intermediate *result* too.
	
	// division
	let quot1 = a / b;			// -4 as `i16`
	let quot2 = c / d;			// -1 as `i32`; remainder truncated!
	
	// modulus
	let mod1 = (c as i16) % a;	// 11 as `i16`
}
```

If you run the above code in the **Rust Playground** (or Rust compiler), you will get several _warnings_ about unused variables. This is because we define the various results, such as, `sum1`, `diff2`, etc., but never do anything with them, like print them. If you find this annoying or disconcerting, you may supress these warnings by adding the `#[allow(unused)]` directive at the top of your code. However, this can be dangerous _general_ practice, because it may result in you unintentionally leaving a variable unused!

In the comments above, we made claims the _types_ of the result variables. But how can we check to ensure that these claims are correct? Rust has a couple of ways to do this. The most common (and obvious!) way is to use the `type_of()` function.




### Floating Point Numbers
For numbers with a decimal point that include a fractional part, Rust has so-called **floating-point numbers**. The name comes from the computer science term [floating-point arithmetic](https://en.m.wikipedia.org/wiki/Floating-point_arithmetic), which is how computers represent these numbers accurately when all of the internal calculations are done with binary (base 2) numbers. At this point, we don't need to concern ourselves with all of the details, but just that Rust has two types of floating-point values, per the IEEE-754 standard, with the `f` type prefix: `f32` and `f64`. The current versions of Rust default to `f64`, since on modern (64-bit) processors performance is about the same as for `f32`.

### Strings
Strings represent textual elements of more than one character in Rust. For historical reasons, programmers call a sequence of text (or other symbolic) characters with a particular order ["strings"](https://en.wikipedia.org/wiki/String_(computer_science)). (This naming _probably_ comes typesetting in which characters were "strung" together into words and lines for page layout.)

#### Characters
For individual (single) characters in Rust, we use single quotes to delimit (identify) them. (You can remember this by: Single character --> Single quotes!)
