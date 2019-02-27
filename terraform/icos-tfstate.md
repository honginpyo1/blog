---
description: ICOS를 Terraform backend로 설정하기 with serverless
---

# ICOS에 tfstate을 저장하여 원격 관리하기

## 1. Intro

{% hint style="info" %}
목적 : 

1. tfstate을 IBM Cloud Object Storage에 저장하여 원격 관리하기
2. Lock을 걸어 동시 작업 불가능 하도록 하기 
{% endhint %}

Terraform Meetup에 참석한 후, 타사 Cloud vendor 얘기만 있어서... 욱하는 마음에 IBM 것도 찾아봤다. 

가장 큰 차이점은 AWS 경우 Postgre DB의 Locking 기능을 이용하지만, 이 글은 IBM Backend 서비스인 Functions로 Lock 기능을 구현해서 동시 접근이 안 되도록 했다.

이 글은 아래 글을 번역해서 포스팅했다. 원문은 영어이니 필요하면 아래 링크를 참고하길 바란다.  
Link : [https://github.com/l2fprod/serverless-terraform-backend](https://github.com/l2fprod/serverless-terraform-backend)

Terraform은 다양한 [backend types](https://www.terraform.io/docs/backends/types/index.html)를 제공한다. 그 중 [http](https://www.terraform.io/docs/backends/types/http.html) 백앤드 방식은 REST Client 를 사용하여 Terraform states 파일을 저장한다.

이 프로젝트는 IBM의 서버리스 [IBM Cloud Functions](https://console.bluemix.net/openwhisk?cm_sp=dw-bluemix-_-kr-_-devcenter)와 [IBM Cloud Object Storage](https://console.bluemix.net/catalog/services/cloud-object-storage?cm_sp=dw-bluemix-_-kr-_-devcenter)를 Terraform states의 REST backend로 사용하여 optional [state locking](https://www.terraform.io/docs/state/locking.html)와 versioning 기능을 구현했다.

## 2. Cloud Object Storage 생성

[IBM Cloud console](https://console.bluemix.net/?cm_sp=dw-bluemix-_-kr-_-devcenter)에 접속하여 [Cloud Object Storage](https://console.bluemix.net/catalog/services/cloud-object-storage?cm_sp=dw-bluemix-_-kr-_-devcenter) 서비스를 생성한 후, Terraform states가 저장될 bucket을 생성한다.

![Create Bucket](https://github.com/honginpyo1/manage_tfstate_ibmcloud/blob/master/xdocs/create-bucket.png?raw=true)

서비스 Credential을 생성한다. 이 때, _인라인 구성 매개변수 추가_ 에 _**{"HMAC" : true}**_ 를 입력하여 _**access\_key\_id**_ 와 _**secret\_access\_key**_ 를 같이 생성한다.

![Create Credential](https://github.com/honginpyo1/manage_tfstate_ibmcloud/blob/master/xdocs/create-credentials.png?raw=true)

![](../.gitbook/assets/image%20%2824%29.png)

원하시는 지역의 API endpoint를 기록해 둔다. 서울의 경우 _**단일 사이트**_, _**seo01**_ 로 설정하면 된다.

![API Endpoint](https://github.com/honginpyo1/manage_tfstate_ibmcloud/blob/master/xdocs/api-endpoints.png?raw=true)

## 3. 서버리스 백엔드 생성

git clone 명령어를 통해 파일을 내려받는다.

```bash
git clone https://github.com/l2fprod/serverless-terraform-backend.git
```

actions/template.local.env 파일을 actions/local.env 로 복사한다.

```bash
cp actions/template.local.env actions/local.env
```

actions/local.env를 텍스트 에디터로 열어 Cloud Object Storage의 Credential을 작성한 뒤 저장한다.

```bash
vi actions/local.env
```

기 정의된 서버리스 백앤드의 action을 배포한다.

```bash
cd actions
./deploy --install
```

API Gateway를 통해 해당 Action의 API를 외부로 노출한다.

```bash
cd actions
./deploy --installApi
```

{% hint style="info" %}
API Gateway endpoint를 기억해둔다. 아래 링크와 유사https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/abcdefabecdef0192837465/terraform/1/backend
{% endhint %}

## 4. Terraform 설정

terraform/backend.tf을 편집하여 address를 API Gateway endpoint로 작성한다.

{% hint style="info" %}
주소의 query 매개 변수를 사용하여 해당 state 파일에서 env 이름을 설정하거나 versioning을 사용하도록 설정한다. 예를 들어 이 주소는 us/south/staging을 키로 사용하여 상태를 유지하고 versioning을 합니다.



> https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/abcdefabecdef0192837465/terraform/1/backend?env=us/south/staging&amp;versioning=true
{% endhint %}

만약 locking을 원한다면 lock\_address와 unlock\_address의 주석을 삭제한 뒤, address에 입력한 값을 동일하게 작성한다.   
password는 Cloud Object Storage의 apikey로 작성한다. Cloud Object Storage의 Credential에서 확인 가능하다.

## 5. Terraform을 통해 저장하기

아래 Terraform 명령어를 통해 ICOS에 tfstate을 생성 및 저장한다.

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

위에서 설정한 Bucket을 Cloud Object Storage의 대쉬보드에서 확인해 보면, Terraform state 파일이 생성된 것을 확인할 수 있다.

향후 main.tf 내용을 변경하여 적용하면, ICOS Bucket에 생성되고, 옵션에 따라 Versioning, Locking을 제공한다. ~~물론 serverless로 만듬...~~

{% hint style="info" %}
License  
This project is licensed under the Apache License Version 2.0 \(http://www.apache.org/licenses/LICENSE-2.0\).
{% endhint %}

