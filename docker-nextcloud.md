## install docker based nextcloud in OMV
```
docker pull nextcloud
docker run -d --name nextcloud \                                            
                -v /sharedfolders/nextcloud/html:/var/www/html \
                -v /sharedfolders/nextcloud/db:/var/lib/mysql \
                -v /sharedfolders/nextcloud/data:/var/www/html/data \
                -p 8088:80 \
                nextcloud
```

### enable smbclient
into the shell of container

```
docker exec -it 容器ID /bin/bash 
apt install libsmbclient libsmbclient-dev
pecl install smbclient
```

add a extension for php.ini

```
cat > /usr/local/etc/php/conf.d/docker-php-ext-smbclient.ini
extension=smbclient.so
```
