http://devdocs.magento.com/guides/v2.0/install-gde/install/cli/install-cli-install.html

- Get started with the command-line installation
http://devdocs.magento.com/guides/v2.0/install-gde/install/cli/install-cli-subcommands.html

  - [Localhost instllation](http://devdocs.magento.com/guides/v2.1/install-gde/install/cli/install-cli-install.html#install-cli-example)
```
magento setup:install --base-url=http://127.0.0.1/magento/ \
--db-host=localhost --db-name=magento --db-user=magento --db-password=magento \
--admin-firstname=Magento --admin-lastname=User --admin-email=user@example.com \
--admin-user=admin --admin-password=admin123 --language=en_US \
--currency=USD --timezone=America/Los_Angeles --use-rewrites=1
```

## Get the metapackage
```
cd <web server docroot directory>
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition magento2  (need public/private keys)
```


## Set permissions for shared hosting (one user)
```
cd <your Magento install dir>
find var vendor pub/static pub/media app/etc -type f -exec chmod u+w {} \;
find var vendor pub/static pub/media app/etc -type d -exec chmod u+w {} \;

chmod u+x bin/magento
```

**Note**:  
Need to disable SELinux or set SELinux to permissive mode (setenforce 0)  
http://devdocs.magento.com/guides/v2.0/install-gde/prereq/security.html

## Clear cache
`rm -rf var/cache/* var/generation/*`ls

## Uninstall Magento
```
bin/magento setup:Uninstall
or
php bin/magento setup:uninstall
```


## Misc
- Softaculous: 1-Click Application Installation   
- http://www.inmotionhosting.com/
