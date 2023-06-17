# Variables

Variables can be declared either `const` or `let`. The qualifier `const` is immutable, while `let` is mutable.

`x` cannot be changed after being declared.
```ts
const x = 5;
let y = 5;
x = 1; // not allowed!
y = 1; // allowed!
```

---
All variables have a [type](./types.md).
We can explicitly state the type. This is called the signature.

signature of `x` being explicitly stated
```ts
const x: num = 5;
```

