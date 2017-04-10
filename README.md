通过命令启动
> docker-compose build
>
> docker-compose up -d


配置文件：根据具体项目所在的位置调整卷

```

version: '2'
services:
    web:
        image: daocloud.io/library/nginx:1.9.9
        ports:
            - "80:80"
        links:
            - "phpfpm"
        volumes:
            - "/data/php:/data"
            - "/data/docker/lamp/nginx:/etc/nginx/conf.d/"
    phpfpm:
        build: ./php
        ports:
            - "9000:9000"
        volumes:
            - "/data/php:/data"
    
```

>nginx配置文件在nginx目录下，新增配置文件即可新增站点，重启容器生效。
