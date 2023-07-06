# Functions

In `type-lang` all declarations are simple.

```
pub const value: u64 = 5;
 |- optional pub

pub const value: u64 = 5;
      |- mutability const or let

pub const value: u64 = 5;
            |- name

pub const value: u64 = 5;
                  |- optional signature

pub const value: u64 = 5;
                     |- assignment

pub const value: u64 = 5;
                       |- value
```

Functions are no different. Here is an example for adding two numbers.

```ts
pub const add = fn (one: i64, two: i64) i64 {
  return one + two;
} 
```

This declaration syntax allows closures to be defined exactly the same

```ts
let my_array = [1, 2, 3];
my_array = my_array.map(fn (x) u64 { 
  return x + 2; 
});
```


