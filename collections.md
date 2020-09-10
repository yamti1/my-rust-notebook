# Collections

## Tuples

```rust
let tup = (500, 6.4, 1);
let tup: (i32, f64, u8) = (500, 6.4, 1);  // With type annotations
let (x, y, z) = tup; 					  // Tuple destructuring
let five_hundred = tup.0; 				  // Direct elemet access
let six_point_four = tup.1;
```

## Arrays

```rust
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];	// With type annotations
let a = [3; 5]; 					// Same as `let a = [3, 3, 3, 3, 3];`
let first = a[0]; 					// Element access
let error = a[5]; 					// Access to an index too big causes panic
```

## Vectors

```rust
let v: Vec<i32> = Vec::new();   // New vector
let v = vec![1, 2, 3];          // New vector macro
v.push(5);                      // Add a value to the vector

// Element access
let third: &i32 = &v[2];        // Panics if out of range
match v.get(2) {                // Returns None if out of range
    Some(third) => println!("The third element is {}", third),
    None => println!("There is no third element."),
}
```
### Vector Ownership
The following won't compile because mutating the vector while there is a reference to an element in it might cause the reference to become invalid (the vector might get reallocated).
```rust
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0];

v.push(6);
```
### Vector Iteration
```rust
let v = vec![100, 32, 57];
for i in &v {
    println!("{}", i);
}

// Iterating while mutating:
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;                   // Note the dereference operator (*)
}
```
### Different Type in a Vector with Enum
```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

## Strings

```rust
let mut s = String::new();

let s = "initial contents".to_string();
let s = String::from("initial contents");

// Pushing into Strings
let mut s = String::from("foo");
s.push_str("bar");
s.push('b');

// Concatination
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used

// Formatting
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = format!("{}-{}-{}", s1, s2, s3);
```

**Note** that Strings in Rust do not supppot indexing to access a single character because UTF-8 encoding might cause some unexpected results.
It is possible to slice a String with a range, but it should be done carefully, as it might cause panics:

```rust
let hello = "Здравствуйте";

let s = &hello[0..4];   // OK. s is "Зд"

let s = &hello[0..1];   // Panic! s cannot be a valid char.
```

### Iterating Over a String
Characters:
```rust
for c in "नमस्ते".chars() {
    println!("{}", c);
}

/* Output:
न
म
स
्
त
े
*/
```
Bytes:
```rust
for b in "नमस्ते".bytes() {
    println!("{}", b);
}

/* Output:
224
164
...
165
135
*/
```

## Hash Maps
```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// Only insert if key does not already exist
scores.entry(String::from("Blue")).or_insert(50);

// Value access. Returns an Option
let score = scores.get(&team_name);

// Iteration
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```
Creating using `collect`:
```rust
use std::collections::HashMap;

let teams = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

let mut scores: HashMap<_, _> =
    teams.into_iter().zip(initial_scores.into_iter()).collect();
```
**Note** that a hash map copies `Copy`able values (like `i32`) but takes ownership over owned values (like `String`).

