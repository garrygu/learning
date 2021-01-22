## Environment
```
# List config items
git config --List
git config --list --show-origin

# Configure user name and email address
git config --global user.name "Your Name"
git config --global user.email you@example.com

# HTTP proxy  
git config --global http.proxy http://proxy.mycompany:80
git config --global http.proxy http://mydomain\\myusername:mypassword@myproxyserver:8080/
git config --get http.proxy

# Tell git to not perform the validation of the certificate
git config --global http.sslVerify false

# Setting $HOME and the startup directory
https://danlimerick.wordpress.com/2011/07/11/git-for-windows-tip-setting-home-and-the-startup-directory/
```

## Commands
- https://twitter.com/profulsadangi/status/1298236182245982210/photo/1
![](./files/EgQvnCMVAAAGmoP.jpeg)


### Using Branches
#### git branch
- `git branch`  
List all branches

- `git branch <branch>`  
Create a new branch called <branch> (not check out)

- `git branch -d <branch>`  
Delete the specified branch (but will prevent from deleting branch with unmerged changes)

- `git branch -D <branch>`  
Force delete the specified branch, even if it has unmerged changes

- `git branch -m <branch>`  
Rename the current branch to &lt;branch&gt;


#### git checkout
- `git checkout <existing-branch>`  
Check out the specified branch (This makes &lt;existing-branch&gt; the current branch, and updates the working directory to match.)

- `git checkout -b <new-branch`  
Create and check out &lt;new-branch&gt;
https://neg-payment/PaymentAPI/TransactionRecipts/285576877/Both <https://neg-payment/PaymentAPI/TransactionRecipts/285576877/Both> PaymentAPI/TransactionRecipts/285576877/Both <http://172.16.70.174/PaymentAPI/TransactionRecipts/285576877/Both>


#### git merge
- `git merge <branch>`  
Merge the specified branch into the current branch

- `git merge --no-ff <branch>`  
Merge the specified branch into the current branch, but always generate a merge commit (even if it was a fast-forward merge)


## A clean, usable history that makes sense
https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa
Four main tools:
```
git commit --amend
git merge, with or without --no-ff
git rebase, especially git rebase -i and git rebase -p
git cherry-pick (which is functionally inseparable from rebase)
```
