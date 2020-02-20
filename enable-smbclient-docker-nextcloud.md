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
