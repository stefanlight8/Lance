I've never tried to make an own programming language. 

Previously I had many ideas how to implement a beautiful programming language which would make Rust, Go, Python and every other language useless, but I wasn't very smart to understand that there's no way to reach performance, simplicity in one language. Just technically not possible.
Rust is the most performance best programming language with personally for me easy syntax, I very like it.
Go is the most concurrent best and simple programming language.
But what about Python? Unfortunately Python in my opinion have not the best philosophy personally for me and it's very simple, but it doesn't have very high performance and have a lot of questionable things: `class MyEnumIs(enum.Enum)` and `@dataclass\nclass MyData:`. Lately last time when I used Python I was disgusted by amount of dynamic in behaviour of code. Where you can receive `int` instead of `str` or just a `None`. And when you use a non-typed library without stubs – yeah, that's quite a pain.

And every time when I think about own programming language every time Rust just closes any ideas. But at this time I thought: What if I will reimagine Python, but with own philosophy. 

# Meet concept of Lance
Lance is a statically-typed and data-oriented programming language focused on simplicity and strongness.

The key motivation of Lance to provide possibility to write easy and predictable code.

## Examples
### Comments / Docs
```
// This is comment

/// This is documentation for function devide
fn devide(x: int, y: int) -> int:
    x / y
```
### Read file to the buffer
```
use std.(fs.File, error.Error)

fn main() -> () | Error:
    let buffer: mut[byte] = []
    let file = File("README.md").open()! // ! - returns error into function body
    
    file.read(buffer)!
```
### Structs
```
use std.error.Error

struct Vec3:
    x: int
    y: int
    z: int

    fn move(self, x: int?, y: int?, z: int?) -> () ! Error:  // ? – makes type = T -> Option<T>
        if x.is_some():
            self.x += x! 
            // this is unsafe and can throw an error, but we checked already is that some or none

        if y.is_some():
            self.y += y!

        if z.is_some():
            self.z += z!
```
### Enums
```
type UserId = int

enum UserType:
    Default
    Super

struct User:
    id: UserId
    name: str
    type: UserType = UserType.Default

fn main():
    let users = mut[
        User { id: 1, name: "Michael" },
        User { id: 2, name: "Braben" },
        User { id: 3, name: "Fabrice" },
    ]

    for match user in users:  // sugar
        User(id: 1) => println("oh this is Michael!")
        User(id, name) => println("hello, {name}")

    users.push(User { id: 4, name: "Linus", type: UserType::Super })
```

* * *

To be continued. 

- Memory management: there's not much ideas, but I'd make something like a hybrid borrowing and GC.
  Like if we create data inside of function's scope and this data keeps inside it doesn't affected by GC, but when data leaves body function, like we create a Message and send it to another function we can send it completely or give a reference, where GC starts to work. But there's a question of GC type, because we don't want to fall into the same trap as Python with its GIL.
  And when we send data from function to struct/enum it becomes shared which means that it can be used everywhere.
  We could make an own GCs for each thread like in Nim (as I remember) or region-based (or context-aware) memory management, but I don't know much about that.
- External libraries: the most interesting is module system and external non-lance libraries, because for such language it would be good to have an unsafe pointers and possibilities to use for example C library (like raylib) with Lance and I have an idea where user will need to add library/file to Lance's configuration file (like `Lance.toml`) and in code `extern "C" module hello_world: fn hello_world() -> void:` (hello_world is the name from configuration file). Or we could make a build system based on build script, like in Zig.
- For some abstraction there must be traits.
- Async: To be honest maybe the most important part of language, because such languages is very useful in I/O programs such as back-ends, bots and everything wich somehow works with network or files, so it must be built in someway. I like Rust's async, because it's very predictable unlike in Python, but it's complex. And I wouldn't make a Go goroutines because it's hard to control them (afaik)
- Of course it will compile, but probably it will be just a binary with own small-runtime and it can be started with just a command, like a Go.
- The first version of Lance compiler will be on Rust and use cranelift/LLVM as backend. There's two possible ways:
  - Make a first version of compiler and build next compiler version on Lance and the first version will compile the second. Like Rust.
  - Or keep compiler on Rust with LLVM just because probably Lance compiler won't be so fast and memory efficient to compile itself.
    But probably Lance could have an unsafe pointers/functions and etc, which could allow to reach maximum of own runtime.

If you're interested in project and have ideas/corrections/opinion you can create an issue or reach me on Bluesky: `@stefanlight.bsky.social`.
