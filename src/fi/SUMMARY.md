# The Rust Programming Language

[The Rust Programming Language](title-page.md)
[Foreword](foreword.md)
[Johdanto](ch00-00-introduction.md)

## Getting started

- [Aloittaminen](ch01-00-getting-started.md)
  - [Installation](ch01-01-installation.md)
  - [Hei, maailma!](ch01-02-hello-world.md)
  - [Hello, Cargo!](ch01-03-hello-cargo.md)

- [Programming a Guessing Game](ch02-00-guessing-game-tutorial.md)

- [Common Ohjelmointikäsitteet](ch03-00-common-programming-concepts.md)
  - [Variables and Mutability](ch03-01-variables-and-mutability.md)
  - [Data Types](ch03-02-data-types.md)
  - [Functions](ch03-03-how-functions-work.md)
  - [Comments](ch03-04-comments.md)
  - [Control Flow](ch03-05-control-flow.md)

- [Omistajuuden ymmärtäminen](ch04-00-understanding-ownership.md)
  - [What is Ownership?](ch04-01-what-is-ownership.md)
  - [References and Borrowing](ch04-02-references-and-borrowing.md)
  - [The Slice Type](ch04-03-slices.md)

- [Rakenteiden käyttö tietojen järjestämiseen](ch05-00-structs.md)
  - [Defining and Instantiating Structs](ch05-01-defining-structs.md)
  - [An Example Program Using Structs](ch05-02-example-structs.md)
  - [Method Syntax](ch05-03-method-syntax.md)

- [Enumit ja mallienmukaisuus](ch06-00-enums.md)
  - [Defining an Enum](ch06-01-defining-an-enum.md)
  - [The `match` Control Flow Construct](ch06-02-match.md)
  - [Concise Control Flow with `if let` and `let else`](ch06-03-if-let.md)

## Basic Rust Literacy

- [Kasvavien projektien hallinta paketeilla, cratella ja moduuleilla](ch07-00-managing-growing-projects-with-packages-crates-and-modules.md)
  - [Packages and Crates](ch07-01-packages-and-crates.md)
  - [Defining Modules to Control Scope and Privacy](ch07-02-defining-modules-to-control-scope-and-privacy.md)
  - [Paths for Referring to an Item in the Module Tree](ch07-03-paths-for-referring-to-an-item-in-the-module-tree.md)
  - [Bringing Paths Into Scope with the `use` Keyword](ch07-04-bringing-paths-into-scope-with-the-use-keyword.md)
  - [Separating Modules into Different Files](ch07-05-separating-modules-into-different-files.md)

- [Yleiset kokoelmat](ch08-00-common-collections.md)
  - [Storing Lists of Values with Vectors](ch08-01-vectors.md)
  - [Storing UTF-8 Encoded Text with Strings](ch08-02-strings.md)
  - [Storing Keys with Associated Values in Hash Maps](ch08-03-hash-maps.md)

- [Virheenkäsittely](ch09-00-error-handling.md)
  - [Unrecoverable Errors with `panic!`](ch09-01-unrecoverable-errors-with-panic.md)
  - [Recoverable Errors with `Result`](ch09-02-recoverable-errors-with-result.md)
  - [To `panic!` or Not to `panic!`](ch09-03-to-panic-or-not-to-panic.md)

- [Geneeriset tyypit, traitit ja eliniät](ch10-00-generics.md)
  - [Generic Data Types](ch10-01-syntax.md)
  - [Traits: Defining Shared Behavior](ch10-02-traits.md)
  - [Validating References with Lifetimes](ch10-03-lifetime-syntax.md)

- [Writing Automated Tests](ch11-00-testing.md)
  - [How to Write Tests](ch11-01-writing-tests.md)
  - [Controlling How Tests Are Run](ch11-02-running-tests.md)
  - [Test Organization](ch11-03-test-organization.md)

- [I/O-projekti: Komentoriviohjelman rakentaminen](ch12-00-an-io-project.md)
  - [Accepting Command Line Arguments](ch12-01-accepting-command-line-arguments.md)
  - [Reading a File](ch12-02-reading-a-file.md)
  - [Refactoring to Improve Modularity and Virheenkäsittely](ch12-03-improving-error-handling-and-modularity.md)
  - [Developing the Library’s Functionality with Test Driven Development](ch12-04-testing-the-librarys-functionality.md)
  - [Working with Environment Variables](ch12-05-working-with-environment-variables.md)
  - [Writing Error Messages to Standard Error Instead of Standard Output](ch12-06-writing-to-stderr-instead-of-stdout.md)

## Thinking in Rust

