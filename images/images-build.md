# 建立映像檔


## 1. 修改映像檔去建議新的映像檔

**啟動 ubuntu 18.04**

```shell
$ sudo docker run -t -i ubuntu:18.04 /bin/bash
root@1436aace3d48:/#
```

### 測試 curl 指令

在一開始建立最初始的系統，沒有包含任何東西，連 curl 也沒有

```shell
root@1436aace3d48:/# curl
bash: curl: command not found
```

我們可以安裝 curl，或任何的東西，打包成一個我們需要的環境，然後將這個環境打包起來

```shell
# 更新 apt-get
root@1436aace3d48:/# apt-get upgrade
root@1436aace3d48:/# apt-get update

# 安裝 curl
root@1436aace3d48:/# apt-get install curl
```


### commit 修改的映像檔

使用 `exit` 指令離開修改後的容器
```shell
root@1436aace3d48:/# exit
```

輸入 commit 訊息，在後面加入剛剛啟動的 `CONTAINER ID`，此容器編號為 `root@1436aace3d48:/#` root@ 後方的那一串編號，並指定遠端的倉庫是那一個，在這裡為 `kejyun/ubuntu1804`，在倉庫後可以加入版本號 `0.1`，指令如下：

> docker commit -m 'Commit 的訊息' <CONTAINER ID> <倉庫:版本號>

```shell
$ docker commit -m 'Add curl' 1436aace3d48 kejyun/ubuntu1804:0.1

sha256:8c8347f200e825c79a363878626f80809fddbe17fb0042d8beb7c8084fcc4aed

$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
kejyun/ubuntu1804    0.1                 a9b3724e2ee3        Less than a second ago   137MB
```

**不指定倉庫及版本**


```shell
$ docker commit -m 'Add curl' 1436aace3d48

sha256:3da67fb2dba41a3352a827e562a57cba9dfccf4b9759f6b4364161bfee0b7a5a

$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
<none>               <none>              3da67fb2dba4        2 seconds ago       137MB
```

**指定倉庫不指定版本**

```shell
$ docker commit -m 'Add curl'  1436aace3d48 kejyun/ubuntu1804

sha256:8c8347f200e825c79a363878626f80809fddbe17fb0042d8beb7c8084fcc4aed

$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
kejyun/ubuntu1804    latest              8c8347f200e8        Less than a second ago   137MB
```

### 進入 commit 版本映像檔

剛剛將映像檔 commit 後，可以直接用 `docker run` 啟動該版本的映像檔

```shell
$ sudo docker run -t -i kejyun/ubuntu1804:0.1 /bin/bash
```


## 2. 使用 Dockerfile 建立映像檔

建立檔案名稱為 `Dockerfile` 的檔案，在 Dockerfile 中輸入來源是哪一個映像檔 `FROM kejyun/ubuntu1804:0.1`，這裡使用的來源為原本上方建立的 `kejyun/ubuntu1804:0.1`

設定完後指定要執行的指令是哪些，在這裡指定安裝了一個 `wget` 的套件，所以預期建立完進入容器後，就可以使用 `wget` 指令

```shell
# 註解，檔案名稱：Dockerfile
FROM kejyun/ubuntu1804:0.1
RUN apt-get update
RUN apt-get upgrade
RUN apt-get install wget
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

| 參數  | 說明  |
|---|---|
| FROM  |  用哪一個映像檔當做 base 去做之後的映像檔 |
| RUN  |  建立映像檔會執行的指令 |

> 一個映像檔，最多不能超過 127 個 RUN 指令

> Increase maximum image depth to 127 from 42

> reference: [moby/CHANGELOG.md at master · moby/moby](https://github.com/moby/moby/blob/master/CHANGELOG.md#072-2013-12-16)



使用 Dockerfile 建立映像檔，指定 tag 為 `kejyun/ubuntu1804docker:0.1`，所以就會從 `kejyun/ubuntu1804:0.1` 映像檔執行 Dockerfile 的指令

> 指令：docker build -t=
> docker build -t "<倉庫:版本號>" <Dockerfile 資料夾路徑>

```shell
$ docker build -t="kejyun/ubuntu1804docker:0.1" .

Sending build context to Docker daemon  2.048kB
Step 1/5 : FROM kejyun/ubuntu1804:0.1
 ---> 0f58b888565e
Step 2/5 : RUN apt-get update
 ---> Running in 6cd82755ea8b
Hit:1 http://archive.ubuntu.com/ubuntu bionic InRelease
Hit:2 http://security.ubuntu.com/ubuntu bionic-security InRelease
Hit:3 http://archive.ubuntu.com/ubuntu bionic-updates InRelease
Get:4 http://archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Fetched 74.6 kB in 2s (34.8 kB/s)
Reading package lists... Done
Removing intermediate container 6cd82755ea8b
 ---> 36a48deac67b
