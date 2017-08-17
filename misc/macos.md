
## Environment Variables
```
env | grep DOCKER

//Unset env variable
unset DOCKER_TLS_VERIFY
```

## Keyboard Shortcuts
(Make `Option` key act like `Meta` in Terminal preferences)  
`alt ⌥+F` to jump Forward by a word  
`alt ⌥+B` to jump Backward by a word  
`alt ⌥+D` to delete a word starting from the current cursor position  
`ctrl+A` to jump to start of the line  
`ctrl+E` to jump to end of the line  
`ctrl+K` to kill the line starting from the cursor position  
`ctrl+Y` to paste text from the kill buffer  
`ctrl+R` to reverse search for commands you typed in the past from your history  
`ctrl+S` to forward search (works in zsh for me but not bash)  
`ctrl+F` to move forward by a char  
`ctrl+B` to move backward by a char  
`ctrl+W` to remove the word backwards from cursor position  

## Commands
### Network
- brctl  
ethernet bridge administration  
`brctl <command> [command-options and arguments]`

- traceroute  
route-tracing utility used to determine the path that an IP packet has taken to reach a destination


### Install Packages
- brew
```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"  
brew install node  
brew update && brew upgrade node  
brew switch node 0.10.26  
```

- Open an app  
`open -a Docker`

- Quit an app  
`osascript -e 'quit app "Docker"'`
