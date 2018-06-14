# 進入容器


## docker attach

> docker attach <CONTAINER ID / CONTAINER NAMES>

若要進入已經開啟的 Container，可以使用 `docker attach` 進入 Container

```shell
docker attach a7d3fc447e45
```


但是如果使用 exit 指令離開 container，此 container 會立即被關掉，對於一個線上服務的 container 是比較危險的指令


## docker exec

> docker exec -it <CONTAINER ID / CONTAINER NAMES>

```
$ docker exec -it a7d3fc447e45 bash
```

若要進入已經開啟的 Container，可以使用 `docker exec` 進入 Container

此指令在 Container 中使用 exit 指令離開，不會關掉 container，容器會繼續執行

## 參考資料
* [进入容器 · Docker —— 从入门到实践](https://yeasy.gitbooks.io/docker_practice/container/attach_exec.html)
