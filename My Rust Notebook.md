# My Rust Notebook

## Constants
```
const SERVER_PORT: u32 = 8000;
```
Contants are evaluated at compile time.


## Collections

### Tuples

```
let tup = (500, 6.4, 1);
let tup: (i32, f64, u8) = (500, 6.4, 1);  // With type annotations
let (x, y, z) = tup; 					  // Tuple destructuring
let five_hundred = tup.0; 				  // Direct elemet access
let six_point_four = tup.1;
```

### Arrays

```
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];	// With type annotations
let a = [3; 5]; 					// Same as `let a = [3, 3, 3, 3, 3];`
let first = a[0]; 					// Element access
let error = a[5]; 					// Access to an index too big causes panic
```

## Functions

```
// Decleration
fn add(a: i32, b: i32) -> i32 {
    a + b 	// No semicolon to mark retrun expression
}

// Call
let sum = add(5, 6);
```

## Control Flow

### Conditions
```
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}


// if is an expression:
let number = if condition { 5 } else { 6 };

```
* The condition must be bool.

### Loops

#### Loop

Loop forever. Stops on `break` or on external signal.
```
loop {
	println!("again!");
}

// loop is an expression
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {}", result);
}
```

#### For Loop

Iterate over a collection
```
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

Iterate over a range
```
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

#### While Loop

```
while number != 0 {
    println!("{}!", number);

    number -= 1;
}
```

## References

Normal references are immutable. 
Holding a reference to something means you do not own it.
```
fn foo(val: &i32) {          // val is a reference to i32.
    println!("{}", val)
    val = val + 1;          // Illegal. val is an immutable reference.
}                           // val is not destroyed at the end because we do not own it.

// Calling:
let mut a = 5;
foo(&a);
```

Mutable references allow mutation of the referee.
```
fn foo(val: &mut i32) {     // val is a mutable reference to i32.
    println!("{}", val)
    val = val + 1;          // OK
}                           // val is not destroyed at the end because we do not own it.

// Calling:
let mut a = 5;
foo(&mut a);
```
At any given time, you can have either one mutable reference or any number of immutable references.

**Note** that references live in a scope untli their last usage in it. So this code is leagal:
```
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{} and {}", r1, r2);
    // r1 and r2 are no longer used after this point

    let r3 = &mut s; // no problem
    println!("{}", r3);
}
```

**Also note** that Rust makes sure that references are always valid.

### Slices

```
fn main() {
    let s = String::from("hello world");

    // String slices:
    let hello = &s[0..5];
    let world: &str = &s[6..11];  // With type annotation
}


// Slice from start:
let my_slice = &s[..4];

// Slice to end:
let my_slice = &s[3..];

// Slice from start to end:
let my_slice = &s[..];
```

Slices are immutable references so Rust makes sure that Slices are always valid.

**Note** that string literals are string slices.

#### Array Slices

```
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
let slice2: &[i32] = &a[1..2];  // With type annotation
```

## Structs

```
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```
Field Init shorthand:
```
fn build_user(email: String, username: String) -> User {
    User {
        email,          // instead of `email: email`
        username,       // instead of `username: username`
        active: true,
        sign_in_count: 1,
    }
}
```
Struct Update syntax:
```
let user2 = User {
    email: String::from("another@example.com"),
    username: String::from("anotherusername567"),
    ..user1
};
```
Tuple structs:
```
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Color(0, 0, 0);
let origin = Point(0, 0, 0);
```
Adding functionallity to structs:
```
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

## Enums
```
enum IpAddrKind {
    V4,
    V6,
}

let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```
Adding data to an enum:
```
enum IpAddr {
    V4(u8, u8, u8, u8),,
    V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
```
Methods can be defined on enums the same way as on structs.

### The Option Enum
A bult-in enum that provides null-safety:
```
enum Option<T> {
    Some(T),
    None,
}
```
Examples:
```
let some_number = Some(5);
let some_string = Some("a string");

let absent_number: Option<i32> = None;
```

### Pattern Matching
```
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```
Allows for a default pattern:
```
let some_u8_value = 0u8;
    match some_u8_value {
    1 => println!("one"),
    3 => println!("three"),
    5 => println!("five"),
    7 => println!("seven"),
    _ => (),
}
```
if-let:
```
if let Some(3) = some_u8_value {
    println!("three");
}
```

## Collections

### Vectors



