# Types Intro

- `any` is the root type
- `num` is the scalar type
- `{}` is the object type
- `[]` is the slice type
- `()` is the function type

Every value type can be cast to `any`.

```
                any
       num | {} | [] | () 
```
If `any` were to be cast to any of the types underneath it, it could error if its underlying type did not have the properties to be that type.

No type can be laterally casted. This is why `any` casts can fail.

num to any to i64.

```ts
const x: num = 1;
const y = x as any;
const z = y as i64;
```

This works because 1 is a safe cast to i64.

```ts
const x: num = 1;
const y = x as any;
const z = y as {}; // !! what does this even mean?
```

Vizualization of how each type is comprised.
```
// num = f64, f32, d64, d32, u32, u64, i32, i64 ...
// {} = { <properties> }
// [] = []<type>
// () = fn(<args>) <type>
```

The `truthy` matrix is quite easy!

- 0 is false, any other number is true.
- an empty object `{}` is false, all others true.
- an empty slice `[]` is false, all others are true.
- every function `()` is false. (do not confuse this with actually executing a function!)

There are shadow types.

- `never` is the unreachable type
- `error` is the error type
- `undefined` is unaccessable type
- `void` is the empty type

`never` exits the program immediately if reached, or signifies that a function does exit.
`error` means that an error has happened.
`undefined` means that a value is null, or some other undefined behavior. It is impossible to get a value out of this type.
`void` is used for function returns that do not return any data to the caller.

Here is their truthyness.

`never`: since never exits, this wouldn't return true or false, it would exit the program.

```ts
if (1 == never) {}
```

`error`: is part of a data result. Therefore, when an error is being worked with directly, it will always be true.

```ts
const x = error {};
if (x) {}
```

`undefined`: is also part of a data result, but will always be false

`void`: It doesn't make sense to truthy check a void function, to help limit bugs with not understanding if a function returns a value or void, the linter will error when trying to compile truthy checking a void function return. 

