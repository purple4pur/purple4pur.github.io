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

### docker

[[Official installing guide]](https://docs.docker.com/engine/install/debian/#install-using-the-repository)

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