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

