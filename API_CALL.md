# gc-playgroud-eks-example

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

# 使用 Kubernetes API 取得 kube-system 命名空間中的 Pod
```
curl -k -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" $EKS_API_SERVER/api/v1/namespaces/kube-system/pods
```

jq效果
```
curl -k -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     "$EKS_API_SERVER/api/v1/namespaces/kube-system/pods" | jq
```

只取得Pod name
```
curl -k -H "Authorization: Bearer $TOKEN" \
     -H "Content-Type: application/json" \
     "$EKS_API_SERVER/api/v1/namespaces/kube-system/pods" | jq -r '.items[].metadata.name'
```

取得EKS 版本 try try API server
```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER/version
```

檢查 API 伺服器的健康狀態
```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER/healthz
```

列出所有命名空間
```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER/api/v1/namespaces
```

列出所有節點
```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER/api/v1/nodes
```

列出所有 pod
```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER/api/v1/pods
```

列出所有服務

```
curl -k -H "Authorization: Bearer $TOKEN" $EKS_API_SERVER/api/v1/services
```
