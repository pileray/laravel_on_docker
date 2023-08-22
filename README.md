# laravel_on_docker
## 要件
- dockerを用いてLaravelの開発環境を構築する
- app,db,redisコンテナごとにDockerfileを作成し、docker-composeで起動させる
- nginxとphp-fpmはsupervisorで起動する
- .envでの環境変数ファイルを作成し、dbなどの情報を管理すること

## テスト証跡
### ビルド結果
```
fukuiryousukenoMacBook-Pro:dev fukuiryousuke$ docker-compose up -d
Creating network "dev_default" with the default driver
Building db
[+] Building 5.1s (14/14) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                                                   0.1s
 => => transferring dockerfile: 37B                                                                                                                                                                    0.0s
 => [internal] load .dockerignore                                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                                        0.0s
 => [internal] load metadata for docker.io/library/mysql:8.0.27                                                                                                                                        4.9s
 => [auth] library/mysql:pull token for registry-1.docker.io                                                                                                                                           0.0s
 => [1/8] FROM docker.io/library/mysql:8.0.27@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709                                                                                  0.0s
 => [internal] load build context                                                                                                                                                                      0.0s
 => => transferring context: 28B                                                                                                                                                                       0.0s
 => CACHED [2/8] RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 467B942D3A79BD29                                                                                                         0.0s
 => CACHED [3/8] RUN apt-get update                                                                                                                                                                    0.0s
 => CACHED [4/8] RUN apt-get install -y --no-install-recommends    locales     python     vim     && apt-get clean     && rm -rf /var/cache/apt/archives/*     /var/lib/apt/lists/*                    0.0s
 => CACHED [5/8] RUN echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen &&     locale-gen ja_JP.UTF-8                                                                                                          0.0s
 => CACHED [6/8] RUN touch /var/log/mysqld.log     && chown mysql:adm /var/log/mysqld.log                                                                                                              0.0s
 => CACHED [7/8] RUN mkdir /var/mysql     && chown mysql:adm /var/mysql     && rm -rf /etc/mysql/conf.d                                                                                                0.0s
 => CACHED [8/8] COPY ./my.cnf /etc/mysql/                                                                                                                                                             0.0s
 => exporting to image                                                                                                                                                                                 0.1s
 => => exporting layers                                                                                                                                                                                0.0s
 => => writing image sha256:416ebbbb2aacff6be03cc936dcf93d338ed748e1da088c2e5d59a2844e8eb6ac                                                                                                           0.0s
 => => naming to docker.io/library/laravel-db                                                                                                                                                          0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
WARNING: Image for service db was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Pulling redis (redis:latest)...
latest: Pulling from library/redis
1fe172e4850f: Pull complete
6fbcd347bf99: Pull complete
993114c67627: Pull complete
90ee703e9ece: Pull complete
80d8b53e83a7: Pull complete
a09780893e15: Pull complete
Digest: sha256:96c3e4dfe047ba9225a7d36fc92b5a5cff9e047daf41a1e0122e2bd8174c839e
Status: Downloaded newer image for redis:latest
Building app
[+] Building 0.3s (26/26) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                                                   0.0s
 => => transferring dockerfile: 37B                                                                                                                                                                    0.0s
 => [internal] load .dockerignore                                                                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                                                                        0.0s
 => [internal] load metadata for docker.io/library/php:7.4-fpm-alpine                                                                                                                                  0.0s
 => [ 1/21] FROM docker.io/library/php:7.4-fpm-alpine                                                                                                                                                  0.0s
 => [internal] load build context                                                                                                                                                                      0.0s
 => => transferring context: 354B                                                                                                                                                                      0.0s
 => CACHED [ 2/21] WORKDIR /var/www/app                                                                                                                                                                0.0s
 => CACHED [ 3/21] RUN apk --update add tzdata &&     cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime &&     apk del tzdata &&     rm -rf /var/cache/apk/*                                            0.0s
 => CACHED [ 4/21] RUN apk update &&     apk upgrade &&     apk add --update --no-cache     autoconf     bash     build-base     curl-dev     freetype-dev     g++     gcc     git     libjpeg-turbo-  0.0s
 => CACHED [ 5/21] RUN docker-php-ext-install pdo_mysql soap                                                                                                                                           0.0s
 => CACHED [ 6/21] RUN docker-php-ext-configure gd     --with-freetype=/usr/include/     --with-jpeg=/usr/include/ &&     NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) &&     docker-ph  0.0s
 => CACHED [ 7/21] RUN pip install awscli                                                                                                                                                              0.0s
 => CACHED [ 8/21] RUN rm -f /usr/local/etc/php-fpm.conf.default                                                                                                                                       0.0s
 => CACHED [ 9/21] RUN rm -f /usr/local/etc/php-fpm.d/zz-docker.conf                                                                                                                                   0.0s
 => CACHED [10/21] COPY php/php-fpm.conf /usr/local/etc/php-fpm.conf                                                                                                                                   0.0s
 => CACHED [11/21] COPY php/php.ini /usr/local/etc/php/php.ini                                                                                                                                         0.0s
 => CACHED [12/21] COPY php/www.conf /usr/local/etc/php-fpm.d/www.conf                                                                                                                                 0.0s
 => CACHED [13/21] RUN pecl install xdebug-2.9.2 redis                                                                                                                                                 0.0s
 => CACHED [14/21] RUN docker-php-ext-enable xdebug                                                                                                                                                    0.0s
 => CACHED [15/21] RUN curl -sS https://getcomposer.org/installer | php -- --version=2.2.6 --install-dir=/usr/bin                                                                                      0.0s
 => CACHED [16/21] RUN mv /usr/bin/composer.phar /usr/bin/composer                                                                                                                                     0.0s
 => CACHED [17/21] ADD nginx/nginx.conf /etc/nginx/nginx.conf                                                                                                                                          0.0s
 => CACHED [18/21] ADD nginx/dev-laravel.menta.work.conf /etc/nginx/conf.d/dev-laravel.menta.work.conf                                                                                                 0.0s
 => CACHED [19/21] COPY supervisor/supervisord.conf /etc/supervisord.conf                                                                                                                              0.0s
 => CACHED [20/21] COPY supervisor/app.conf /etc/supervisor/conf.d/app.conf                                                                                                                            0.0s
 => CACHED [21/21] RUN echo files = /etc/supervisor/conf.d/*.conf >> /etc/supervisord.conf                                                                                                             0.0s
 => exporting to image                                                                                                                                                                                 0.1s
 => => exporting layers                                                                                                                                                                                0.0s
 => => writing image sha256:42eee971c3043f3fc6d0226cd5bc68d340dfd70785b3f7c6c677092f4f27d120                                                                                                           0.0s
 => => naming to docker.io/library/laravel-app                                                                                                                                                         0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
WARNING: Image for service app was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating laravel-redis ... done
Creating laravel-db    ... done
Creating laravel-app   ... done
```

### dev-laravel.menta.workでのアクセス
```
fukuiryousukenoMacBook-Pro:dev fukuiryousuke$ ping dev-laravel.menta.work
PING dev-laravel.menta.work (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.046 ms
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.104 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.168 ms
64 bytes from 127.0.0.1: icmp_seq=3 ttl=64 time=0.128 ms
^C
--- dev-laravel.menta.work ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 0.046/0.112/0.168/0.044 ms
```
![image](https://user-images.githubusercontent.com/60649665/165877028-2433d158-85fb-40fe-9e8f-167339c05618.png)

