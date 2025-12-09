*Disadvantage of docker container.*
1.	Single host – as it has single host one container is impacting other containers, no way that container can come up.
2.	Lack of Auto-Healing
      a. If a container is terminated unexpectedly, the application becomes inaccessible until someone manually restarts the container.
  	  b. There can be hundreds of reasons for a container to fail, but expecting a developer to manually intervene each time is not practical in large-scale environments.
4.	No Auto scaling
a.	Docker doesn’t provide built-in auto-scaling.
b.	For example, if an application usually handles 10,000 users but suddenly spikes to 100,000 (like during a major event or movie release), containers must be scaled manually/automatically, (both manual and automatic is not supported)
c.	Load balancing: if there are 2 containers and we cannot give IP address to users, under wood LB take care of this and also load is distributed equally among the containers.
5.	Docker doesn’t provide any enterprise level support:
a.	Docker is a simple platform, doesn’t provide features like  load balancer, firewall, auto scaling, healing, API gateways, blacklisting, whitelisting .

*Above Issues are solved by Kubernetes:*
1.	By default Kubernetes is cluster(groups of nodes). Kubernetes is installed on master  node architecture.
a.	If you installed Kubernetes and have 2 nodes, if c1  is impacting C99 in 1 node then Kubernetes places the c99 in another node. A faulty application in node which is impacting the other application, as Kubernetes have multi node architecture puts the faulty application into other node
2.	Auto scaling: 
a.	Kubernetes have replica set, if there is increase in application load, in replicatasetcontroller.yaml file -mention increase replicas from  1 to 10 – manual way.
b.	Also support Horizontal pod auto-scaler- - when container is receiving load of 80%, the spin up the container – automatic way
3.	Auto healing - Kubernetes controls and fix the damage. 
a.	Kubernetes has feature of auto healing, when the container goes down or even before the container goes down, Kubernetes creates another container.
4.	Provides enterprise level support.

*K8s Architecture*
Control plane: is the one which is controlling the actions
Data plane: is the one which is executing the actions.
kubelet, kube-proxy, container runtime
Pod – Pod is wrapper over container with some advanced capabilities.

Kubelet :
•	Kubelet is responsible for running your pod. 
•	It checks whether pod is running or not. If not, informs one of the components in Kubernetes, so that Kubernetes can auto-heal.
Container runtime: 
•	Actually, running your container. which provides execution environment for your container.
•	Pod will have container in it, to run container we need container runtime.
•	In Kubernetes docker is not mandatory. Doker only have containerd. 
•	Container runtime in k8s: Docker shim, containerD, crio(Kubernetes will allows you to use if they implement container interface with the standard Kubernetes has set)
Kube-proxy : 
•	Provides networking like IP address and default LB capalibities.
•	Example: if you have 2 replica to pod, then kube-proxy tells send 50% load to one  and 50% load to other replica set.


Control Plane:
•	API server:
o	API server is the heart and core component in Kubernetes which deals with instruction coming from user.
•	Scheduler: 
o	Scheduler is responsible for scheduling pod or resources in Kubernetes.
o	schedules the resource, whether it has to go to WN1 or WN2 decision is taken take AI server and it is implemented by scheduler.
•	Etcd:
o	It is backing store for your entire kubernentes cluster.
o	Entire cluster information is stored as object/key-value pair inside etcd. Without etcd no cluster information can be restored.
•	Controller manager:
o	It is responsible for managing controllers — each controller watches specific Kubernetes resources (like Pods, Nodes, Deployments, etc.) and tries to ensure the actual cluster state matches the desired state defined in YAML files
•	Cloud controller manager (CCM):
o	allows Kubernetes to interact with cloud provider APIs.
o	So user request to create a load balancer or a storage, that request is sent to Kubernetes. Kubernetes has to understand underlying cloud provider to create a load balancer. So Kubernetes needs to translate the request from the user to the API request that your cloud provider understands. So this mechanism has been implemented in CCM. 
o	CCM is an open source utility in GitHub. If you have your own cloud provider, write a logic for your cloud provider inside the CCM, pull the request, add a logic, submit to CCM.
o	 So for on-prem, CCM is not required.
