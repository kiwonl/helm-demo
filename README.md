0. 환경설정
```shell
export PROJECT_ID=$(gcloud config get-value project)
gcloud services enable clouddeploy.googleapis.com

git clone https://github.com/kiwonl/helm-deployment.git
```
```shell
gcloud services enable clouddeploy.googleapis.com
```

2. GKE 클러스터 생성

```shell
gcloud container clusters create-auto test-skaffold-helm --region us-central1
```

```shell
gcloud container clusters create-auto prod-skaffold-helm --region us-central1
```

3. Artifact Registry

```shell
gcloud artifacts repositories create docker-repo \
    --repository-format=docker \
    --location=us-central1
```

4. Cloud Deploy

```shell
cd helm-deployment
sed -i s/PROJECT_ID/${PROJECT_ID}/g clouddeploy.yaml

gcloud deploy apply --file=clouddeploy.yaml \
  --region=us-central1
```
