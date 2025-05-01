What is ResourceQuota?
A ResourceQuota in Kubernetes is a policy tool used to limit the aggregate resource consumption (like CPU, memory, storage, etc.) per namespace. This ensures fair resource sharing and prevents a single team or application from consuming excessive cluster resources.

Examples of resources that can be limited using resource quotas include :-
* Pod memory and CPU limits.
* Pod memory and CPU requests.
* Storage requests for Persistent Volume Claims.
* Object count (such as the count of Deployments).



## COMMANDS :-

1) To view quota in a namespace : kubectl get resourcequota -n <namespace-name>

2) To describe quota details : kubectl describe resourcequota <quota-name> -n <namespace-name>

3) To apply quota : kubectl apply -f quota.yaml

4) To monitor quota usage : kubectl describe quota <quota-name> -n <namespace-name>
