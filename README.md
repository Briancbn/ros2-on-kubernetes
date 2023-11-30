# ROS2 on Kubernetes

## Prerequisite

- [Setup AWS tools](./docs/setup-aws-tools.md)
- [Install kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

## Kubernetes Setup

The following setup uses WeaveNet CNI Plugins, as it requires no additional configuration for DDS multicast discovery.

- [AWS EKS](docs/aws-eks-setup.md)

## ROS2 topic Demo

Run a ROS2 talker and listener

```bash
cd ./ros2-topic
kubectl apply -f talker.yaml  -f listener.yaml
```

List the pods that are created

```bash
$ kubectl get pods

NAME                           READY   STATUS    RESTARTS   AGE
listener-d0-7657df896f-7w8t5   1/1     Running   0          9s
talker-d0-56f7f6d96-qfbvq      1/1     Running   0          9s
```

In this case, the listener pod is named `listener-d0-7657df896f-7w8t5`, check the logs from the pod

```bash
kubectl logs -f listener-d0-7657df896f-7w8t5
```

To stop the pods

```bash'
cd ./ros2-topic
kubectl delete -f talker.yaml  -f listener.yaml
```

# RMF office Demo

- 