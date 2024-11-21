cluster-autoscaler-policy.json

{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Action": [
              "autoscaling:DescribeAutoScalingGroups",
              "autoscaling:DescribeAutoScalingInstances",
              "autoscaling:DescribeLaunchConfigurations",
              "autoscaling:DescribeTags",
              "autoscaling:SetDesiredCapacity",
              "autoscaling:TerminateInstanceInAutoScalingGroup",
              "ec2:DescribeLaunchTemplateVersions"
          ],
          "Resource": "*",
          "Effect": "Allow"
      }
  ]
}

aws iam create-policy \
--policy-name AmazonEKSClusterAutoscalerPolicy \
--policy-document file://cluster-autoscaler-policy.json

policy arn
arn:aws:iam::593713876380:policy/AmazonEKSClusterAutoscalerPolicy

curl -o cluster-autoscaler-autodiscover.yaml https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
modify the cluster-autoscaler-autodiscover.yaml
line 165 , to your cluster name

kubectl apply -f cluster-autoscaler-autodiscover.yaml

eksctl create iamserviceaccount \
  --cluster=gc-playground-eks \
  --namespace=kube-system \
  --name=cluster-autoscaler \
  --attach-policy-arn=arn:aws:iam::593713876380:policy/AmazonEKSClusterAutoscalerPolicy \
  --override-existing-serviceaccounts \
  --approve


  kubectl patch deployment cluster-autoscaler \
  -n kube-system \
  -p '{"spec":{"template":{"metadata":{"annotations":{"cluster-autoscaler.kubernetes.io/safe-to-evict": "false"}}}}}'

  kubectl -n kube-system edit deployment.apps/cluster-autoscaler
  找到
  - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/eks-demo
  下面多加
  - --balance-similar-node-groups
  - --skip-nodes-with-system-pods=false


調整deploy pod replicas 超過上限
觀察

kubectl -n kube-system logs -f deployment.apps/cluster-autoscaler
