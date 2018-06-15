# 停止容器


## 終止容器

```shell
docker container stop <CONTAINER ID / CONTAINER NAMES>
```

指令後方可以輸入 `CONTAINER ID` 或 `CONTAINER NAMES` 去終止容器


**取得 Container ID**

```shell
$ docker ps -a
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                           PORTS               NAMES
a7d3fc447e45        kejyun/ubuntu1804docker:0.2   "/bin/bash"              41 minutes ago      Up 41 minutes                    80/tcp              gracious_hugle
```

**終止 Container**

```shell
$ docker container stop a7d3fc447e45
```

## 列出所有 container


```shell
docker container ls -a
```


```shell
$ docker container ls -a

CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                           PORTS               NAMES
a7d3fc447e45        kejyun/ubuntu1804docker:0.2   "/bin/bash"              About an hour ago   Exited (0) 2 minutes ago                             gracious_hugle
3807a3a021a2        14e3ffbaff4f                  "/bin/bash"              About an hour ago   Exited (0) About an hour ago                         determined_northcutt
b21b6a4adab7        159fea2b67bb                  "/bin/bash"              About an hour ago   Exited (0) About an hour ago                         upbeat_sammet
aa5cca969bed        a54db3844855                  "/bin/sh -c 'buildPy…"   About an hour ago   Exited (127) About an hour ago                       distracted_mahavira
3c5e5af2cf1e        a54db3844855                  "/bin/sh -c 'buildPy…"   About an hour ago   Exited (127) About an hour ago                       mystifying_villani
```

## 啟動容器

```shell
docker container start <CONTAINER ID / CONTAINER NAMES>
```

指令後方可以輸入 `CONTAINER ID` 或 `CONTAINER NAMES` 去啟動容器

```shell
$ docker container start a7d3fc447e45
```

## 重新啟動容器


```shell
docker container restart <CONTAINER ID / CONTAINER NAMES>
```

指令後方可以輸入 `CONTAINER ID` 或 `CONTAINER NAMES` 去重新啟動容器

```shell
$ docker container restart a7d3fc447e45
```

## `docker stop` 與 `docker rm` 的差異

### `docker stop` 停止

停止 container 並保留在 `docker ps -a` 的清單中，讓你有機會可以去做 commit，產生一個全新的 container

### `docker rm` 移除

移除已經停止的 container，在 `docker ps -a` 的清單將看不到這個 container

在 container 執行中時，是無法使用 `docker rm` 指令去移除，必須要先用 `docker stop` 停止 container 後才能移除

若要強制移除執行中的 container，可以加入 `-f` 參數強制移除他

```shell
$ docker rm -f <CONTAINER_ID>
```

## 移除所有 container

```shell
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

## 參考資料
* [终止 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/container/stop.html)
