## Text Editors
- [Brackets](http://brackets.io/)
> A modern, open source text editor that understands web design.
- [Atom](https://atom.io/)
> Atom is a text editor that's modern, approachable, yet hackable to the core—a tool you can customize to do anything but also use productively without ever touching a config file.
- [Sublime Text](https://www.sublimetext.com/)
> Sublime Text is a sophisticated text editor for code, markup and prose.   
You'll love the slick user interface, extraordinary features and amazing performance.

  - [A Collection of Sublime Text 3 Plugins for Frontend Devs](https://github.com/jfilter/Sublime-Text-Plugins-for-Frontend-Web-Development)


- [Notepad++](https://notepad-plus-plus.org/)
> Notepad++ is a free (as in "free speech" and also as in "free beer") source code editor and Notepad replacement that supports several languages. Running in the MS Windows environment, its use is governed by GPL License.


## Online Web Dev Tools
- http://plnkr.co/
- https://jsfiddle.net/
- [Online Javascript Editor](https://js.do/)
- [CodePad](https://codepad.remoteinterview.io/)
- [JS Bin](https://jsbin.com)
- [StackBlitz](https://stackblitz.com) Online VS Code IDE for Angular & React.


## Atom
- Config Settings
```
apm config list
apm config set proxy "http://domain\user:pass@host:port/"
apm config set https_proxy "http://domain\user:pass@host:port/"
```
file (windows): ~\.atom\.apmrc
```
http-proxy="http://localhost:3128"
https-proxy="http://localhost:3128"
strict-ssl=false
```

- Setup  
  * [The Ultimate Atom Editor Setup (+for JS/React)](https://medium.com/productivity-freak/my-atom-editor-setup-for-js-react-9726cd69ad20)


## PowerShell
- Set environment variables  
http://www.dowdandassociates.com/blog/content/howto-set-an-environment-variable-in-windows-command-line-and-registry/
```
$env:Path  
$env:Path += ";C:\Program Files\GnuWin32\bin"  
#List all environment variables
Get-ChildItem Env:  
#Set environment variables  
setx EC2_CERT "%USERPROFILE%\aws\cert.pem"
```


## Deployment
- https://www.softaculous.com
