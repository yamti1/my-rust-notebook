# Constants
```
const SERVER_PORT: u32 = 8000;
```
Contants are evaluated at compile time.

# Functions

```
// Decleration
fn add(a: i32, b: i32) -> i32 {
    a + b 	// No semicolon to mark retrun expression
}

// Call
let sum = add(5, 6);
```

# Control Flow

## Conditions
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

## Loops

### Loop

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

### For Loop

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

### While Loop

```
while number != 0 {
    println!("{}!", number);

    number -= 1;
}
```
# Structs

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

# Enums
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

## The Option Enum
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

## Pattern Matching
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