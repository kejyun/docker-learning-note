# 刪除映像檔

## 列出目前所有的映像檔

```shell
$ docker images

REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
kejyun/ubuntu1804docker   0.1                 a54db3844855        21 hours ago        144MB
kejyun/ubuntu1804         0.1                 a54db3844855        21 hours ago        144MB
kejyun/ubuntu             0.1                 61853cc80834        21 hours ago        140MB
kejyun/ubuntu1804         v0.1                317f7e8c2321        22 hours ago        137MB
kejyun/ubuntu1804         latest              8c8347f200e8        22 hours ago        137MB
laradock_nginx            latest              2586012f8dad        47 hours ago        26.7MB
laradock_workspace        latest              8f74a5ffe174        47 hours ago        721MB
laradock_mysql            latest              61a7a3f27ca2        47 hours ago        445MB
ubuntu                    16.04               5e8b97a2a082        8 days ago          114MB
ubuntu                    18.04               113a43faa138        8 days ago          81.2MB
ubuntu                    latest              113a43faa138        8 days ago          81.2MB
nginx                     alpine              bc7fdec94612        8 days ago          18MB
nginx                     latest              cd5239a0906a        8 days ago          109MB
mysql                     latest              a8a59477268d        5 weeks ago         445MB
redis                     latest              bfcb1f6df2db        6 weeks ago         107MB
laradock/php-fpm          2.0-72              22f190ba7b61        5 months ago        384MB
laradock/workspace        2.0-72              69a57f6aca78        5 months ago        721MB
```

## 刪除本地映像檔

```shell
$ docker image rm [選項] <映像檔1> [<映像檔2> ...]
```

`<映像檔1>` 可以是 `IMAGE ID`、`映像檔名稱`


**移除 nginx:alpine 映像檔**

`映像檔名稱` 為 `<資源庫名稱 REPOSITORY>:<標籤 TAG>`

```shell
$ docker image rm nginx:alpine
Untagged: nginx:alpine
Untagged: nginx@sha256:4a85273d1e403fbf676960c0ad41b673c7b034204a48cb32779fbb2c18e3839d
```

```shell
$ docker image rm nginx:latest
Untagged: nginx:latest
Untagged: nginx@sha256:3e2ffcf0edca2a4e9b24ca442d227baea7b7f0e33ad654ef1eb806fbd9bedcf0
Deleted: sha256:cd5239a0906a6ccf0562354852fae04bc5b52d72a2aff9a871ddb6bd57553569
Deleted: sha256:530991fd6d0f08206190b1bf71ef51b4534365669785cb461c24d62083f67bb3
Deleted: sha256:725a91602941a09ec4a9ff02dcfde78c2ada44b28780aecd0dfd64d2b2817509
```

## Untagged 和 Deleted

**Untagged**

還有其他不同標籤的映像檔有指定到此映像檔，所以移除的話，僅是將這個標籤移除

**Deleted**

映像檔所有的標籤皆沒有指定到此刪除的映像檔，所以會直接將此映像檔做刪除

container 之間會做互相依賴，若要刪除的映像檔有執行中的 container 那麼此映像檔就不能被刪除，若要真的完整刪除的話，則必須要將這些相互依賴的 container 刪除後，才可以刪除映像檔


## 批次刪除映像檔

```shell
$  docker image rm $(docker image ls -q redis)
Untagged: redis:latest
Untagged: redis@sha256:87275ecd3017cdacd3e93eaf07e26f4a91d7f4d7c311b2305fccb50ec3a1a8cd
Deleted: sha256:bfcb1f6df2db8a62694aaa732a3133799db59c6fec58bfeda84e34299e7270a8
Deleted: sha256:1b2300a82ed1f2881032ae2fa5b6d570297faf1906e054ef1459e356b3fcc569
Deleted: sha256:2ed7c0db59ecf786fa13c85e88489c3c0e5eadc1a24c9da38a62b9f564aac880
Deleted: sha256:faa8313d6bff25bcd501468b3ebb45e3d002454fe7d3a6d72dba77dce0a64d55
Deleted: sha256:ba4122a82d251fc6b432237079bbb0d645ef47049e800c1f9e56d61fc8e823cf
Deleted: sha256:2425077af8e28faee2557ddabb85a00938d81d13cf8c41897958d07aaeaa39d2
Deleted: sha256:ba291263b0854589e32a6fa7775c898d662ed835cd686ac9ac2d33dcefa91a39
```

```shell
$ docker image rm $(docker image ls -q ubuntu)
Untagged: ubuntu:16.04
Untagged: ubuntu@sha256:b050c1822d37a4463c01ceda24d0fc4c679b0dd3c43e742730e2884d3c582e3a
Deleted: sha256:5e8b97a2a0820b10338bd91674249a94679e4568fd1183ea46acff63b9883e9c
Deleted: sha256:ef572e1ba2ecca900f0ec3db00e997de12dd380ce3e360b5813fd75920232359
Deleted: sha256:98fc4d5421178c7be7d5718d2d44abba8053dc5c712e51658fe5b872675b4f7a
Deleted: sha256:7b2cc05dfd889e28234f8831c80ac20cf299d5bbebbbac013f8f7d2b7abc0d65
Deleted: sha256:6b0187d1cdff63eb5966ac72bf4ccd96150586c1409eb858bb98783f02018ee7
Deleted: sha256:644879075e24394efef8a7dddefbc133aad42002df6223cacf98bd1e3d5ddde2
Error response from daemon: conflict: unable to delete 113a43faa138 (cannot be forced) - image has dependent child images
```

## 參考資料
* [删除本地镜像 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/image/rm.html)
