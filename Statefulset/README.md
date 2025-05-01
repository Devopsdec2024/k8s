StateFulSet:
Statefulset is the workload api object used to manage stateful applications
Manages the deployment and scaling of a set of pods and provides guarantees about the ordering and uniqueness of these pods.
Like deployment, a statefulset manages pods that are based on an identical container spec. unlike deployment, a statefulset maintains a sticky identity for each of their pods. These pods are created from the same spec but are not interchangeable. Each has a persistent identifier that it maintains across any rescheduling. 


StatefulSets are valuable for applications that require one or more of the following.
1.Stable, unique network identifiers.
2.Stable, persistent storage.
3.Ordered, graceful deployment and scaling.
4.Ordered, automated rolling updates.

Limitations
The storage for a given Pod must either be provisioned by a PersistentVolume Provisioner (examples here) based on the requested storage class, or pre-provisioned by an admin.
Deleting and/or scaling a StatefulSet down will not delete the volumes associated with the StatefulSet. This is done to ensure data safety, which is generally more valuable than an automatic purge of all related StatefulSet resources.
StatefulSets currently require a Headless Service to be responsible for the network identity of the Pods. You are responsible for creating this Service.
StatefulSets do not provide any guarantees on the termination of pods when a StatefulSet is deleted. To achieve ordered and graceful termination of the pods in the StatefulSet, it is possible to scale the StatefulSet down to 0 prior to deletion.
When using Rolling Updates with the default Pod Management Policy (OrderedReady), it's possible to get into a broken state that requires manual intervention to repair.


Components of the given statefulset.yml:

1. Service (Headless Service)
   a) apiVersion: v1 – This service uses the core Kubernetes API.
   b) kind: Service – Defines a Kubernetes Service to enable network access to the pods.
   c) metadata.name: nginx – The name of the service, referenced by the StatefulSet.
   d) labels – Key-value pairs used for organization and selectors.
   e) spec.ports – The service exposes port 80, named web.
   f) clusterIP: None – This makes it a Headless Service, meaning no load balancing or cluster IP. Required for StatefulSets to give each pod a stable DNS name.
   g) selector – Targets pods with app: nginx label, allowing the service to route traffic to them.

2. StatefulSet
   a)kind: StatefulSet – Used to manage stateful applications where each pod needs a stable identity and persistent storage.
   b)metadata.name: web – Name of the StatefulSet.
   c)serviceName: "nginx" – Links to the Headless Service named nginx. Used to create stable DNS entries like web-0.nginx, web-1.nginx.
   d)replicas: 2 – Creates 2 pods (web-0 and web-1).
   e)selector.matchLabels – Ensures the StatefulSet manages only pods with app: nginx.
   f)template.metadata.labels – Assigns the app: nginx label to pods created by the StatefulSet.
   g)containers – Runs the NGINX slim image inside each pod.
   f)ports.containerPort: 80 – Exposes container port 80.
   h)volumeMounts – Mounts a volume named www to /usr/share/nginx/html (NGINX's default web root).

3. volumeClaimTemplates
   a)This ensures each pod gets its own PersistentVolumeClaim named www.
   b)ReadWriteOnce – Only one pod can mount the volume at a time (suitable for EBS).
   c)storageClassName: ebs-sc – Uses a custom StorageClass (e.g., AWS EBS).
   d)requests.storage: 1Gi – Each pod will request a 1Gi volume.
 

-->Pods are named web-0, web-1, maintaining order and identity.


Deploying Cassandra with a StatefulSet:
    https://kubernetes.io/docs/tutorials/stateful-application/cassandra/
