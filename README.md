0. 환경설정
```shell
export PROJECT_ID=$(gcloud config get-value project)
export REGION=us-central1

gcloud services enable clouddeploy.googleapis.com
```

1. GKE 클러스터 생성
```shell
gcloud container clusters create-auto helm-demo-dev --region ${REGION}
```

```shell
gcloud container clusters create-auto helm-demo-prod --region ${REGION}
```

2. Artifact Registry
```shell
gcloud artifacts repositories create docker-repo \
    --repository-format=docker \
    --location=${REGION}
```

3. 코드 저장소 설정 (Cloud Source Repository)
```
git clone https://github.com/kiwonl/helm-demo.git
cd helm-demo

sed -i s/PROJECT_ID/${PROJECT_ID}/g skaffold.yaml
sed -i s/PROJECT_ID/${PROJECT_ID}/g helm/dev/values.yaml
sed -i s/PROJECT_ID/${PROJECT_ID}/g helm/prod/values.yaml
```

```
git init
git add .
git commit -m "initial"

# in case of Qwiklab platform
export USER_NAME=$(gcloud config get-value account)
git config --global user.email ${USER_NAME}
git config --global user.name ${USER_NAME_SPLIT=($(echo $USER_NAME | tr "@" "\n"));echo ${USER_NAME_SPLIT[0]}}
```
- Add Respository
- Create new Repository

<p align="left">
<img src="/doc/img/csr.png" width="800" alt="CSR" />
</p>


4. IAM 설정
- GCE SA : Editor, Cloud Deploy Runner
- Cloud Build SA : Service Account User, Cloud Deploy Operator


5. Cloud Deploy
```shell
sed -i s/PROJECT_ID/${PROJECT_ID}/g clouddeploy.yaml
sed -i s/REGION/${REGION}/g clouddeploy.yaml

gcloud deploy apply --file=clouddeploy.yaml \
  --region=${REGION}
```

6. Cloud Build Trigger 설정
