# 執行容器

## 執行指定容器

```shell
$ docker run -it kejyun/ubuntu1804docker:0.2 /bin/bash
```

## 顯示容器 Log


```shell
$ docker container logs <CONTAINER ID / CONTAINER NAMES>
```

指令後方可以輸入 `CONTAINER ID` 或 `CONTAINER NAMES` 去取得容器 Log

**取得 Container ID**

```shell
$ docker ps -a
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                           PORTS               NAMES
a7d3fc447e45        kejyun/ubuntu1804docker:0.2   "/bin/bash"              41 minutes ago      Up 41 minutes                    80/tcp              gracious_hugle
```

```shell
$ docker container logs a7d3fc447e45

root@a7d3fc447e45:/# cd /home/web/
root@a7d3fc447e45:/home/web# ls
img
```
