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
