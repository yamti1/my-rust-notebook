# References and Ownership

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

## Slices

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

### Array Slices

```
let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];
let slice2: &[i32] = &a[1..2];  // With type annotation
```
