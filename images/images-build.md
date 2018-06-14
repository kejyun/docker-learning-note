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

```Dockerfile
# 註解，檔案名稱：Dockerfile
FROM kejyun/ubuntu1804:0.1
RUN apt-get update
RUN apt-get upgrade
RUN apt-get install wget
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

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
