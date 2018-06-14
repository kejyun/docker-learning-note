# 列出映像檔

## 列出所有映像檔

可以使用 `docker images` 或 `docker image ls` 列出所有映像檔

```shell
$ docker images
$ docker image ls
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
| 說明 | 來自哪個資源庫 | 標記  | 唯一編號  | 建立時間  | 檔案大小  |
| example 1 | laradock_nginx |  latest |  2586012f8dad | 24 hours ago  |  26.7MB |
| example 2 | ubuntu |  16.04 | 5e8b97a2a082 | 7 days ago  | 114MB |
| example 3 | ubuntu |  18.04 | 113a43faa138 | 7 days ago  | 81.2MB |
| example 4 | ubuntu |  latest| 113a43faa138 | 7 days ago  | 81.2MB |

IMAGE ID 是唯一編號，在目前的 latest 版本與 18.04 版本是同一個映像檔，所以 IMAGE ID 則為相同


**列出所有虛擬映像檔**

```shell
$ docker image ls -f dangling=true

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              bb74cce9ef29        3 hours ago         145MB
<none>              <none>              3da67fb2dba4        22 hours ago        137MB
```

虛擬映像檔指的是他不屬於任何一個資源庫（Repository）也沒有指定任何標記（Tag）的映像檔


**移除所有虛擬映像檔**

可以使用 `docker image prune` 移除所有的虛擬映像檔

```shell
$ docker image prune
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
Deleted Images:
deleted: sha256:bb74cce9ef29d7747ba6f168510ee77aa8b039ff67dc8b8efbbf91bc55dc7d9e
deleted: sha256:bbed30714b50963e8a4e86ae94755d6f245ca43fd07c34f03b810137d73f11c3
deleted: sha256:3da67fb2dba41a3352a827e562a57cba9dfccf4b9759f6b4364161bfee0b7a5a

Total reclaimed space: 1.179MB
```

**中間映像檔**

可以使用 `docker image ls -a` 或 `docker images -a` 列出所有中間映像檔，會了加速建立映像檔，Docker 會利用中間映像檔去建立新的映像檔。

這些無標籤的映像檔皆為中間映像檔，中間映像檔是其他映像檔依賴的鏡像映像檔，若中間映像檔被刪除了，則會導致依賴出錯。

中間映像檔不需要刪除，相同的 container 只會被儲存一次，所以不會佔太多空間，若原本依賴他的映像檔被刪除後，浙西中間映像檔也會被刪除。

```shell
$ docker image ls -a
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
kejyun/ubuntu1804         0.1                 a54db3844855        21 hours ago        144MB
kejyun/ubuntu1804docker   0.1                 a54db3844855        21 hours ago        144MB
<none>                    <none>              4d45c8de0b0e        21 hours ago        142MB
<none>                    <none>              4d1c46269cae        21 hours ago        142MB
<none>                    <none>              36a48deac67b        21 hours ago        142MB
<none>                    <none>              0f58b888565e        21 hours ago        142MB
<none>                    <none>              39247bc7e5f9        21 hours ago        140MB
<none>                    <none>              c1f733062d8b        21 hours ago        140MB
<none>                    <none>              c82e7b150562        21 hours ago        140MB
kejyun/ubuntu             0.1                 61853cc80834        21 hours ago        140MB
<none>                    <none>              039ac36a425a        21 hours ago        138MB
<none>                    <none>              db6ccb8f76ae        21 hours ago        138MB
<none>                    <none>              a9b3724e2ee3        22 hours ago        137MB
```

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

**其他語法**

| 參數  | 說明  |
|---|---|
| --expose  | 開放內部的 port 到外部 |
| --rm  | 退出終端介面後，立即刪除 container，若只是測試可以用這個指令，避免浪費空間，但如果需要故障排除，保留資料的話，不可以下這個指令 |



## 列出映像檔

**列出目前啟動的映像檔 container**

> docker ps

**列出所有映像檔 container**

> docker ps -a

```shell
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
1436aace3d48        ubuntu              "/bin/bash"         4 seconds ago       Up 6 seconds                                    mystifying_kare
d4816a250cca        ubuntu              "/bin/bash"         12 seconds ago      Exited (0) 8 seconds ago                        infallible_shirley
f8e6e8b94132        ubuntu:18.04        "/bin/bash"         6 minutes ago       Exited (0) 23 seconds ago                       distracted_kowalevski
```

## 查詢 Ubuntu containers 系統資訊

可以使用 `cat /etc/os-release` 查詢 Ubuntu CONTAINER 版本

```shell
root@f5f0353fc32b:/\# cat /etc/os-release

NAME="Ubuntu"
VERSION="18.04 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

## 參考資料
* [Docker Documentation](https://docs.docker.com/engine/reference/run/)
* [Docker run 命令 | 菜鸟教程](http://www.runoob.com/docker/docker-run-command.html)
* [Docker run 命令的使用方法 - DockOne.io](http://dockone.io/article/152)
* [最重要的Docker Run | 全面易懂的Docker指令大全](https://joshhu.gitbooks.io/dockercommands/content/Containers/DockerRun.html)
* [列出镜像 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/image/list.html)
