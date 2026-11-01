## Clone a Repository With Its Submodules

If is a first-time clone:

```bash
git clone --recursive <...>
```

If is an existing repository:

```bash
git submodule update --init
```

## Add a Submodule

```bash
git submodule add <repo_url> [<path>]
```

Running this command will:

* clone `<repo_url>` into `<path>` , but separate its `.git` folder (see <ins>Change Settings of Submodules</ins>)
* add submodule infomation into `.gitmodules`
* register submodule in `.git/modules/path/to/submodule` (where the `.git` folder is actually in)
* register submodule in `.git/config` (do a `git submodule init` )
* stage `.gitmodules` and the submodule folder

This clone will be a very full clone, which means not so many configs can play a role.

## Pull Upstream Changes

Two ways:

* `git submodule update --remote [<path>]`
* OR: `git pull` in the submodule

Pulling upstream will also update the recorded hashes. Don't forget to commit changes.

## Change Settings of Submodules

There're two paths available:

* `/path/to/submodule` (recommended)
* `.git/modules/path/to/submodule` (the actual path of the `.git` folder)

Change working directory into one of them, then any git commands will apply to the submodule. For the first path, if is not been cloned, commands would still apply to the top-level repository. It is recommended to open a new terminal tab/window for changes within a submodule.

What can do in a submodule:

* pull upstream changes
* setup [sparse checkout](https://purple4pur.github.io/post/Git%20Sparse%20Checkout.html) (not that practical)

What can NOT do (or not be recommended) in a submodule:

* change branch (see <ins>Change Tracking Branch</ins>)
* change remote url (see <ins>Change Submodule URL</ins>)

## Change Tracking Branch

A submodule can only be tracked with a **remote named branch**, not a hash, not a local branch.

### 1. Update Branch Setting

* `git submodule set-branch -b <remote-branch> <module>` (not recommended - verbose and easy to mismatch)
* OR: manually append `branch = <remote-branch>` below submodule's url in `.gitmodules`

Warning: `<module>` must match the name in `.gitmodules` .

If want to remove this setting (means to use the default branch):

* `git submodule set-branch -d <module>` (not recommended)
* OR: manually remove the `branch = <remote-branch>` line

### 2. Switch to the New Branch

```bash
git submodule update --remote [<path>]
```

This command will:

1. do a `git pull; git checkout origin/<remote-branch>` in submodules
2. update recorded hash (checked with `status`. See <ins>`git submodule status`</ins>)

Don't forget to commit changes.

## Change Submodule URL

Mostly the same as <ins>Change Tracking Branch</ins> , but change `url = <URL>` line instead.

## Move a Submodule to a New Path

```bash
git mv /path/to/submodule /new/path
```

`git mv` handles everything.

## Remove a Submodule From Current Repository

1. `git submodule deinit /path/to/submodule` : unregister submodule from `.git/config`
2. remove `.git/modules/path/to/submodule` manually
3. `git rm /path/to/submodule` : remove submodule folder, unregister from `.gitmodules` and stage changes

[Reference](https://stackoverflow.com/a/16162000/12509229)

## Submodule Command Explanation

[Full Documentation](https://git-scm.com/docs/git-submodule) | [Example Usage](https://git-scm.com/book/en/v2/Git-Tools-Submodules)

### `git submodule -h`

List all sub-commands.

### `git submodule`

Same as `git submodule status` without parameters.

### `git submodule status`

List all submodules with paths and **current hashes**. Hashes may have a prefix:

* `-` : not initialized. An `init` or `update --init` is wanted
* `+` : currently checked out hash differs from the **recorded one**. An `update` is wanted

### `git submodule add`

Add a new submodule. See <ins>Add a Submodule</ins> .

### `git submodule init/deinit`

(Un)register a submodule. "Registered" means "active". `update` and other sub-commands will only apply to active submodules.

### `git submodule update`

Checkout the recorded hash of submodules. Some options:

* `--init` : shorthand of `init` then `update`
* `--remote` : pull upstream and update recorded hashes. See <ins>Pull Upstream Changes</ins>
* `--recursive` : `update` all nested submodules

### `git submodule foreach <command>`

Run `<command>` within each submodule. Example: `git submodule foreach git pull` .
