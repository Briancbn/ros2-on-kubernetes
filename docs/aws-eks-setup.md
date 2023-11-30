# Create EKS cluster

First create a key pair for SSH `rmf-eks-sample-key` access to the node

```bash
aws ec2 create-key-pair \
    --key-name rmf-eks-sample-key \
    --key-type rsa \
    --key-format pem \
    --query "KeyMaterial" \
    --output text > rmf-eks-sample-key.pem
```

Create the cluster with no K8S Node initialized

```bash
eksctl create cluster \
  --name rmf-eks-sample \
  --nodes 0 \
  --ssh-access \
  --ssh-public-key rmf-eks-sample-key
```

Configure your computer to communicate with your cluster

```bash
aws eks update-kubeconfig --name rmf-eks-sample
```

Verify the configuration by listing the pods

```bash
kubectl get pods --all-namespaces
```

You should be able to see the `weave-net` pods in the output

If the Cluster is created using IAM account, the root account might not be able to see from the AWS dashboard. 

To enable root account access

```shell
kubectl edit configmap aws-auth -n kube-system
```

Add in the following lines

```yaml
mapUsers: |
  - userarn: arn:aws:iam::[account_id]:root
    groups:
    - system:masters
```

## Install WeaveNet Network CNI Plugins

Delete Default VPC CNI and install WeaveNet CNI. This is needed in order for multicast to work for ROS2 DDS

```bash
kubectl delete ds aws-node -n kube-system
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

Edit instance security group (security group that includes `ClusterSharedNodeSecurityGroup` in its name) to allow **TCP 6783** and **UDP 6783/6784** ports

![security-groups](/home/chenbn/k8s-ros2-demo/docs/security-groups.png)

Create a nodegroup with 2 nodes

```bash
eksctl create nodegroup \
  --cluster rmf-eks-sample \
  --name rmf-eks-sample-ng0 \
  --node-type t3.xlarge \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key rmf-eks-sample-key
```

Verify that the weavenet nodes are running

```bash
kubectl get nodes --all-namespaces
```

