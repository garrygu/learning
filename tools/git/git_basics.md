## Environment
```
# configure user name and email address
git config --global user.name "Your Name"
git config --global user.email you@example.com

# HTTP proxy  
git config --global http.proxy http://proxy.mycompany:80
```


## Commands
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
