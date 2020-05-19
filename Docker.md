# Tutorial
* https://philipzheng.gitbook.io/docker_practice/
* https://ithelp.ithome.com.tw/articles/10199339

# Dockerfile
* Official website: https://docs.docker.com/engine/reference/builder/

### Dockerfile 分為四部分：
1. 基底映像檔資訊
2. 維護者資訊
3. 映像檔操作指令
4. 容器啟動時執行指令

### 說明
* 使用 `#` 來註釋
* `FROM` 指令告訴 Docker 使用哪個映像檔作為基底
* `MAINTAINER` 是維護者的信息
* `RUN` 開頭的指令會在建立映像檔時執行
    * `RUN` 當命令較長時可以使用 `\` 來換行
    * 在 shell 中執行 `RUN /bin/sh -c`
    * 使用 exec 執行 `RUN ["executable", "param1", "param2"]`
    * 比如使用 apt-get 來安裝一個套件 `RUN apt-get -y update && apt-get install -y supervisor`
* `ADD` 命令複製本地檔案到映像檔
* `EXPOSE` 命令向外部開放埠號
* `CMD` 命令描述容器啟動後執行的程序等，每個 Dockerfile 只能有一條 `CMD` 命令 ，如果指定了多條命令，只有最後一條會被執行
    * 使用 exec 執行 `CMD ["executable", "param1", "param2"]`
    * 在 shell 中執行，需要互動的指令的情況時使用 `CMD command param1 param2`
    * 提供給 `ENTRYPOINT` 的預設參數 `CMD ["param1", "param2"]`
* `ENTRYPOINT`：每個 Dockerfile 中只能有一個 `ENTRYPOINT`，當指定多個時，只有最後一個會生效。

### Example 1
```
# 載入 Node.js 需要的執行環境
# 每個不同的程式需要的環境可能都不同，這裏下載的是 node:10.15.3-alpine
# 詳細的其他版本可以在 Dockerhub 上看到
FROM node:10.15.3-alpine

# 在這個 Docker 的環境之中建立一個工作目錄 /app
WORKDIR /app

# 把跟 Dockerfile 同個資料夾的程式加到剛建立的工作目錄 /app 中
ADD . /app

# 運行 npm install，讓 npm 透過讀取 package.json 下載相依的 package
RUN npm install

# 指定 container 對外開放的 port
EXPOSE 3000

# 透過 node index.js 來執行我們的 Server
CMD node index.js    # execute node index.js
```
### Example 2
```
# 使用官方的 Python 執行環境作為基本的 Docker 影像
FROM python:2.7-slim

# 設定工作目錄為 /app
WORKDIR /app

# 複製目前目錄下的內容，放進 Docker 容器中的 /app
ADD . /app

# 安裝 requirements.txt 中所列的必要套件
RUN pip install -r requirements.txt

# 讓 80 連接埠可以從 Docker 容器外部存取
EXPOSE 80

# 定義環境變數
ENV NAME World

# 當 Docker 容器啟動時，自動執行 app.py
CMD ["python", "app.py"]
```

# Images
* Image is a template
* Image 是用來建立 container 的模板，一個 image 可以建立很多個不同的 container
* Image 是唯讀的
* A image has max 127 layers
* Docker 在執行容器前需要本地端存在對應的映像檔，如果映像檔不存在本地端，Docker 會從映像檔倉庫下載（預設是 Docker Hub 公共註冊伺服器中的倉庫）

# Containers
* Container is an instance of image. Programs run in container
* 容器是獨立執行的一個或一組應用，以及它們的執行態環境
* 使用者可以隨時刪除和新建立容器
* Container 啟動時，會在上面建立一層可以寫入的層級。(最多寫入127層)

# Repository
* Repository is used to keep all images

# Registry 註冊伺服器
* 註冊伺服器是管理倉庫的具體伺服器，每個伺服器上可以有多個倉庫，而每個倉庫下面有多個映像檔

# Docker commands
* Official website: 
    * https://docs.docker.com/engine/reference/commandline/docker/
    * https://docs.docker.com/engine/reference/commandline/cli/
```bash
# help
docker --help

# Version and information
docker -v
docker --version
docker version
docker-compose --version
docker-machine --version
docker info

# List 
docker ps
docker ps -a

# Image 映像檔 常用指令
search          搜尋    docker search centos
pull            下載    docker pull centos
images          列表    docker images
run             執行    docker run -ti centos /bin/bash
rmi [Image ID]  刪除    docker rmi 615cb40d5d19
build           建立    docker build -t member:1 .
login           登入    docker login docker.okborn.com
push            上傳    docker push

# List images
docker images
docker image ls

# Search
docker search image_name

# Build image
docker build . -t image_name
docker build -t image_name .

# 移除本地端的映像檔
docker rmi -f image_name_or_id
# 在刪除映像檔之前要先用 docker rm 刪掉依賴於這個映像檔的所有容器
# -f: 強置刪除

