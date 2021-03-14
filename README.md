Kubernetes Cron Jobs Example on Amazon EKS
==========================================

# Quick Start

## 0. Prerequisites

- [kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- [eksctl](https://github.com/weaveworks/eksctl)

## 1. Create Cluster

ref https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html

```
$ eksctl create cluster \
  --profile <aws_profile>
  --name <my_cluster> \
  --region <region> \
  --fargate
```

## 2. IAM RoleとKubernetes service accountの作成

ref https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html

Kubernets ServiceAccountとIAM Roleを作成しその２つを関連付ける。IAM Roleには指定したIAM PolicyがAttachされます。参考: https://aws.amazon.com/jp/blogs/news/introducing-fine-grained-iam-roles-service-accounts/

```sh
eksctl create iamserviceaccount \
    --profile <aws_profile> \
    --name <service_account_name> \
    --namespace <service_account_namespace> \
    --cluster <cluster_name> \
    --attach-policy-arn <IAM_policy_ARN> \
    --approve \
    --override-existing-serviceaccounts
```

## 3. Start Cron Jobs


```sh
$ kubectl apply -f ./jobs -R
```


