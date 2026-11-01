### Background

Very often, we may want to start an individual thread with `fork` block in a loop. And also very often, if we use looped variables in `fork` block, things may not work as expected. In the following example, we will see `do_something()` is always applying to the last looped node in `nodes` array.

```systemverilog
// Example of a bad usage
foreach (nodes[i]) begin
    Node node = nodes[i];
    fork
        do_something(node);
    join_none
end
```

To solve this issue, two things we should know and understand: `fork` working mechanism and `automatic` variable.

### When do `fork` threads start?

It's a kind of tricky must-know. According to [SystemVerilog LRM (2023)](https://ieeexplore.ieee.org/document/10458102) *9.3.2 Parallel blocks*:

> In all cases, processes spawned by a fork-join block shall not start executing **until the parent process is blocked or terminates**.

The key point here is a thread does not start immediately when it reaches `fork`. For `fork-join` and `fork-join_any` it's quite straight-forward because the parent thread is blocked when it reaches `join` or `join_any`.

But `join_none` does not block the parent thread, so in the above example, `do_something()` would not start in each loop, until they see something blocking after the `foreach` loop. At that time they starts, the variable `node` has been assigned to the last looped node for each thread. That's why we see the same results for each thread.

### `automatic` vs `static` variable

`automatic` and `static` are variable lifetime modifiers.

`automatic` means the variable only lives in current scope, which is the default behavior of many other programming languages. According to [SystemVerilog LRM (2023)](https://ieeexplore.ieee.org/document/10458102) *6.21 Scope and lifetime*:

> Class methods and declared `for` loop variables are by default automatic, regardless of the lifetime attribute of the scope in which they are declared.

`static` means the variable is *re-used* among each declaration or access in the same scope, like how the C `static` works. **This is the default lifetime in SystemVerilog.**

Back to the example, `node` is static by default, so each `fork` context shares the same `node` variable, which is the last looped node in this case.

```systemverilog
// Example of a bad usage
foreach (nodes[i]) begin
    Node node = nodes[i]; // static variable by default, reused in each loop
    fork
        do_something(node);
    join_none
end
```

To assign and use a new `node` variable in each loop and each `fork` context, we should explicitly mark the variable as `automatic`.

```systemverilog
// Best practice
foreach (nodes[i]) begin
    automatic Node node = nodes[i]; // explicit automatic lifetime
    fork
        do_something(node);
    join_none
end
```

### Conclusion

The gap here is the default lifetime is different from other languages like C, and the `fork` block is not as intuitive as it looks like. I suggest to go through the whole *6.21 Scope and lifetime* section to understand the lifetime model.

### Read more

- [[Automatic variables in fork - SystemVerilog - Verification Academy]](https://verificationacademy.com/forums/t/automatic-variables-in-fork/31041)