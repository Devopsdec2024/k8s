DAEMON SET:
A DaemonSet defines pods that provide node-local facilities. A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. 
As nodes are removed from the cluster, those Pods are garbage collected. 
Deleting a DaemonSet will clean up the Pods it created.


Some typical uses of a DaemonSet are:
1.running a cluster storage daemon on every node.
2.running a logs collection daemon on every node.
3.running a node monitoring daemon on every node.


Required Fields in yaml file: A DaemonSet needs apiVersion, kind, metadata and Pod template.


Pod Template:
The .spec.template is a pod template. It has exactly the same schema as a Pod, except it is nested and does not have an apiVersion or kind.
A Pod Template in a DaemonSet must have a RestartPolicy equal to Always, or be unspecified, which defaults to Always.

