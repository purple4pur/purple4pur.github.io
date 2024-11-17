### git

```sh
(root) apt update
(root) apt install git
```

### chezmoi

[[Official installation guide]](https://www.chezmoi.io/install/#one-line-binary-install)

```sh
sh -c "$(curl -fsLS get.chezmoi.io)" # install to $HOME/bin
```

### starship

[[Official installation guide]](https://starship.rs/guide/#step-1-install-starship)

```sh
(root) curl -sS https://starship.rs/install.sh | sh # install to /usr/local/bin
```

### neovim

[[Official installation guide]](https://github.com/neovim/neovim/blob/master/INSTALL.md#pre-built-archives-1)

```sh
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz
mkdir $HOME/opt
rm $HOME/opt/nvim-linux64 -rf
tar -C $HOME/opt -xzf nvim-linux64.tar.gz # install to $HOME/opt/nvim-linux64/bin
rm nvim-linux64.tar.gz
```