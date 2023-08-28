0. 환경설정
```shell
export PROJECT_ID=$(gcloud config get-value project)
export USER_NAME=$(gcloud config get-value account)

gcloud services enable clouddeploy.googleapis.com
```

1. 코드 저장소 설정 (Cloud Source Repository)
```
git clone https://github.com/kiwonl/helm-demo.git
cd helm-demo
git init
git add .
git config --global user.email ${USER_NAME}
git config --global user.name ${USER_NAME_SPLIT=($(echo $USER_NAME | tr "@" "\n"));echo ${USER_NAME_SPLIT[0]}}
```
<p align="left">
<img src="/doc/img/csr.png" width="800" alt="CSR" />
</p>


2. GKE 클러스터 생성
```shell
gcloud container clusters create-auto helm-demo-test --region us-central1
```

```shell
gcloud container clusters create-auto helm-demo-prod --region us-central1
```

3. IAM 설정
- GCE SA : Editor, Cloud Deploy Runner
- Cloud Build SA : Service Account User, Cloud Deploy Operator


4. Artifact Registry
```shell
gcloud artifacts repositories create docker-repo \
    --repository-format=docker \
    --location=us-central1
```


5. Cloud Deploy
```shell
sed -i s/PROJECT_ID/${PROJECT_ID}/g clouddeploy.yaml

gcloud deploy apply --file=clouddeploy.yaml \
  --region=us-central1
```
