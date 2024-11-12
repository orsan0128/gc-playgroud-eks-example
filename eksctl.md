# eksctl

## 官網 https://eksctl.io/

```
於gc-playground 建立eks

eksctl create cluster --name gc-playground-eks --region ap-northeast-1
```

eksctl 會使用幾個cloudformation 去依序建立相關VPC , nodegroup .....相關資源

相關紀錄
```
2024-11-06 17:01:13 [!]  recommended policies were found for "vpc-cni" addon, but since OIDC is disabled on the cluster, eksctl cannot configure the requested permissions; the recommended way to provide IAM permissions for "vpc-cni" addon is via pod identity associations; after addon creation is completed, add all recommended policies to the config file, under `addon.PodIdentityAssociations`, and run `eksctl update addon`

eksctl utils associate-iam-oidc-provider --region ap-northeast-1 --cluster gc-playground-eks --approve

eksctl update addon --name vpc-cni --cluster gc-playground-eks --region ap-northeast-1

安裝完後 Addons 會有  kube-prox , vpn-cni , coredns .

eksctl get nodegroup --cluster gc-playground-eks --region ap-northeast-1

CLUSTER			NODEGROUP	STATUS	CREATED			MIN SIZE	MAX SIZE	DESIRED CAPACITY	INSTANCE TYPE	IMAGE ID	ASG NAME					TYPE
gc-playground-eks	ng-2e3c9ec2	ACTIVE	2024-11-06T09:03:43Z	2		2		2			m5.large	AL2_x86_64	eks-ng-2e3c9ec2-c4c9805b-8dc0-c1b8-0368-686a705f89b4	managed
```

# Create a basic cluster in minutes with just one command
```
eksctl create cluster
```

default parameters
```
* exciting auto-generated name, e.g., fabulous-mushroom-1527688624
* two m5.large worker nodes (this instance type suits most common use-cases, and is good value for money)
* use the official AWS EKS AMI
* us-west-2 region
* a dedicated VPC (check your quotas)
```

Example output
```
 $ eksctl create cluster
  [ℹ]  using region us-west-2
  [ℹ]  setting availability zones to [us-west-2a us-west-2c us-west-2b]
  [ℹ]  subnets for us-west-2a - public:192.168.0.0/19 private:192.168.96.0/19
  [ℹ]  subnets for us-west-2c - public:192.168.32.0/19 private:192.168.128.0/19
  [ℹ]  subnets for us-west-2b - public:192.168.64.0/19 private:192.168.160.0/19
  [ℹ]  nodegroup "ng-98b3b83a" will use "ami-05ecac759c81e0b0c" [AmazonLinux2/1.11]
  [ℹ]  creating EKS cluster "floral-unicorn-1540567338" in "us-west-2" region
  [ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial nodegroup
  [ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-west-2 --cluster=floral-unicorn-1540567338'
  [ℹ]  2 sequential tasks: { create cluster control plane "floral-unicorn-1540567338", create nodegroup "ng-98b3b83a" }
  [ℹ]  building cluster stack "eksctl-floral-unicorn-1540567338-cluster"
  [ℹ]  deploying stack "eksctl-floral-unicorn-1540567338-cluster"
  [ℹ]  building nodegroup stack "eksctl-floral-unicorn-1540567338-nodegroup-ng-98b3b83a"
  [ℹ]  --nodes-min=2 was set automatically for nodegroup ng-98b3b83a
  [ℹ]  --nodes-max=2 was set automatically for nodegroup ng-98b3b83a
  [ℹ]  deploying stack "eksctl-floral-unicorn-1540567338-nodegroup-ng-98b3b83a"
  [✔]  all EKS cluster resource for "floral-unicorn-1540567338" had been created
  [✔]  saved kubeconfig as "~/.kube/config"
  [ℹ]  adding role "arn:aws:iam::376248598259:role/eksctl-ridiculous-sculpture-15547-NodeInstanceRole-1F3IHNVD03Z74" to auth ConfigMap
  [ℹ]  nodegroup "ng-98b3b83a" has 1 node(s)
  [ℹ]  node "ip-192-168-64-220.us-west-2.compute.internal" is not ready
  [ℹ]  waiting for at least 2 node(s) to become ready in "ng-98b3b83a"
  [ℹ]  nodegroup "ng-98b3b83a" has 2 node(s)
  [ℹ]  node "ip-192-168-64-220.us-west-2.compute.internal" is ready
  [ℹ]  node "ip-192-168-8-135.us-west-2.compute.internal" is ready
  [ℹ]  kubectl command should work with "~/.kube/config", try 'kubectl get nodes'
  [✔]  EKS cluster "floral-unicorn-1540567338" in "us-west-2" region is ready
```
