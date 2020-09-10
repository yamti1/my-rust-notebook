# Errors

## Panic
Crashes the program.
```rust
panic!("DIE!");
```

## Result
Allows graseful handling of errors
```rust
use std::fs::File;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}
```
Can also match on different errors:
```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt");

    let f = match f {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => {
                panic!("Problem opening the file: {:?}", other_error)
            }
        },
    };
}
```
This can also be writen using closures:
```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {:?}", error);
            })
        } else {
            panic!("Problem opening the file: {:?}", error);
        }
    });
}
```

### `unwrap` and `except`
```rust
// Panics if failed to open
let f = File::open("hello.txt").unwrap();

// Panics if failed to open, but with the given error message
let f = File::open("hello.txt").expect("Failed to open hello.txt");
```

### Error Propagation
```rust
use std::fs::File;
use std::io;
use std::io::Read;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();

    // The ? operator causes an error result to be returned automatically:
    File::open("hello.txt")?.read_to_string(&mut s)?;

    Ok(s)
}
```
Note that this example can actually be done with a single line:
```rust
use std::fs;
use std::io;

fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```
**Also note** that in the `main` function the `?` operator is allowed but the return type of the function should be `Result<(), Box<dyn Error>>`. `Box<dyn Error>` means "any kind of error".