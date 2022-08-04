# My Dot files:

## Automate this
```sh
### Instalando Git e coisas básicas

###Instalando o Chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco feature enable -n=allowGlobalConfirmation

### Setup dos dotfiles
git clone https://github.com/orsanor/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
ln -s ~/.dotfiles/.gitconfig ~/.gitconfig

### Finalização
cd ~ && mkdir ./dev


