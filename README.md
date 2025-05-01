What is Namespace ?
Namespace helps in creating a virtual cluster inside your Kubernetes cluster.
Namespace helps you isolate your resources between different teams. Allocate resources or apply quota of resources.

Advantages of namespace :-
* Helps in easy management of applications of each environment
* Control multiple app with single name in a single cluster
* Cost effective

Default k8s namespaces are :-
* Default : adding an object to a cluster without providing a namespace will place it within the default namespace
* Kube-system : kube system namespace is used for k8s components managed by k8s
* Kube-public : kube public it is readable by everyone, but the namespace is reserved for system usage



## COMMANDS :-
1) To create New Namespaces : kubectl create namespace <your-namespace>

2) To view Namespaces in k8 : kubectl get namespaces

3) To describe a Namespace  : kubectl describe namespace <my-namespace>

4) To list Pods in Specific Namespace : kubectl get pods --namespace=<my-namespace>

5) To label the Namespace : kubectl label namespaces <namespace> <labelKey>=<value>

6) To delete a Namespace : kubectl delete namespace <namespace-name>

7) To set default Namespace for Current Context : kubectl config set-context --current --namespace=<namespace-name>

8) To run a command is specific Namespace : kubectl get pods -n <namespace-name>




