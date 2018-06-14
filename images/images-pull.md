# 下載映像檔


## 下載 ubuntu 16.04

```shell
$ sudo docker pull ubuntu:16.04
16.04: Pulling from library/ubuntu
b234f539f7a1: Pull complete
55172d420b43: Pull complete
5ba5bbeb6b91: Pull complete
43ae2841ad7a: Pull complete
f6c9c6de4190: Pull complete
Digest: sha256:b050c1822d37a4463c01ceda24d0fc4c679b0dd3c43e742730e2884d3c582e3a
Status: Downloaded newer image for ubuntu:16.04
```

## 下載最新 ubuntu

```shell
$ sudo docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
6b98dfc16071: Pull complete
4001a1209541: Pull complete
6319fc68c576: Pull complete
b24603670dc3: Pull complete
97f170c87c6f: Pull complete
Digest: sha256:5f4bdc3467537cbbe563e80db2c3ec95d548a9145d64453b06939c4592d67b6d
Status: Downloaded newer image for ubuntu:latest
```

> 如果沒有指定 TAG，預設使用 latest



## 參考資料
* [library/ubuntu - Docker Hub](https://hub.docker.com/_/ubuntu/)
