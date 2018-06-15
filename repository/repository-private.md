# 私有資源庫

## docker-registry

### 取得官方 registry 映像檔

```shell
docker run -d -p 5000:5000 --restart=always --name registry registry
```

執行指令啟動後，服務會放在本機 `127.0.0.1:5000`

## 標記映像檔

映像檔規則為 `<主機名>/<資源庫:版本號>`，所以必須將目前的映像檔標記改為 `127.0.0.1:5000/ubuntu1804docker:0.2`


```shell
docker tag kejyun/ubuntu1804docker:0.2 127.0.0.1:5000/ubuntu1804docker:0.2
```

## 上傳映像檔到本地端資源庫

```shell
$ docker push <主機名>/<資源庫:版本號>
```

```shell
$ docker push 127.0.0.1:5000/ubuntu1804docker
```

```shell
$ docker push 127.0.0.1:5000/ubuntu1804docker

The push refers to repository [127.0.0.1:5000/ubuntu1804docker]
fe7af704fb45: Pushed
f2275227fb1d: Pushed
66f24876620a: Pushed
a8645678ca8b: Pushed
d0372d680e20: Pushed
4325494d08e7: Pushed
007964cf94bc: Pushed
e56250f6319b: Pushed
367333e60e93: Pushed
4eac5ec4ca11: Pushed
58de71695ef5: Pushed
e802a24bb41c: Pushed
74a361fc2b8a: Pushed
7aa15b87e975: Pushed
b6f13d447e00: Pushed
a20a262b87bd: Pushed
904d60939c36: Pushed
3a89e0d8654e: Pushed
db9476e6d963: Pushed
0.1: digest: sha256:b0d70651b6ee7fdd0a6a5b611472740de946f3ebda3c7ad1ce3da66555593f5c size: 4285
```

## 查詢本地端資源庫映像檔

```shell
curl 127.0.0.1:5000/v2/_catalog
```

```shell
$ curl 127.0.0.1:5000/v2/_catalog
{"repositories":["ubuntu1804docker"]}
```

使用 curl 指令可以查詢目前所有本地端的所有資源庫

## 下載本地端資源庫映像檔

**移除原本映像檔**

```shell
docker image rm 127.0.0.1:5000/ubuntu1804docker:0.1
```

**下載本地端映像檔**

```shell
$ docker pull 127.0.0.1:5000/ubuntu1804docker:0.1

0.1: Pulling from ubuntu1804docker
Digest: sha256:b0d70651b6ee7fdd0a6a5b611472740de946f3ebda3c7ad1ce3da66555593f5c
Status: Downloaded newer image for 127.0.0.1:5000/ubuntu1804docker:0.1
```
