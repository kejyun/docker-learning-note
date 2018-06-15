# 容器互相連線

## 建立網路介面

```shell
docker network create -d <介面驅動 Driver> <介面名稱 Name>
```

**介面驅動**

* bridge
* overlay


**指令**

```shell
docker network create -d bridge my-network
```

## 目前所有網路介面

```shell
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
0d3e1401d7cf        bridge              bridge              local
e6874a337aae        host                host                local
5a17e1d50e2d        my-network          bridge              local
c724e3ea2870        none                null                local
```

## 1. 啟動容器，名稱為 my_container_a，使用 my-network 網路介面

使用 `--network` 參數可以指定網路介面

```shell
docker run -it --rm --name my_container_a --network my-network ubuntu sh
```

**安裝 ping 套件**

容器基本上沒有安裝任何的其它套件，我們需要 ping 套件讓我們可以去確認可以連線到其他容器，所以需要額外安裝 ping 套件

```shell
$ apt-get update
$ apt-get install iputils-ping
```

## 2. 啟動容器，名稱為 my_container_b，使用 my-network 網路介面

```shell
docker run -it --rm --name my_container_b --network my-network ubuntu sh
```

**安裝 ping 套件**

```shell
$ apt-get update
$ apt-get install iputils-ping
```

## Ping 其他容器


```shell
ping my_container_a
PING my_container_a (172.18.0.2) 56(84) bytes of data.
64 bytes from cf56372437f2 (172.18.0.2): icmp_seq=1 ttl=64 time=0.036 ms
64 bytes from cf56372437f2 (172.18.0.2): icmp_seq=2 ttl=64 time=0.076 ms
64 bytes from cf56372437f2 (172.18.0.2): icmp_seq=3 ttl=64 time=0.067 ms
64 bytes from cf56372437f2 (172.18.0.2): icmp_seq=4 ttl=64 time=0.046 ms
```

```shell
ping my_container_b
PING my_container_b (172.18.0.3) 56(84) bytes of data.
64 bytes from my_container_b.my-net (172.18.0.3): icmp_seq=1 ttl=64 time=0.081 ms
64 bytes from my_container_b.my-net (172.18.0.3): icmp_seq=2 ttl=64 time=0.118 ms
64 bytes from my_container_b.my-net (172.18.0.3): icmp_seq=3 ttl=64 time=0.078 ms
64 bytes from my_container_b.my-net (172.18.0.3): icmp_seq=4 ttl=64 time=0.118 ms
```

如果你有多個容器需要互相連線，可以使用 Docker Compose。

## 參考資料
* [容器互联 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/network/linking.html)
* [Docker - Ubuntu - bash: ping: command not found - Stack Overflow](https://stackoverflow.com/questions/39901311/docker-ubuntu-bash-ping-command-not-found)
