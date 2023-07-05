# nvm

[GitHub - nvm-sh/nvm: Node Version Manager - POSIX-compliant bash script to manage multiple active node.js versions](https://github.com/nvm-sh/nvm)

```
# install
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

# verify install
# outputs "nvm"
command -v nvm


# list available versions
nvm ls-remote

# list installed versions
nvm ls


# install latest version
nvm install node

# install latest lts
nvm install --lts

# install lts hydrogen
nvm install lts/hydrogen

# install node 16
nvm install 16


# unistall latest version
nvm uninstall node

# uninstall latest lts
nvm uninstall --lts

# unistall lts/hydrogen
nvm uninstall lts/hydrogen

# uninstall node 16
nvm uninstall 16


# use system version (installed with apt)
nvm use system

# use latest version
nvm use node

# use latest lts
nvm use --lts

# use lts hydrogen
nvm use lts/hydrogen

# use node 16
nvm use 16


# set project default to latest version
echo "node" > .nvmrc

# set project default to latest lts
echo "lts/*" > .nvmrc

# set project default to lts hydrogen
echo "lts/hydrogen" > .nvmrc

# set project default to node 16
echo "16" > .nvmrc


# set default version to latest version
nvm alias default node

# set default version to latest lts
nvm alias default "lts/*"

# set default version to lts hydrogen
nvm alias default lts/hydrogen

# set default version to node 16
nvm alias default 16
```


