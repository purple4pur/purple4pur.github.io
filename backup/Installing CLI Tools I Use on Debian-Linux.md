### Renew in 2026

1. Install `bin` from <https://github.com/marcosnils/bin/releases/latest>

```bash
# Download bootstrap
cd ~ && mkdir bin
curl -LO <asset_url>
chmod +x <executable>

# Install & manage itself
./<executable> install https://github.com/marcosnils/bin

# Cleanup
rm <executable>

# Prepand ~/bin to $PATH
# DIY
```

2. Install binaries

```bash
bin i <url>

# Tools installed as root as an example
Path                            Version   URL                                                        Status
/usr/local/bin/bin              v0.26.0   https://github.com/marcosnils/bin/releases/tag/v0.26.0     OK
/usr/local/bin/btop             v1.4.7    https://github.com/aristocratos/btop                       OK
/usr/local/bin/chezmoi          v2.70.3   https://github.com/twpayne/chezmoi/releases/tag/v2.70.3    OK
/usr/local/bin/dust             v1.2.4    https://github.com/bootandy/dust                           OK
/usr/local/bin/fd               v10.4.2   https://github.com/sharkdp/fd/releases/tag/v10.4.2         OK
/usr/local/bin/frpc             v0.68.1   https://github.com/fatedier/frp/releases/tag/v0.68.1       OK
/usr/local/bin/goecs            v0.1.129  https://github.com/oneclickvirt/ecs/releases/tag/v0.1.129  OK
/usr/local/bin/lazygit          v0.61.1   https://github.com/jesseduffield/lazygit                   OK
/usr/local/bin/mihomo-v3-go123  v1.19.25  https://github.com/MetaCubeX/mihomo/releases/tag/v1.19.25  OK
/usr/local/bin/moor             v2.13.2   https://github.com/walles/moor/releases/tag/v2.13.2        OK
/usr/local/bin/nvim.appimage    v0.12.2   https://github.com/neovim/neovim/releases/tag/v0.12.2      OK
/usr/local/bin/rg               15.1.0    https://github.com/BurntSushi/ripgrep/releases/tag/15.1.0  OK
/usr/local/bin/riff             3.6.1     github.com/walles/riff                                     OK
/usr/local/bin/starship         v1.25.1   https://github.com/starship/starship/releases/tag/v1.25.1  OK
/usr/local/bin/uv               0.11.14   https://github.com/astral-sh/uv                            OK

# Make binaries executable for everyone (when installed as root)
bin | rg '^/' | cut -d' ' -f1 | xargs chmod +x

# Set GitHub PAT to resolve access limits
export GITHUB_AUTH_TOKEN=github_pat_xyz
```

---

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
rm $HOME/opt/nvim-linux64 -rf # remove old version
tar -C $HOME/opt -xzf nvim-linux64.tar.gz # install to $HOME/opt/nvim-linux64/bin
rm nvim-linux64.tar.gz
```

### fd

[[Github]](https://github.com/sharkdp/fd)

```sh
(root) apt update
(root) apt install fd-find
(root) ln -sf /usr/bin/fdfind /usr/bin/fd
```

### rg

[[Github]](https://github.com/BurntSushi/ripgrep)

```sh
(root) apt update
(root) apt install ripgrep
```

### docker

[[Official installation guide]](https://docs.docker.com/engine/install/debian/#install-using-the-repository)

```sh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Hello world
sudo docker run hello-world

# Add user to docker group to grant him privileges
# WARNING: Do with caution! docker group has the same permissions of root!
sudo usermod -aG docker USERNAME
```