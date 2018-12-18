# Istio Book 微服务架构学习 #

## Service Mesh 服务网络 ##
Service Mesh是专用的基础设施层，轻量级高性能网络代理，提供安全地、快速地、可靠地服务间通信，与实际的应用部署在一起，但对应用透明。应用作为服务的发起方，只需要简单的方式将请求发送给本地的网络代理，然后网络代理会进行后续的操作，如服务发现，负载均衡，最后将请求转发给目标服务就。
![Service Mesh架构图](https://github.com/chijinxina/IstioBook/blob/master/doc/service_mesh.png)
## Istio ##
Istio 首先是一个服务网络，但是Istio又不仅仅是服务网格: 在 Linkerd,Envoy 这样的典型服务网格之上，Istio提供了一个完整的解决方案，为整个服务网格提供行为洞察和操作控制，以满足微服务应用程序的多样化需求。Istio在服务网络中统一提供了许多关键功能：
![Istio架构图](https://github.com/chijinxina/IstioBook/blob/master/doc/istio.png)
+ 流量管理：控制服务之间的流量和API调用的流向，使得调用更可靠，并使网络在恶劣情况下更加健壮。
+ 可观察性：了解服务之间的依赖关系，以及它们之间流量的本质和流向，从而提供快速识别问题的能力。
+ 策略执行：将组织策略应用于服务之间的互动，确保访问策略得以执行，资源在消费者之间良好分配。策略的更改是通过配置网格而不是修改应用程序代码。
+ 服务身份和安全：为网格中的服务提供可验证身份，并提供保护服务流量的能力，使其可以在不同可信度的网络上流转。
+ 平台支持：Istio旨在在各种环境中运行，包括跨云， 预置，Kubernetes，Mesos等。最初专注于Kubernetes，但很快将支持其他环境。
+ 集成和定制：策略执行组件可以扩展和定制，以便与现有的ACL，日志，监控，配额，审核等解决方案集成。



