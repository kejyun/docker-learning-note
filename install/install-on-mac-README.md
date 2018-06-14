# 在 Mac 安裝 Docker


## 系統需求

1. Mac hardware must be a 2010 or newer model, with Intel’s hardware support for memory management unit (MMU) virtualization, including Extended Page Tables (EPT) and Unrestricted Mode. You can check to see if your machine has this support by running the following command in a terminal: sysctl kern.hv_support

2. macOS El Capitan 10.11 and newer macOS releases are supported. We recommend upgrading to the latest version of macOS.

3. At least 4GB of RAM

4. VirtualBox prior to version 4.3.30 must NOT be installed (it is incompatible with Docker for Mac). If you have a newer version of VirtualBox installed, it’s fine.

## 安裝

從 [Docker Community Edition for Mac - Docker Store](https://store.docker.com/editions/community/docker-ce-desktop-mac) 下載後，直接點選 `Docker.dmg` 檔案，然後將 docker 拖曳到 Application 即可

> 過程中需要輸入密碼，需要管理者權限存取系統資料

![docker mac](./images/docker-app-drag.png)

## docker 版本檢查

```shell
$ docker --version
Docker version 18.03.1-ce, build 9ee9f40
```

```shell
docker-compose --version
docker-compose version 1.21.1, build 5a3f1a3
```

```shell
$ docker-machine --version
docker-machine version 0.14.0, build 9ba6da9
```


## 參考資料
* [Get started with Docker for Mac | Docker Documentation](https://docs.docker.com/docker-for-mac/)
* [Install Docker for Mac | Docker Documentation](https://docs.docker.com/docker-for-mac/install/)
* [Docker Community Edition for Mac - Docker Store](https://store.docker.com/editions/community/docker-ce-desktop-mac)
