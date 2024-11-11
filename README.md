# gc-playgroud-eks-example

https://docs.google.com/presentation/d/1XvZfbaePyVA0mQU2W2BwkP798sxI_oooN06I1RzfhF0/edit#slide=id.g3125a9e6449_0_47

Page.11 API Call

API server 溝通
```
aws-vault exec gc-playground
```

設定環境變數
```
export EKS_CLUSTER_NAME=gc-playground-eks
export EKS_API_SERVER=$(aws eks describe-cluster --name $EKS_CLUSTER_NAME --query "cluster.endpoint" --output text)
export TOKEN=$(aws eks get-token --cluster-name $EKS_CLUSTER_NAME --query "status.token" --output text)
```

第一次連EKS
```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER
```
