Kubernetes Cron Jobs Example on EKS
====================================

# Quick Start

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

ref: https://docs.aws.amazon.com/eks/latest/userguide/iam-roles-for-service-accounts.html

#### Prerequisites

- OIDC Provider作成済みであること
  - https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html
- IAM Roleにattachするpolicy作成しておくこと

Kubernets ServiceAccountとIAM Roleを作成しその２つを関連付ける。IAM Roleには指定したIAM PolicyがAttachされる

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

確認

```
$ kubectl get cronjobs -o wide
```

ログの確認

```
$ stern example-
```

