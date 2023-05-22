# Async

Many languages have overly complex asynchronous control flow. Javascript and Typescript has a problem where once a function returns a `Promise`, everything else needs to become async, and a lot of custom handlers are introduced.

In `tlang` the runtime is asynchronous by default.

```ts
pub fn get_google_page() !string {
  const response = await http.get("https://google.com");
  return await response!.to_string();
}
```

Notice how the function and the return did not need to be marked as asynchronous. The `tlang` runtime is entirely asynchronous, and converting anything to async has no burden. Let us take for example,

```ts
const data = long_running_process();
const other_data = long_running_process2();
```

These two functions take a long time, and they are unrelated as `long_running_process2` has no dependencies on data.
In other languages, refactoring both of those long running processes to async can be tedious. Let us go through converting this to be asynchronous and we can explain what is happening within the `tslang` runtime.

```ts
pub fn long_running_process() Data {
  // lots of work.
  return data;
}
```

and the conversion,
```ts
pub fn long_running_process() frame Data {
  // lots of work.
  return data;
}

pub fn long_running_process2() frame Data {
  // lots of work.
  return data;
}
```

This simple change states that this function doesn't execute, it returns a `frame`. A `frame` is a block of code wherein the caller will manage the execution.

The reason why the initial example requires no change, is because the `await` keyword is used. See again, below.

```ts
pub fn get_google_page() !string {
  const response = await http.get("https://google.com"); 
  return await response!.to_string();
}
```

The `await` keyword, uses the default executor, and executes the function immediately.

Now, lets convert every invocation of `long_running_process` and `long_running_process2`, since they are now async.

instead of,
```ts
const data = long_running_process();
const other_data = long_running_process2();
```

now we have,
```ts
const data = await long_running_process();
const other_data = await long_running_process2();
```
Very easy to convert!

At first glance, nothing seems better about this change, as both processes are executed one after each other, because `await` immediately begins executing the `frame`.

The current file, or current block of code, is only one view of the program execution.

In fully `asynchronous` code, many threads are being executed concurrently. Long running processes or system IO can block the currently executing thread for long periods of time. This takes careful visualization to see, so just understand that converting a long process to a frame, and awaiting that frame still has benefit, in context of the overall program.

When we look at improving the performance of our long running processes, this code should be executed in parallel on an executor.

```ts
const data_frame = long_running_process();
const other_data_frame = long_running_process2();
const result = await thread [data_frame, other_data_frame];
```

We can choose our executors Here is an exhaustive list of executors.

- pool
- thread
- queue
- message & notify

`queue` is the default executor, and why `await` works by itself.
