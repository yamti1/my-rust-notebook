# Collections

## Tuples

```
let tup = (500, 6.4, 1);
let tup: (i32, f64, u8) = (500, 6.4, 1);  // With type annotations
let (x, y, z) = tup; 					  // Tuple destructuring
let five_hundred = tup.0; 				  // Direct elemet access
let six_point_four = tup.1;
```

## Arrays

```
let a = [1, 2, 3, 4, 5];
let a: [i32; 5] = [1, 2, 3, 4, 5];	// With type annotations
let a = [3; 5]; 					// Same as `let a = [3, 3, 3, 3, 3];`
let first = a[0]; 					// Element access
let error = a[5]; 					// Access to an index too big causes panic
```

## Vectors

```
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
```
let mut v = vec![1, 2, 3, 4, 5];

let first = &v[0];

v.push(6);
```
### Vector Iteration
```
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
```
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