## DevBox
http://devdocs.magento.com/guides/v2.1/install-gde/docker/docker-trouble.html

### Connect to web container
- docker-compose exec --user=magento2 <service> /bin/bash
- cd /var/www/magento2/ && php bin/magento setup:di:compile
- php bin/magento setup:static-content:deploy


### Start Over
- docker-compose ps
- docker rm -fv <service>
- ./m2devbox-init.sh  / m2devbox-init.bat
