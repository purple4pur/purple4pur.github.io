## File Path

* Linux: `~/.gitconfig`
* Windows: `C:/Users/<user>/.gitconfig`

## Example

```
[user]
    email = xxx@yyy.zzz
    name = xxx
[http]
    proxy = 127.0.0.1:10809
[core]
    editor = nvim
    pager = moar
[color]
    ui = auto
[diff]
    tool = meld
```

## CLI Command

```sh
git config --global --list
           --local  --get
                    --set   user.email "xxx@yyy.zzz"
                    --unset
```