## Online Playground
https://angular.io/resources/live-examples/quickstart/ts/eplnkr.html

## Local Development
https://angular.io/docs/ts/latest/guide/setup.html

- Clone QuickStart seed
```
git clone https://github.com/angular/quickstart.git quickstart
cd quickstart
npm install
npm start
```

- Delete non-essential files (optional)  
```
# OS/X
xargs rm -rf < non-essential-files.osx.txt
rm src/app/*.spec*.ts
rm non-essential-files.osx.txt
#
# Windows
for /f %i in (non-essential-files.txt) do del %i /F /S /Q
rd .git /s /q
rd e2e /s /q
```