Step 3/5 : RUN apt-get upgrade
 ---> Running in c559f0923632
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Removing intermediate container c559f0923632
 ---> 4d1c46269cae
Step 4/5 : RUN apt-get install wget
 ---> Running in 62a29ea84e88
Reading package lists... Done
Building dependency tree
Reading state information... Done
wget is already the newest version (1.19.4-1ubuntu2.1).
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Removing intermediate container 62a29ea84e88
 ---> 4d45c8de0b0e
Step 5/5 : RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
 ---> Running in 1b05d4f98f8a
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 1603k  100 1603k    0     0   960k      0  0:00:01  0:00:01 --:--:--  960k
Removing intermediate container 1b05d4f98f8a
 ---> a54db3844855
Successfully built a54db3844855
Successfully tagged kejyun/ubuntu1804docker:0.1


docker images
REPOSITORY                TAG                 IMAGE ID            CREATED                  SIZE
kejyun/ubuntu1804docker   0.1                 a54db3844855        Less than a second ago   144MB
kejyun/ubuntu1804         0.1                 0f58b888565e        5 minutes ago            142MB
```

如果 tag 指定為同一個映像檔，則會將原本的映像檔做覆蓋，如下方的指令，執行完後 `IMAGE ID` 則變為相同

```shell
$ docker build -t="kejyun/ubuntu1804:0.1" .

$ docker images
REPOSITORY                TAG                 IMAGE ID            CREATED              SIZE
kejyun/ubuntu1804         0.1                 a54db3844855        About a minute ago   144MB
kejyun/ubuntu1804docker   0.1                 a54db3844855        About a minute ago   144MB
```

使用 `docker bulid` 指令，所有動作皆依照 `Dockerfile` 指令去執行，所以一定要有 `Dockerfile`，當所有指令執行完畢，會有一個最終映像檔的 IMAGE ID，中間所有在 `Dockerfile` 執行 `RUN` 指令產生的容器都會被刪掉。


### Dockerfile RUN

一個 `RUN` 指令指的是一層 containers 需要做的事，每一層的東西並不會在下一層被刪除，會一直跟隨著映像檔，所以在寫 `RUN` 指令時，確保每一層只填寫需要的指令。

此時可以在 `RUN` 指令行尾加入 `\` 命令去做換行，使用 `&&` 符號將不同的指令連接再一起，使用縮排去做排版，並在首行使用 `#` 去寫註解，確保每一層只有執行真正需要的東西。

```shell
# Dockerfile
FROM kejyun/ubuntu1804:0.1
# 安裝 Python
RUN buildPython='apt-get update \
    && apt-get upgrade \
    && apt-get install wget \
    && curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py' $buildPython
```

若沒有使用這樣的方式去建構 Docker 映像檔，很容易做出了很肥的映像檔


## 複製檔案到映像檔

要複製的檔案必須要放在 Dockerfile 的目錄下，用相對路徑去複製

```shell
# Dockerfile
FROM kejyun/ubuntu1804:0.1
# 複製檔案
COPY ./package.json /home
# 安裝 Python
RUN buildPython='apt-get update \
    && apt-get upgrade \
    && apt-get install wget \
    && curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py' $buildPython /home/
```

## 複製目錄到映像檔

整個目錄必須要在 `Dockerfile` 所在目錄

```shell
# Dockerfile
FROM kejyun/ubuntu1804:0.1
# 複製檔案
COPY ./package.json /home
# 複製目錄
COPY ./web /home/web
# 安裝 Python
RUN buildPython='apt-get update \
    && apt-get upgrade \
    && apt-get install wget \
    && curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py' $buildPython /home/
```

若是複製整個目錄，但目錄中有些檔案不想要被複製，可以在 `Dockerfile` 目錄下，新增 `.dockerignore` 檔案，將不需要複製的檔案路徑寫在裡面，寫法與 `.gitignore` 一樣

```shell
# .dockerignore
web/html/not-need-copy.html
web/html/not-need-copy.png
```

## 參考資料
* [docker build | Docker Documentation](https://docs.docker.com/engine/reference/commandline/build/)
* [建立 · 《Docker —— 從入門到實踐­》正體中文版](https://philipzheng.gitbooks.io/docker_practice/content/image/create.html)
* [moby/CHANGELOG.md at master · moby/moby](https://github.com/moby/moby/blob/master/CHANGELOG.md#072-2013-12-16)
* [INFO 0011 Cannot create container with more than 127 parents · Issue #12624 · moby/moby](https://github.com/moby/moby/issues/12624)
* [CMD 容器启动命令 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/image/dockerfile/cmd.html)
* [利用 commit 理解镜像构成 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/image/commit.html)
* [使用 Dockerfile 定制镜像 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/image/build.html)
* [Best practices for writing Dockerfiles | Docker Documentation](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
