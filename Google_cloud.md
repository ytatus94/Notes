
# gcloud
* 安裝 `./google-cloud-sdk/install.sh`
* 初始化 `./google-cloud-sdk/gcloud init`

### Settings
* configuration 在 gcloud CLI 中是 properties 預設是用 default 可以用 gcloud init 或是 gcloud config set 來設定 properties
```bash
# Set project
gcloud config set project development-233900

# Delete project
gcloud projects delete

# 列出 properties
gcloud config list

# 要或不要送資料給 google
gcloud config set disable_usage_reporting true (或 false) 
gcloud init --console-only 設定時不要開啟 broswer
```
gcloud auth list 有哪些帳號在本機上

 

gcloud help

gcloud help compute instances create

getconf LONG_BIT 看幾位元

存取 google cloud storage 上的檔案

gs://bucket_name/path/to/the/file

# Cloud SDK components
```bash
# 安裝 alpha 和 beta 版本
gcloud components install

# Update Google Cloud SDK components
gcloud components update
```

# Google Cloud VM
```
# List all VM
gcloud compute instances list 

# Create VM
gcloud compute instances create <VM_name>

# Start VM
gcloud compute instances start <VM_name> --zone <zone>

# Stop VM
gcloud compute instances stop <VM_name> --zone <zone>

# Login VM
gcloud compute ssh <account@VM_name>

gcloud compute ssh --project development-233900 --zone us-west1-b yu_ting_shen@intel-20190605-105929

gcloud compute --project "project-id" ssh --zone "region" "notebooks-instance -- -L 8080:localhost:8080 
# 然後在瀏覽器中輸入 http://localhost:8080

# Transfer files
gcloud compute scp --recurse yu_ting_shen@intel-20190605-105929:remote_path load_path
# 和 google cloud VM 互相傳送檔案，用這個 download 大檔案會比用 browser download 快



檢查有哪些 model: gcloud ai-platform models list

更新 gcloud: gcloud components update
列出 project id: gcloud config list project --format "value(core.project)"

gcloud ai-platform local train \  這是在本地端跑訓練的指令
--package-path trainer \ 指明要跑哪個 package
--module-name trainer.task \ 指明要跑哪個 module
--job-dir local-training-output 指明輸出到哪邊

gcloud ai-platform jobs submit training $JOB_NAME \ Job name 是唯一的，所以每次跑都需要不同的
--package-path trainer/ \
--module-name trainer.task \
--region $REGION \
--python-version 3.5 \
--runtime-version 1.13 \ 指明用哪個 runtime version 在 AI platform 上跑，和機器還有安裝的套件有關
--job-dir $JOB_DIR \
--stream-logs

gcloud ai-platform jobs submit training ${JOB_NAME}_hpt \
--config hptuning_config.yaml \ 把要調的超參數寫在 yaml 檔案裡面
--package-path trainer/ \
--module-name trainer.task \
--region $REGION \
--python-version 3.5 \
--runtime-version 1.13 \
--job-dir $JOB_DIR \
--stream-logs


檢查 job 的狀態:

gcloud ai-platform jobs describe my_first_keras_job_1

gcloud ai-platform jobs stream-logs my_first_keras_job_1

gcloud ai-platform models create $MODEL_NAME \
--regions $REGION

建立 model version:

gcloud ai-platform versions create $MODEL_VERSION \
--model $MODEL_NAME \
--runtime-version 1.13 \
--python-version 3.5 \
--framework tensorflow \
--origin $SAVED_MODEL_PATH

做預測的命令是:

gcloud ai-platform predict \
--model $MODEL_NAME \
--version $MODEL_VERSION \
--json-instances prediction_input.json

建立新的 bucket: gsutil mb -l $REGION gs://$BUCKET_NAME
gsutil mb -l $REGION gs://$BUCKET_NAME 只有在 bucket 不存在時才要建立新的 bucket
設定 model 在 bucket 內的位置 SAVED_MODEL_PATH=$(gsutil ls $JOB_DIR/keras_export | tail -n 1)


gcloud container clusters get-credentials europe-west6-dominos-new-5b097dbf-gke \
    --zone europe-west6-c \
    --project dominos-pipeline

gcloud builds submit . --config=cloudbuild-eoq.yaml

gcloud config set project dominos-pipeline

gsutil du -h gs://dominos-raw/customers/dominos/raw/*

gcloud config set project development-233900
gcloud compute scp --recurse jupyter@intel-20190605-105929:/home/jupyter/yuting/task* .

gcloud config set project development-233900
gcloud builds submit . --config=cloudbuild-eoq.yaml
kubectl apply -f job_training.yaml


gcloud beta dataproc clusters create apni-cluster \
--optional-components=ANACONDA,JUPYTER \
--image-version=1.3 \
--enable-component-gateway \
--bucket dominos-artifacts \
--region us-central1 \
--project dominos-pipeline

gcloud dataproc jobs submit spark --id e-"$USER"-"$BUCKET"-dev-`date +"%Y%m%d%H%M"`...

gcloud compute ssh incorta --project dominos-pipeline --zone europe-west6-a -- -L 5436:localhost:5436
gcloud compute ssh incorta --project dominos-pipeline --zone europe-west6-a -- -L 8080:localhost:8080
gcloud ai-platform jobs stream-logs singlewarehouse_yuting

gcloud ai-platform jobs describe singlewarehouse_policy_None
gcloud ai-platform jobs stream-logs singlewarehouse_policy_None

$ gcloud config list --format='text(core.project)'
core.project: dominos-pipeline
$ gcloud config list --format='value(core.project)'
dominos-pipeline

gcloud ai-platform jobs cancel JOB_NAME

# Forward port from AI platform to localhost
gcloud compute ssh yt-unifi-3 --project seeloz-production --zone us-west1-b -- -L 6006:localhost:6006
```
