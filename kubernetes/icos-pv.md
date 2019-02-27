# ICOS에 PV 데이터 백업하기

## 1. Intro

{% hint style="info" %}
목적 : 

1. _**IKS**_\(IBM Kubernetes Service\)의 PV 데이터를 _**ICOS**_\(IBM Cloud Object Storage\)에 백업하기 
{% endhint %}

## 2. 공용 IBM K8S 이미지에 접근하기 

```bash
ibmcloud cr region-set global
ibmcloud cr images --include-ibm
```

## 3. ICOS Plug-in Helm 차트 설치하기

link : [https://console.bluemix.net/containers-kubernetes/solutions/helm-charts/ibm/ibmcloud-object-storage-plugin](https://console.bluemix.net/containers-kubernetes/solutions/helm-charts/ibm/ibmcloud-object-storage-plugin)

## 4. ibm-backup-restore 이미지로 Pod 배포하기 

Link : [https://console.bluemix.net/docs/services/RegistryImages/ibm-backup-restore/index.html\#ibmbackup\_restore\_starter](https://console.bluemix.net/docs/services/RegistryImages/ibm-backup-restore/index.html#ibmbackup_restore_starter)

