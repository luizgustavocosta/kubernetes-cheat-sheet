# kubernetes-cheat-sheet
Kubernetes cheat sheet


## Add dashboard
[Complete article from @munza](https://medium.com/@munza/local-kubernetes-with-kind-helm-dashboard-41152e4b3b3d)
1.Start Docker
2.List all clusters "kind get clusters"
3.Delete all clusters "kind delete clusters --all"
4.Create a new cluster "kind create cluster --name 16-bits"
5.What is Helm?
6.Install Helm "helm install dashboard kubernetes-dashboard/kubernetes-dashboard -n kubernetes-dashboard --create-namespace"
7.Start the Dashboard "kubectl proxy"
8.Access the Dashboard on http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:dashboard-kubernetes-dashboard:https/proxy/#/login
9.Create token
9.1. Create new file service-account.yaml "touch service-account.yaml"
9.2. Edit the file "vi service-account.yaml"
9.3. Deploy using "kubectl apply -f service-account.yaml"
9.4. Describe the service "kubectl describe serviceaccount admin-user -n kubernetes-dashboard"
9.5. See the content of token "kubectl describe secret admin-user-token-5dlgd -n kubernetes-dashboard" The value admin-user-token-5dlgd could change in your environment
9.6. Copy the token to screen on step 8 and hit the button sign in.
10. That's it! A dashboard to see the namespaces and more


Details

// Create a new cluster
cat <<EOF | kind create cluster --name=16-bits --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
EOF
--
kind get clusters // created clusters
--
curl localhost //curl: (52) Empty reply from server
-- Install ingress
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml

or 
kubectl apply -f ingress.yaml
--
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
  // Wait ingress
--
curl localhost //curl: 
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx</center>
</body>
</html>
--
kubectl create namespace 16-bits
--
kubectl get namespaces
--
Generate application yml
mvn clean verify
Change the image name 
Change the imagePullPolicy: to IfNotPresent

Add the ingress for this application
---
Deploy
kubectl -n 16-bits apply -f /Users/luizcosta/workspace/git/16-bits-quarkus/service-4-k8s/target/kubernetes/kubernetes.yml

--Check the logs
1. Get pods
kubectl get pods -n <namespace>
kubectl get pods -n 16-bits
2. Check the log for specific pod
kubectl logs <pod-name> -n <namespace>
kubectl logs service-4-k8s-5656b487bf-2bw6q -n 16-bits
--
Curl
curl localhost/hello-resteasy //Hello RESTEasy from service-4-k8s-5656b487bf-2bw6q/10.244.0.10

---
Install dashboard
From github

--
References
https://kind.sigs.k8s.io/docs/user/ingress/
