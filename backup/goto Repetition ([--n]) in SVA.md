This article is basically a dev memo of [Ben Cohen's amazing tutorial on this topic](https://systemverilog.us/vf/goto_conseq.pdf) , followed by a practical live demo in my work.

### Basic usage and live demo

In a word, goto repetition describe a sequence of a boolean value `b` becoming True for `n` times in following cycles, and **don't care when or continuously or not**.

Given property `a |=> b[->2] ##1 c`:

```
       a: 1 - - - - - - -
       b: - 0 1 0 0 0 1 -
       c: - - - - - - - 1
property: |---^-------^-! // match
```

Let's say here's a counter of range 0 to max value 2. I want a property to trace the counter from 0 to 2 then back to 0, and the other values are all not-care.

```
   counter: 0 -> 1 -> 0 -> 0 -> 1 -> 2 -> 1 -> 2 -> 1 -> 0
 fail case: |---------! // fail
match case:                |---------^-------------------! // match
```

1. Starting point (antecedent)

It should start from `(cnt == 0)`:

```systemverilog
(cnt == 0) |=> // TODO
```

2. Finding 2

A value of 2 is expected in following cycles:

```systemverilog
(cnt == 0) |=> (cnt == 2)[->1] // TODO
```

3. Back to 0

After that let's get back to 0 and done:

```systemverilog
(cnt == 0) |=> (cnt == 2)[->1] ##1 (cnt == 0)[->1]; // Done!
```

OK. I believe this property can handle the most ideal case:

```
 counter: 0 -> 1 -> 2 -> 1 -> 0
property: |---------^---------! // match
```

But it has issues. One is I want it to fail when the counter falls back to 0 before reaching 2, and now it won't:

```
 counter: 0 -> 1 -> 0 -> 2 -> 1 -> 0
property: |---------?----^---------! // match
                    ^-I want it to fail here
```

To fix that, replace `(cnt == 2)[->1]` with a restriction of not seeing 0 before:

```systemverilog
(cnt == 0) |=> (cnt != 0)[*0:$] ##1 (cnt == 2) ##1 (cnt == 0)[->1];
```

Second issue is it will start a property once it sees 0, which is definitely not necessary:

```
 counter: 0 -> 0 -> 0 -> 1 -> 2
property: |-------------------^ // not necessary
               |--------------^ // not necessary
                    |---------^ // I want only this
```

To fix that, replace the antecedent with a short sequence of seeing 0 then 1:

```systemverilog
(cnt == 0) ##1 (cnt == 1) |=> (cnt != 0)[*0:$] ##1 (cnt == 2) ##1 (cnt == 0)[->1];
// OR:     $rose(cnt > 0) |=> ...
```

Now it works great!

### [->n] vs [*0:$]

`b[->n]` is equivalent to `(!b[*0:$] ##1 b)[*n]`, which works like syntactic sugar.

### [->n] vs first_match()

`b[->n]` only accepts `b` as a single boolean value without logical operators. So in these cases use `first_match()` instead.

```systemverilog
(AWREADY & AWVALID)[->1]        // bit operator: OK
first_match(AWREADY && AWVALID) // logical operator: use first_match()
```

### When NOT to use

NEVER use it in the 1st term of antecedent. Because it will start tons of unnecessary threads.

```systemverilog
a[->1] ##1 b |=> c; // BAD and not necessary
```

For more detailed use guides please refer to [Ben's article](https://systemverilog.us/vf/goto_conseq.pdf) .