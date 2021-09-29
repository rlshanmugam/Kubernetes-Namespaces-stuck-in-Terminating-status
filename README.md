# Kubernetes-Namespaces-stuck-in-Terminating-status

**Issue:**
I tried to delete the namespaces in AKS using kubectl command 

$ kubectl delete namespaces cert-manager

Unfortunately, my namespace got stuck and was showing status as terminating.

![image](https://user-images.githubusercontent.com/44049960/135212420-dfc053f6-e063-4794-bbc8-e35b5ce953fa.png)


**Solution:**

Dump the descriptor as JSON to a file

$ kubectl get namespace cert-manager -o json > cert-manager.json


Open the json file and find finalizers array

![image](https://user-images.githubusercontent.com/44049960/135213204-56de8b3a-b177-43c1-b360-282126b9d248.png)


Remove kubernetes from the finalizers array
![image](https://user-images.githubusercontent.com/44049960/135213309-c78f83b5-b522-4a70-b8d3-f2eb4ffe2d41.png)


Executing cleanup command, we can instruct our cluster to get rid of that annoying namespace:

$ kubectl replace --raw "/api/v1/namespaces/cert-manager/finalize" -f ./cert-manager.json

You will get output like this,

![image](https://user-images.githubusercontent.com/44049960/135213734-69d1f440-3f56-4e02-8721-338c91d9db66.png)

Your annoying namespace should have been got deleted now. Please check using below command,

$ kubectl get namespaces

![image](https://user-images.githubusercontent.com/44049960/135213949-c38a97a2-0ce2-413a-8e47-230c8724a021.png)



I hope this solution works for you !!! 

