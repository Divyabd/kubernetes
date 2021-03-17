# kubernetes
links:https://minikube.sigs.k8s.io/docs/start/

2Start your cluster
From a terminal with administrator access (but not logged in as root), run:

minikube start
If minikube fails to start, see the drivers page for help setting up a compatible container or virtual-machine manager.

3Interact with your cluster
If you already have kubectl installed, you can now use it to access your shiny new cluster:

kubectl get po -A
Alternatively, minikube can download the appropriate version of kubectl, if you don’t mind the double-dashes in the command-line:

minikube kubectl -- get po -A
Initially, some services such as the storage-provisioner, may not yet be in a Running state. This is a normal condition during cluster bring-up, and will resolve itself momentarily. For additional insight into your cluster state, minikube bundles the Kubernetes Dashboard, allowing you to get easily acclimated to your new environment:

minikube dashboard
4Deploy applications
Create a sample deployment and expose it on port 8080:

kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
It may take a moment, but your deployment will soon show up when you run:

kubectl get services hello-minikube
The easiest way to access this service is to let minikube launch a web browser for you:

minikube service hello-minikube
Alternatively, use kubectl to forward the port:

kubectl port-forward service/hello-minikube 7080:8080
Tada! Your application is now available at http://localhost:7080/

LoadBalancer deployments
To access a LoadBalancer deployment, use the “minikube tunnel” command. Here is an example deployment:

kubectl create deployment balanced --image=k8s.gcr.io/echoserver:1.4  
kubectl expose deployment balanced --type=LoadBalancer --port=8080
In another window, start the tunnel to create a routable IP for the ‘balanced’ deployment:

minikube tunnel
To find the routable IP, run this command and examine the EXTERNAL-IP column:

kubectl get services balanced
Your deployment is now available at <EXTERNAL-IP>:8080

5Manage your cluster
Pause Kubernetes without impacting deployed applications:

minikube pause
Halt the cluster:

minikube stop
Increase the default memory limit (requires a restart):

minikube config set memory 16384
Browse the catalog of easily installed Kubernetes services:

minikube addons list
Create a second cluster running an older Kubernetes release:

minikube start -p aged --kubernetes-version=v1.16.1
Delete all of the minikube clusters:

minikube delete --all