- [Funktionaaliset ominaisuudet: Iteraattorit ja sulkeiset](ch13-00-functional-features.md)
  - [Closures: Anonymous Functions that Capture Their Environment](ch13-01-closures.md)
  - [Processing a Series of Items with Iterators](ch13-02-iterators.md)
  - [Improving Our I/O Project](ch13-03-improving-our-io-project.md)
  - [Comparing Performance: Loops vs. Iterators](ch13-04-performance.md)

- [Lisää Cargo:sta ja Crates.io:sta](ch14-00-more-about-cargo.md)
  - [Customizing Builds with Release Profiles](ch14-01-release-profiles.md)
  - [Publishing a Crate to Crates.io](ch14-02-publishing-to-crates-io.md)
  - [Cargo Workspaces](ch14-03-cargo-workspaces.md)
  - [Installing Binaries from Crates.io with `cargo install`](ch14-04-installing-binaries.md)
  - [Extending Cargo with Custom Commands](ch14-05-extending-cargo.md)

- [Smart Pointers](ch15-00-smart-pointers.md)
  - [Using `Box<T>` to Point to Data on the Heap](ch15-01-box.md)
  - [Treating Smart Pointers Like Regular References with the `Deref` Trait](ch15-02-deref.md)
  - [Running Code on Cleanup with the `Drop` Trait](ch15-03-drop.md)
  - [`Rc<T>`, the Reference Counted Smart Pointer](ch15-04-rc.md)
  - [`RefCell<T>` and the Interior Mutability Pattern](ch15-05-interior-mutability.md)
  - [Reference Cycles Can Leak Memory](ch15-06-reference-cycles.md)

- [Turvallinen rinnakkaisuus](ch16-00-concurrency.md)
  - [Using Threads to Run Code Simultaneously](ch16-01-threads.md)
  - [Using Message Passing to Transfer Data Between Threads](ch16-02-message-passing.md)
  - [Shared-State Concurrency](ch16-03-shared-state.md)
  - [Extensible Concurrency with the `Sync` and `Send` Traits](ch16-04-extensible-concurrency-sync-and-send.md)

- [Fundamentals of Asynchronous Programming: Async, Await, Futures, and Streams](ch17-00-async-await.md)
  - [Futures and the Async Syntax](ch17-01-futures-and-syntax.md)
  - [Applying Concurrency with Async](ch17-02-concurrency-with-async.md)
  - [Working With Any Number of Futures](ch17-03-more-futures.md)
  - [Streams: Futures in Sequence](ch17-04-streams.md)
  - [A Closer Look at the Traits for Async](ch17-05-traits-for-async.md)
  - [Futures, Tasks, and Threads](ch17-06-futures-tasks-threads.md)

- [Rustin oliopohjaiset ohjelmointiominaisuudet](ch18-00-oop.md)
  - [Characteristics of Object-Oriented Languages](ch18-01-what-is-oo.md)
  - [Using Trait Objects That Allow for Values of Different Types](ch18-02-trait-objects.md)
  - [Implementing an Object-Oriented Design Pattern](ch18-03-oo-design-patterns.md)

## Advanced Topics

- [Mallit ja mallinmukaisuus](ch19-00-patterns.md)
  - [All the Places Patterns Can Be Used](ch19-01-all-the-places-for-patterns.md)
  - [Refutability: Whether a Pattern Might Fail to Match](ch19-02-refutability.md)
  - [Pattern Syntax](ch19-03-pattern-syntax.md)

- [Kehittyneet ominaisuudet](ch20-00-advanced-features.md)
  - [Unsafe Rust](ch20-01-unsafe-rust.md)
  - [Advanced Traits](ch20-02-advanced-traits.md)
  - [Advanced Types](ch20-03-advanced-types.md)
  - [Advanced Functions and Closures](ch20-04-advanced-functions-and-closures.md)
  - [Macros](ch20-05-macros.md)

- [Loppuprojekti: Monisäikeisen verkkopalvelimen rakentaminen](ch21-00-final-project-a-web-server.md)
  - [Building a Single-Threaded Web Server](ch21-01-single-threaded.md)
  - [Turning Our Single-Threaded Server into a Multithreaded Server](ch21-02-multithreaded.md)
  - [Graceful Shutdown and Cleanup](ch21-03-graceful-shutdown-and-cleanup.md)

- [Liite](appendix-00.md)
  - [A - Keywords](appendix-01-keywords.md)
  - [B - Operators and Symbols](appendix-02-operators.md)
  - [C - Derivable Traits](appendix-03-derivable-traits.md)
  - [D - Useful Development Tools](appendix-04-useful-development-tools.md)
  - [E - Editions](appendix-05-editions.md)
  - [F - Translations of the Book](appendix-06-translation.md)
  - [G - How Rust is Made and “Nightly Rust”](appendix-07-nightly-rust.md)