# 新建 container 並啟動
docker run -d -p host_port:docker_port -it image_name_or_image_id
docker run -t -i image_name <program such as /bin/bash> <arguments>
docker run -it --entrypoint bash apni-singlewarehouse:dev
docker run image_run --arg1 arg1 --arg2 arg2
docker exec -it stable_baselines bash
docker run -v volumn_name:/path/in/container -it image_name
docker run -v /local/directory/:/path/in/containder -it image_name
# -d: run in background
# -p: Port 對應(host port:container port)
# -t: 讓 Docker 分配一個虛擬終端（pseudo-tty）並綁定到容器的標準輸入上，
# -i: 則讓容器的標準輸入保持打開
# --rm: 執行完後會自動刪除 Container
# -e：設定環境變數(AAA=BBB)
# -v：設定 volume 資料對應(host folder:container folder)
# --name：設定容器名稱


# Container 容器 常用指令
run                 新建或啟動      docker run -d centos
start [Contain ID]  啟動           docker start a469b9226fc8
stop [Contain ID]   停止           docker stop a469b9226fc8
rm [Contain ID]     刪除           docker rm a4
ps -a               列表           docker ps -a
logs [Contain ID]   查看容器內的資訊 docker logs -f a4
exec [Contain ID]   進入容器(開新console)    docker exec -ti a4 /bin/bash
attach              進入容器(退出停止容器)    dockr attach a4
inspect             查看           docker inspect a4

# 進入容器進行操作
docker exec -it contain_name_or_id bash
docker attach contain_name
# -e：設定環境變數(AAA=BBB)

# 將一個已經終止的容器啟動執行
docker start container_id
docker attach container_id

# 將一個執行中的容器終止，然後再重新啟動它
docker restart

# Stop 終止一個執行中的容器
docker stop container_id

# 移除容器
docker rm contain_name_or_id 刪除一個處於終止狀態的容器
# -f: 刪除一個執行中的容器

# 取得容器的輸出訊息
docker logs containdr_id
# -f：不會跳出，會一直列印最新的log資訊

# 列出容器，等價於 docker ps
docker container ls

# Transfer files
docker cp target_file contain_id:/path/to/file
docker cp contain_id:/path/to/file destination
docker cp b149f9c52b01:/home/projects/yuting/swh.tgz .
docker cp config_dominos_pipeline_test.json b149f9c52b01:/home/projects/yuting/singlewarehouse
docker cp calendar_dominos_pipeline_test.json b149f9c52b01:/home/projects/yuting/singlewarehouse


# Login Docker Cloud
docker login

# Tag
docker tag image_name_or_image_id username/repository:tag
docker tag image_id repository:tag

# Commit
docker commit -m "message" container_id username/repository:tag

# Pull from docker hub
docker pull image_name

# Push to docker hub
docker push username/repository:tag
docker push repository:tag

# 建立映像檔到本地檔案
docker save image_name > output_file.tar
docker save -o output_file.tar image_name

# 從建立的本地檔案中再匯入到本地映像檔庫
docker load --input input_file.tar
docker load < input_file.tar

# 匯出本地某個容器 將匯出容器快照到本地檔案
docker export contain_id > output_file
docker export --output="output_file.tar" image_name

# 使用 docker import 從容器快照檔案中再匯入為映像檔
docker import < input_file.tar
docker import - username/repository:tag
docker import input_file_URL username/repository

# 新增一個 volume 用來讓本機和 container 互通檔案
docker volume create --name volumn_name
docker volume ls
docker volumn rm volumn_name



docker top hash-id # 秀出 Container 正在執行的 process
docker kill hash-id


# Registry 倉庫 常用指令
commit  容器存檔  docker commit db aaa:v1
pull    下載  docker pull docker.okborn.com/okborn:base
tag     標籤  docker tag aaa docker.okborn.com/aaa
push    上傳  docker push docker.okborn.com/member:1
login   登入  docker login docker.okborn.com
export  匯出  docker export 7691a814370e > ubuntu.tar
import  匯入  cat ubuntu.tar sudo docker import - test/ubuntu:v1.0


上傳到 GCP Registry

gcloud docker -- push  asia.gcr.io/joyi-205504/aaa:v1

docker network ls

docker stats

docker tag IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]
```

* `docker load` 和 `docker import` 的區別

使用者既可以使用 docker load 來匯入映像檔儲存檔案到本地映像檔庫，也可以使用 docker import 來匯入一個容器快照到本地映像檔庫。這兩者的區別在於容器快照檔案將丟棄所有的歷史記錄和原始資料訊息（即僅保存容器當時的快照狀態），而映像檔儲存檔案將保存完整記錄，檔案體積也跟著變大。此外，從容器快照檔案匯入時可以重新指定標籤等原始資料訊息。
