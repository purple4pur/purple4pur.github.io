## What Is Sparse Checkout?

Checkout only part of the entire repository. Focus on files you need.

Keep in mind: sparse-checkout settings are only kept in local copy. Won't be pushed. Won't be cloned.

## Enable/Set/Add

```bash
git sparse-checkout set/add [--no-cone] <file(s)>
```

* `set` : re-write current settings
* `add` : append to current settings

Running this will automatically enable sparse-checkout feature, which can be checked with `git config --list` .

Internally, sparse-checkout maintains two files:

1. `.git/config` , where stores the feature ON/OFF flags
2. `.git/info/sparse-checkout` . This file has a format the same as `.gitignore` but plays a **whitelist** role , and is the one `set/add` write to.

Alternatively, the user can edit `.git/info/sparse-checkout` manually. (See also: <ins>`--cone` (default flag) vs `--no-cone`</ins>)

## List Current Settings

```bash
git sparse-checkout list
```

## Disable/Re-enable

### Disable

```bash
git sparse-checkout disable
```

: turn off all sparse-checkout flags in `.git/config` , but not touch `.git/info/sparse-checkout` .

### Re-enable

```bash
git sparse-checkout add # no files here
```

: turn on flags, continue using current `.git/info/sparse-checkout` .

## `--cone` (default flag) vs `--no-cone`

`--cone` is more like a "smart" mode:

* It automatically add `/*` and `!/*/` to `.git/info/sparse-checkout` (which means it **always allows top-level files** such as `/README.md` . It is impossible to filter them out.)
* When `set/add` , for example, passing `lua` will become a `/lua/` line in `.git/info/sparse-checkout`

`--no-cone` is somewhat "manual" mode:

* When `set/add` , `<file(s)>` will be written into `.git/info/sparse-checkout` character by character.

In this mode, the user can filter anything without any restriction. But to remember: always set a path with a leading `/` like this: `git sparse-checkout add /README.md /lua /doc` .

If you have been aware of all of above, I suggest `--no-cone` mode, which is more flexible. [An official discussion here](https://git-scm.com/docs/git-sparse-checkout#_internalsnon_cone_problems) .

## Extra: Start From a Minimal Clone

```bash
git clone --filter=blob:none --no-checkout <URL> [<...>]
git sparse-checkout set [<...>]
git checkout
```

[Reference](https://github.blog/2020-01-17-bring-your-monorepo-down-to-size-with-sparse-checkout/#sparse-checkout-and-partial-clones)