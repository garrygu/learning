## Apache
- Install Apache  
`yum -y install httpd`  
`httpd -v`

- Config file  
`/etc/httpd/conf/httpd.conf`

- List the modules enabled  
`apachectl -M | sort`  
`httpd -M`

- Finding out what user Apache is running as  
`ps aux | egrep '(apache|httpd)'`  
`ps -ef | egrep '(httpd|apache2|apache)' | grep -v `whoami` | grep -v root | head -n1 | awk '{print $1}'`  
`ps aux | grep -v root | grep apache | cut -d\  -f1 | sort | uniq`  

- Find user groups  
`groups apache`  
`ps axo user,group,comm | grep apache`  


## Find the docroot for Apache 2.4
- Open: `/etc/apache2/sites-available/default/000-default.conf`
- Search for `DocumentRoot`, such as: `DocumentRoot /var/www/html`


## Security
### Maintained by a single user [[source]](http://serverfault.com/questions/357108/what-permissions-should-my-website-files-folders-have-on-a-linux-webserver)


If only one user is responsible for maintaining the site, set them as the user owner on the website directory and give the user full rwx permissions. Apache still needs access so that it can serve the files, so set `apache` as the group owner and give the group r-x permissions.

```
chown -R eve contoso.com  
chgrp -R apache contoso.com  
chmod -R 750 contoso.com  
chmod g+s contoso.com  
```

For folders writable by apache:  
`chmod g+w uploads`


## Bad Ideas
- chmod 777  
This allows anyone with access to your system write into the directories and files and execute any code under the `apache` user

- chgrp -R apache $HOME  
This allows `apache` to read or write any files in the home directory.

- chown -R $USER:$USER /var/www/html
Unless the world has read permissions on `/var/www/html`, the webserver running under `apache` will not be able to read (serve) the files.
