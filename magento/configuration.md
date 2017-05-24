## Magento modes
- Default mode
- Developer mode
- Production mode
  - static view files are populated in the pub/static directory
  - code compilation
- Display the current mode  
  `magento deploy:mode:show`

## Set the Magento mode
- Developer and production modes are set in **env.php**
- Command usage:  
`magento deploy:mode:set {mode} [-s|--skip-compilation]`
- Change to production mode  
`magento deploy:mode:set production`
- Change to developer mode  
`rm -rf <your Magento install dir>/var/di/* <your Magento install dir>/var/generation/*`  
`magento deploy:mode:set developer`
- Contents of following directories are cleared when change mode:
```
var/cache
var/di
var/generation
var/view_preprocessed
pub/static
```
- By default, Magento uses the `var` directories to store the cache, logs, and compiled code.

**Exceptions**  
  - .htaccess files are not removed
  - pub/static contains a file that specifies the version of static content; this file is not removed

## Deploy static view files
- Static view files are located in the `<your Magento install dir>/pub/static` directory, and some are cached in the <your Magento install dir>/var/view_preprocessed directory
- Log in to the Magento server  
  `su <Magento file system owner> -s /bin/bash -c <command>`, or  
  `sudo -u <Magento file system owner>  <command>`
- Add `<your Magento install dir>/bin` to your system PATH  
  `export PATH=$PATH:/var/www/html/magento2/bin`
- Run Magento commands
  * cd `<your Magento install dir>/bin` and run them as `./magento <command name>`
  * `php <your Magento install dir>/bin/magento <command name>`
- Run the static view files deployment tool `<your Magento install dir>/bin/magento setup:static-content:deploy`.  
  Command options:  
  `magento setup:static-content:deploy <lang> ... <lang> [--dry-run] `
  - **lang**: Space-separated list of ISO-636 language codes. Default is `en_US`   
    Find lang list: `magento info:language:list`
  -

## Clean static files cache
- In the `Magento Admin`. Go to `System > Tools > Cache Management` and click `Flush Static Files Cache`.
- Manually by clearing the `pub/static` and `var/view_preprocessed` directories and subdirectories except for `pub/static/.htaccess`.
- Command line: `rm -R pub/static/*`


## File system ownership and permissions
- In version 2.0.6 and later, Magento does not explicitly set file or directory permissions.
- Owner: a user who owns and can write to files in the Magento file system
- Restrict access with a **umask**
  - Magento uses a three-bit mask, by default `002`, that you subtract from the UNIX defaults of `666` for files and `777` for directories.
  - `775` for directories, which means full control by the user, full control by the group, and enables everyone to traverse the directory.
  - `664` for files, which means writable by the user, writable by the group, and read-only for everyone else.
- Optionally set `magento_umask`  
  - Enable you to optionally create a file named `magento_umask` in your Magento root directory that restricts permissions for the web server group and everyone else.
  - A common suggestion is to use a value of `022` in the magento_umask file, which means:
    * `755` for directories: full control for the user, and everyone else can traverse directories.
    * `644` for files: read-write permissions for the user, and read-only for everyone else.


## Display the Admin URI
`bin/magento info:adminuri`
