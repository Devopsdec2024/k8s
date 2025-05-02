Services

K8s service type allows you to specify what kind of service you want, the default is cluster IP.

Type values and their behaviours are:

ClusterIP:  A ClusterIP is the default service type in Kubernetes. It exposes the service only within the cluster, meaning it can be accessed by other pods or services internally but not from outside the cluster.

NodePort: A NodePort is a type of Kubernetes service that exposes a pod to external traffic by assigning a static port on each node's IP.

LoadBalancer: A LoadBalancer is a Kubernetes service type that provisions an external IP address through your cloud provider (e.g., AWS, GCP, Azure) to expose a service outside the cluster. It automatically configures a cloud load balancer and routes traffic to your pods.

Node port and cluster IP services, to which the external load balancer routes or automatically created.

ExternalName: Maps a Kubernetes service to an external DNS name.

ExternalIPs: Forwards traffic from a manually assigned IP address to a service inside the cluster.

