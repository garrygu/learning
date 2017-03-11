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


### Common DevBox commands
- Populate the storefront and cache and run cron
`docker-compose exec --user=magento2 web m2init magento:finalize --magento-warm-up-storefront=1 --magento-cron-run=1 --no-interaction`

- Start the Magento cron job only
`docker-compose exec --user=magento2 web m2init magento:finalize --magento-cron-run=1 --no-interaction`

- Populate the storefront and cache only
`docker-compose exec --user=magento2 web m2init magento:finalize --magento-warm-up-storefront=1 --no-interaction`

- Configure DevBox to always populate the storefront and cache and run cron
  * Stop all running containers: `docker-compose stop`
  * Open `<project root dir>/docker-compose.yml`
    * To enable cron to run only: `MAGENTO_CRON_RUN=1`
    * To only populate the storefront and cache: `MAGENTO_WARM_UP_STOREFRONT=1`
    * To both enable the Magento cron job and to always populate the storefront and cache:   
      `MAGENTO_CRON_RUN=1`  
      `MAGENTO_WARM_UP_STOREFRONT=1`
  * Start all containers: `docker-compose start`

- Restart the containers after rebooting
`m2devbox-reset[.bat|sh]`

- Find currently used ports
`docker-compose ps`

- Start, stop, restart services (db, web)
```
docker-compose start <service>
docker-compose stop <service>
docker-compose restart <service>
```
