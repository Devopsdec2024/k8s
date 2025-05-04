Volumes
Kubernetes volumes provide a way for containers in a pod to access and share data via the filesystem. Kubernetes supports many types of volumes. A Pod can use any number of volume types simultaneously. Ephemeral volume types have a lifetime linked to a specific Pod, but persistent volumes exist beyond the lifetime of any individual pod. When a pod ceases to exist, Kubernetes destroys ephemeral volumes; however, Kubernetes does not destroy persistent volumes. For any kind of volume in a given pod, data is preserved across container restarts.

Types of volumes
1. hostPath: A hostPath volume mounts a file or directory from the host node's filesystem into your Pod. Using the hostPath volume type presents many security risks.

2. persistentVolumeClaim: A PersistentVolumeClaim (PVC) is a request for storage by a user. Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany, ReadWriteMany, or ReadWriteOncePod, see AccessModes). While PersistentVolumeClaims allow a user to consume abstract storage resources, it is common that users need PersistentVolumes.	

3. persistentVolume: A PersistentVolume (PV) is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV.

4. configMap:  A ConfigMap provides a way to inject configuration data into pods. The data stored in a ConfigMap can be referenced in a volume of type configMap and then consumed by containerized applications running in a pod. When referencing a ConfigMap, you provide the name of the ConfigMap in the volume.

5. secrets: A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Because Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods. 

Storage class
A StorageClass in Kubernetes defines how storage is provisioned dynamically. It acts like a blueprint for creating PersistentVolumes (PVs) on-demand, usually by interacting with a cloud provider or external storage backend.

Dynamic provision
In Kubernetes, one of the key features for handling persistent storage is dynamic provisioning, where the PersistentVolume (PV) is automatically created when a PersistentVolumeClaim (PVC) is made. 
EBS CSI Driver: With dynamic provisioning, Kubernetes can create and delete EBS volumes on demand as pods are scheduled and scaled, without needing to manually create or attach volumes.
EFS CSI Driver: For EFS, dynamic provisioning means that Kubernetes can automatically create an EFS file system (or mount target) and attach it to the cluster as required, without manual intervention.

Dynamic provisioning benefits:
Automation: No need to pre-create PVs.
Scalability: Volumes are automatically provisioned when required by the cluster.
Cost-Efficiency: Storage is only provisioned as needed, reducing wasted capacity.

EBS CSI driver
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm repo update

helm upgrade --install aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver \
  --namespace kube-system \
  --set controller.serviceAccount.create=false \
  --set controller.serviceAccount.name=ebs-csi-controller-sa

EFS CSI drive
helm repo add aws-efs-csi-driver https://kubernetes-sigs.github.io/aws-efs-csi-driver/
helm repo update

helm upgrade --install aws-efs-csi-driver aws-efs-csi-driver/aws-efs-csi-driver \
  --namespace kube-system



