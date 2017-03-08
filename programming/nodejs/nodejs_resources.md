## Install
### Homebrew (Mac)


### N
- Install N  
`npm install -g n`

- Install nodejs versions 
```
n 0.10.26
n 0.8.17
n 0.9.6
n latest
n stable
# switch to previous version you were using
n prev
# see list of installed nodes
n
```


### NVM
- Install nvm
```
curl https://raw.github.com/creationix/nvm/master/install.sh | sh  
wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```

- Install node
```
nvm install 0.10
nvm uer 0.10
nvm use (create an .nvmrc file containing version number in the project root folder)
nvm run 0.10
nvm ls
```