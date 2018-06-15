# 匯出與匯入容器

## 匯出容器

**查詢執行中的 Container**

```shell
docker ps -a
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS                      PORTS                    NAMES
a7d3fc447e45        kejyun/ubuntu1804docker:0.2   "/bin/bash"              23 hours ago        Up 22 hours                 80/tcp                   gracious_hugle
```

**匯出 Container**

```shell
docker export <CONTAINER ID> > <EXPORT.tar>
```

```shell
docker export a7d3fc447e45 > ubuntu1804docker.tar
```

## 匯入容器

```shell
cat <EXPORT.tar> | docker import - <主機名>/<資源庫:版本號>
```

```shell
$ cat ubuntu1804docker.tar | docker import - test/ubuntu:0.4

sha256:5290d40aa1a5ef19135623977354afd95dce2d1df4d5a5fdcccbf6bb3550cf68
```

**查詢是否匯入成功**

```shell
$ docker images
REPOSITORY                        TAG                 IMAGE ID            CREATED                  SIZE
test/ubuntu                       0.4                 5290d40aa1a5        Less than a second ago   126MB
```

## 參考資料
* [导出和导入 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/container/import_export.html)
