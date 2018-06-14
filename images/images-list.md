# 列出映像檔

## 列出所有映像檔

```shell
$ sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
laradock_redis       latest              3ec3f541bae0        24 hours ago        107MB
laradock_nginx       latest              2586012f8dad        24 hours ago        26.7MB
laradock_php-fpm     latest              235ab07df83e        24 hours ago        384MB
laradock_workspace   latest              8f74a5ffe174        24 hours ago        721MB
laradock_mysql       latest              61a7a3f27ca2        24 hours ago        445MB
ubuntu               16.04               5e8b97a2a082        7 days ago          114MB
ubuntu               18.04               113a43faa138        7 days ago          81.2MB
ubuntu               latest              113a43faa138        7 days ago          81.2MB
nginx                alpine              bc7fdec94612        7 days ago          18MB
mysql                latest              a8a59477268d        5 weeks ago         445MB
redis                latest              bfcb1f6df2db        6 weeks ago         107MB
laradock/php-fpm     2.0-72              22f190ba7b61        5 months ago        384MB
laradock/workspace   2.0-72              69a57f6aca78        5 months ago        721MB
```

| 標記 |  REPOSITORY |  TAG | IMAGE ID  | CREATED  |  SIZE |
|---|---|---|---|---|---|
| 說明 | 來自哪個倉庫 | 標記  | 唯一編號  | 建立時間  | 檔案大小  |
| example 1 | laradock_nginx |  latest |  2586012f8dad | 24 hours ago  |  26.7MB |
| example 2 | ubuntu |  16.04 | 5e8b97a2a082 | 7 days ago  | 114MB |
| example 3 | ubuntu |  18.04 | 113a43faa138 | 7 days ago  | 81.2MB |
| example 4 | ubuntu |  latest| 113a43faa138 | 7 days ago  | 81.2MB |

IMAGE ID 是唯一編號，在目前的 latest 版本與 18.04 版本是同一個映像檔，所以 IMAGE ID 則為相同

## 啟動映像檔

**啟動 ubuntu 18.04**

```shell
$ sudo docker run -t -i ubuntu:18.04 /bin/bash
root@f8e6e8b94132:/#
```

**啟動最新 ubuntu**

```shell
$ sudo docker run -t -i ubuntu /bin/bash
root@1436aace3d48:/#
```

> 如果沒有指定 TAG，預設使用 latest

### docker run 參數

**語法**

> docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

> docker run -t -i ubuntu:18.04 /bin/bash

| 參數  | 說明  |
|---|---|
| run  |  執行 container |
| -t  | 替 container 分配一個虛擬終端機借，通常與 -t 同時執行 |
| -i  | 命令互動模式，通常與 -t 同時執行  |
| ubuntu:18.04  |  映像檔名稱 |
|  /bin/bash |  在 container 執行 `/bin/bash` 指令 |


## 列出目前啟動的映像檔

```shell
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
1436aace3d48        ubuntu              "/bin/bash"         4 seconds ago       Up 6 seconds                                    mystifying_kare
d4816a250cca        ubuntu              "/bin/bash"         12 seconds ago      Exited (0) 8 seconds ago                        infallible_shirley
f8e6e8b94132        ubuntu:18.04        "/bin/bash"         6 minutes ago       Exited (0) 23 seconds ago                       distracted_kowalevski
```


## 參考資料
* [Docker run 命令 | 菜鸟教程](http://www.runoob.com/docker/docker-run-command.html)
