# To create a new Pod (or any other resource) using an exsiting resource  
```bash
kubectl get pod webapp -o yaml > my-new-pod.yaml
```  
You might have to delete few things.  

Then make the changes to the exported file using an editor (vi editor). Save the changes

# To list all container names in a POD  
```bash
kubectl get pods POD_NAME -o jsonpath="{.spec.containers[*].name}"
```  
# To list only POD names in a namesapce  
```bash
kubectl get pods -n NAMESPACE --no-headers -o custom-columns="POD NAME:.metadata.name"
```  

# To view all clusters available  
```bash
kubectl config get-clusters 

# For more details  
kubectl config view
```  

# To view permissions of a user on pods  
```bash
k get pods --as user-name #--as-group group-name
```  

# Create a role binding for user1, user2, and group1 using the admin cluster role
```bash
kubectl create rolebinding admin --clusterrole=admin --user=user1 --user=user2 --group=group1 -n default
```
  
# Create a role binding for serviceaccount monitoring:sa-dev using the admin role
```bash
kubectl create rolebinding admin-binding --role=admin --serviceaccount=monitoring:sa-dev -n default
```

# Create a role named "pod-reader" that allows user to perform "get", "watch" and "list" on pods
```bash
kubectl create role pod-reader --verb=get --verb=list --verb=watch --resource=pods
```
  
# Create a role named "pod-reader" with ResourceName specified
```bash
kubectl create role pod-reader --verb=get --resource=pods --resource-name=readablepod --resource-name=anotherpod
```
  
# Create a role named "foo" with API Group specified
```bash
kubectl create role foo --verb=get,list,watch --resource=rs.apps
```
  
# Create a role named "foo" with SubResource specified
```bash
kubectl create role foo --verb=get,list,watch --resource=pods,pods/status
```  

# To update service account in existing Deployment  
```bash
kubectl set serviceaccount deployment/my-deployment my-service-account
```  

# To get the service account of a pod  
```bash
kubectl get pod my-pod -o jsonpath='{.spec.serviceAccountName}'
```  

# check logs from a container  
```bash
kubectl exec mycontainer -- cat /mylogpath/mylog.log
```  

# To get an ingress resource 
```bash
kubectl get ingress my-ingress -o yaml -n my-namespace
```  

# To get context details without kubectl  
```bash
cat ~/.kube/config

#or
cat ~/.kube/config | grep -A5 "context"
```

# To check logs of a container in a pod  
```bash
kubectl logs my-pod -c my-container
```  

# To check logs of a container in a pod with timestamps  
```bash
kubectl logs my-pod -c my-container --timestamps
```  

# To check environment variables of a container in a pod    
```bash
kubectl exec my-pod -c my-container -- env
#or
kubectl exec my-pod -c my-container -- env | grep MY_ENV_VAR
```  
# To find the cluster CIDR (pod IP ranges) and the service CIDR, you can run the following commands:  
# For the service CIDR:  

```bash
kubectl cluster-info dump | grep -m 1 service-cluster-ip-range
#or
kubectl get svc kubernetes -o=jsonpath='{.spec.clusterIP}'

# or

kubectl describe kube-api-server | grep service-cluster-ip-range
# Kube api server name might be different. Find the exact name using the below cmd
kubectl get pods -n kube-system | grep kube-apiserver

```

# For the pod CIDR (cluster CIDR):
```bash
kubectl cluster-info dump | grep -m 1 cluster-cidr
```  
**Note**: `-m 1` is used to stop the search after the first match.  

# To check if a service account have the required permissions  
```bash
kubectl auth can-i create pods --as=system:serviceaccount:default:my-service-account
```

# To check if a user have the required permissions  
```bash
kubectl auth can-i create pods --as=user-name
```  

