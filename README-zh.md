# Istio and OpenCensus 101 - Lightning Demo


forked from [thesandlord/Istio101](https://github.com/thesandlord/Istio101)


练习此任务之前，需要安装 kubernetes 和 istio ，可以参考 [vagrant-kubeadm](https://github.com/kuops/vagrant-kubeadm)，初始化一个集群


# Code

Code 作用是，向下游服务生成一个请求，获取请求结果，并且把 name ，延迟信息，和 下游 url 连接起来


# build 镜像

```
# 克隆代码

git clone https://github.com/kuops/Istio101.git

# 更改为自己的 repo 名称
DOCKER_REPO=kuopsme

# Build Docker 镜像
cd Istio101
docker build -t ${DOCKER_REPO}/istiotest:1.0 ./code/code-only-istio
docker build -t ${DOCKER_REPO}/istio-opencensus-simple:1.0 ./code/code-opencensus-simple
docker build -t ${DOCKER_REPO}/istio-opencensus-full:1.0 ./code/code-opencensus-full
docker push ${DOCKER_REPO}/istiotest:1.0
docker push ${DOCKER_REPO}/istio-opencensus-simple:1.0
docker push ${DOCKER_REPO}/istio-opencensus-full:1.0
```

> 这三个镜像，一个用于 `vanilla Istio` 演示，其余两个用于 `OpenCensus` 演示


部署 kubernetes

```
kubectl apply -f ./configs/kube/services.yaml
sed "s@<DOCKER_REPO>@${DOCKER_REPO}@g" ./configs/kube/deployments.yaml | kubectl apply -f -
```
