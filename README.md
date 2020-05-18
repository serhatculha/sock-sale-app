

# sock-sale-app

  

# What has been done?

 -   Argo CD is installed to EKS Cluster's argocd namespace
    
    ```
      kubectl create namespace argocd
      kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
      kubectl create clusterrolebinding test-cluster-admin-binding --clusterrole=cluster-admin --user=myemail@gmail.com
    ```
    
-   Service type LB :
    ```
      kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    ```
    
-   Got the Argo CD endpoint :
    
    ```
      kubectl get svc -n default --> get LB endpoint
    ```
    
-   Got admin user password:
    
    ```
      kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2
    ```
    
-   Argo CLI installed :
    
    ```
      curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/$VERSION/argocd-darwin-amd64
      chmod +x /usr/local/bin/argocd
    ```
    
-   Logged in with CLI and changed password:
    
    ```
      argocd login <LB_ADDRESS>
      argocd account update-password
    ```
    
-   Argocd permissions updated for admin rights in default namespace:
    
    ```
      kubectl apply -f permissions.yaml
    ```
    
-   sock-sale-app chart created:
    
    ```
      helm create sock-sale-app
    ```
    
-   Ingress controller added to requirements as a dependency.
    
-   Service and Deployments definitions are created for catalog and frontend service
    
-   Create ingress resource with basic auth annotations by following : [https://kubernetes.github.io/ingress-nginx/examples/auth/basic/](https://kubernetes.github.io/ingress-nginx/examples/auth/basic/)
    
-   Changes committed, logged in to Argo CD and the application on Argo CD under default namespace has been created.
    
-   The application has been synced and starting pods were checked from both cli(kubectl get svc -n default) and Argo CD console
    
-   The LB endpoint of ingress is taken and website is reachable by browser.