# Istio Book 微服务架构学习 #

## Service Mesh 服务网络 ##
Service Mesh是专用的基础设施层，轻量级高性能网络代理，提供安全地、快速地、可靠地服务间通信，与实际的应用部署在一起，但对应用透明。应用作为服务的发起方，只需要简单的方式将请求发送给本地的网络代理，然后网络代理会进行后续的操作，如服务发现，负载均衡，最后将请求转发给目标服务就。
![Service Mesh架构图](https://github.com/chijinxina/IstioBook/blob/master/doc/service_mesh.png)
## Istio ##
Istio 首先是一个服务网络，但是Istio又不仅仅是服务网格: 在 Linkerd,Envoy 这样的典型服务网格之上，Istio提供了一个完整的解决方案，为整个服务网格提供行为洞察和操作控制，以满足微服务应用程序的多样化需求。
+ **Istio在服务网络中统一提供了许多关键功能：**
1. 流量管理： 控制服务之间的流量和API调用的流向，使得调用更可靠，并使网络在恶劣情况下更加健壮。
2. 可观察性： 了解服务之间的依赖关系，以及它们之间流量的本质和流向，从而提供快速识别问题的能力。
3. 策略执行： 将组织策略应用于服务之间的互动，确保访问策略得以执行，资源在消费者之间良好分配。策略的更改是通过配置网格而不是修改应用程序代码。
4. 服务身份和安全： 为网格中的服务提供可验证身份，并提供保护服务流量的能力，使其可以在不同可信度的网络上流转。
5. 平台支持： Istio旨在在各种环境中运行，包括跨云， 预置，Kubernetes，Mesos等。最初专注于Kubernetes，但很快将支持其他环境。
6. 集成和定制：策略执行组件可以扩展和定制，以便与现有的ACL，日志，监控，配额，审核等解决方案集成。
![Istio架构图](https://github.com/chijinxina/IstioBook/blob/master/doc/istio.png)
+ **Istio的关键功能包括:**
1. HTTP/1.1，HTTP/2，gRPC和TCP流量的自动区域感知负载平衡和故障切换。
2. 通过丰富的路由规则，容错和故障注入，对流行为的细粒度控制。
3. 支持访问控制，速率限制和配额的可插拔策略层和配置API。
4. 集群内所有流量的自动量度，日志和跟踪，包括集群入口和出口。
5. 安全的服务到服务身份验证，在集群中的服务之间具有强大的身份标识。
+ **Istio的关键组件:** https://github.com/istio/istio/releasesIstioBook
1. **istio/mixer**
2. **pilot**
3. **proxy_debug**
4. **istio-ca**

## Istio在kubernetes下安装 [简书](https://www.jianshu.com/p/7b06a122da30) ##
1. **https://github.com/istio/istio/releases 先到下载页面下载 istio-Release**
2. **Istio安装目录：**
  + install/ : 包含了在 k8s 中安装 istio 时使用的 .yaml 文件
  + samples/ : 示例应用
  + bin/ : 包含 istioctl 命令
  + istio.VERSION : 配置文件
3. **安装CRDS(istio 的 Custom Resource Definition)**
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
4. **安装选项：**
  + ***安装选项1: Install Istio without mutual TLS authentication between sidecars***  
kubectl apply -f install/kubernetes/istio-demo.yaml
  + ***安装选项2: Install Istio with default mutual TLS authentication***  
kubectl apply -f install/kubernetes/istio-demo-auth.yaml
  + ***安装选项3: Render Kubernetes manifest with Helm and deploy with kubectl***   
//首先 Render Istio’s core components to a Kubernetes manifest called istio.yaml:   
helm template install/kubernetes/helm/istio --name istio --namespace istio-system > $HOME/istio.yaml   
//然后 Install the components via the manifest:   
kubectl create namespace istio-system   
kubectl create -f $HOME/istio.yaml
  + ***安装选项4: Use Helm and Tiller to manage the Istio deployment***  
//如果还没有为 Tiller 安装 service Account, 先安装一个:  
kubectl create -f install/kubernetes/helm/helm-service-account.yaml   
//使用 service Account 安装 Install Tiller 到你的集群:   
helm init --service-account tiller  
//安装 Istio:  
helm install install/kubernetes/helm/istio --name istio --namespace istio-system   
5. **安装完成后后验证**  
//确保如下的 kubernetes 服务被部署: istio-pilot, istio-ingressgateway, istio-policy, istio-telemetry, prometheus, istio-galley , 还有 istio-sidecar-injector(如果已安装了的话)  
kubectl get svc -n istio-system  
//确保相应的 pods 被部署 并且所有的 containers 都启动并且运行: istio-pilot-*, istio-ingressgateway-*, istio-egressgateway-*, istio-policy-*, istio-telemetry-*, istio-citadel-*, prometheus-*, istio-galley-*, 还有 istio-sidecar-injector-* (如果已安装了的话)  
kubectl get pods -n istio-system


## Istio部署微服务应用程序 ##
***envoy containers***是服务网格的基础设施。  
***istio-sidecar-injector**负责自动将***envoy containers***注入到微服务应用的Pod中。  
如果启用了***istio-sidecar-injector***，可以直接使用***kubectl apply***部署微服务应用，***istio-sidecar-injector***会自动注入***envoy containers***到微服务应用的Pod中，***kubectl apply***部署微服务应用到相应的***namespace***中，应确保***namespace***命名空间具有标签***istio-injection=enabled***  
***kubectl label namespace <namespace> istio-injection=enabled***  
***kubectl create -n <namespace> -f <your-app-spec>.yaml***
如果没有安装***istio-sidecar-injector***，必须使用***istio kube-inject***手动将***envoy containers***注入到微服务应用的Pod中。













