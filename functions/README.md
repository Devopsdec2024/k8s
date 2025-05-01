Taints
A taint is applied to a node to indicate that it should not accept any pods that do not explicitly tolerate the taint. This mechanism allows nodes to repel certain pods.

Taint Structure:
Key: An arbitrary string.
Value: An arbitrary string.
Effect: Determines the behavior for pods that do not tolerate the taint.

Effects:
NoSchedule: Pods that do not tolerate the taint will not be scheduled on the node.
PreferNoSchedule: The system will try to avoid placing pods that do not tolerate the taint on the node, but it is not guaranteed.
NoExecute: Pods that do not tolerate the taint will be evicted from the node if they are already running, and new pods will not be scheduled on the node.

Applying taint:
kubectl taint nodes <node-name> key=value:effect
Ex- kubectl taint nodes node1 dedicated=database:NoSchedule

What Are Tolerations?
A toleration is applied to a pod to indicate that it can be scheduled on nodes with matching taints. Tolerations allow pods to "tolerate" a node's taint.

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "database"
    effect: "NoSchedule"
	
	
______________________________________________________

Labels:
In Kubernetes, labels are key/value pairs attached to objects such as Pods, Nodes, and Services. They serve as a mechanism to organize, select, and manage subsets of objects within a cluster.

metadata:
  labels:
    disktype: ssd 
	environment: production


The nodeSelector field in a pod specification allows you to constrain the pod to run only on nodes with specific labels. It's a straightforward method to control pod placement.

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx
  nodeSelector:
    disktype: ssd
    environment: production

______________________________________________________

Node Affinity:
Kubernetes offers node affinity, which provides greater flexibility than nodeSelector. Node affinity allows you to specify rules such as:
- Schedule pods on nodes with labels matching a set of values.
- Prefer (but not require) scheduling on nodes with certain labels.

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: disktype
                operator: In
                values:
                  - ssd
                  - nvme
  containers:
    - name: nginx
      image: nginx
______________________________________________________

Pod Affinity & AntiAffinity
Node affinity allows you to schedule a pod on a set of nodes based on labels present on the nodes. However, in certain scenarios, we might want to schedule certain pods together or we might want to make sure that certain pods are never scheduled together. This can be achieved by PodAffinity and/or PodAntiAffinity respectively.
Similar to node affinity, there are a couple of variants in pod affinity namely requiredDuringSchedulingIgnoredDuringExecution and preferredDuringSchedulingIgnoredDuringExecution.

Affinity:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend
        image: frontend:latest
      affinity:
        podAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchLabels:
                app: backend
            topologyKey: kubernetes.io/hostname



AntiAffinity:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: cache
spec:
  replicas: 3
  selector:
    matchLabels:
      app: cache
  template:
    metadata:
      labels:
        app: cache
    spec:
      containers:
      - name: cache
        image: cache:latest
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchLabels:
                app: cache
            topologyKey: kubernetes.io/hostname

______________________________________________________

Persistent Volumes (PVs) and Persistent Volume Claims (PVCs)
For data that needs to persist beyond the Pod's lifecycle, Kubernetes provides persistent storage through PVs and PVCs.
Persistent Volume (PV): A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using StorageClasses. PVs are independent of Pods and exist beyond their lifecycle. ​
Persistent Volume Claim (PVC): A user's request for storage, specifying size, access modes, and storage class. Kubernetes matches the PVC to an available PV that meets the criteria. 

apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: my-volume
  volumes:
  - name: my-volume
    persistentVolumeClaim:
      claimName: my-pvc

______________________________________________________

Probes:
Probes are critical for managing the lifecycle and health of containers within Pods. They help determine when containers should be restarted, when they are ready to accept traffic, and when they have started successfully.

Liveness Probe
The liveness probe checks if a container is still running and healthy. If this probe fails, Kubernetes will restart the container to attempt recovery

livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

Readiness Probe
The readiness probe determines when a container is ready to start accepting traffic. If this probe fails, the container is removed from the service's endpoints, preventing traffic from reaching it.

readinessProbe:
  httpGet:
    path: /readiness
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5


Startup Probe
The startup probe is used to determine if an application within a container has started successfully. If configured, it disables the liveness and readiness probes until it succeeds, allowing for longer initialization times without triggering premature restarts.

startupProbe:
  httpGet:
    path: /startup
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
