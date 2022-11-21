# Deploy MetalLB on Nutanix

You may aware that Kubernetes does not offer network load balancer for bare metal cluster. If you take consideration of various cloud platform, they all call the load balancer and create one for you. (AWS creates Classic Load Balancer, GCP creates Network LoadBalancer …). MetalLB helps deploying a Kubernetes service of type Load balancer and it automatically traffic for you, using pool of address you configure. Without using the Load Balancer external IP of the service remain in the “Pending” state indefinitely.

### Here are the steps to install the MetalLB using Manifest

A.	You may need to apply manifest to install the MetalLB -
    
    kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.7/config/manifests/metallb-native.yaml

Above manifest file will deploy the metallb-system namespace to kubernetes cluster. Below mentioned components will be configured -

-	Deployment - metallb-system/controller
-	Daemonset -  metallb-system/speaker 
-	Service accounts with the RBAC permissions that the components need to function.

B.	If you need to assign the IP address, you may apply using. You may need to assign the same range as your worker nodes frontend network.

    apiVersion: metallb.io/v1beta1
    kind: IPAddressPool
    metadata:
    name: first-pool
    namespace: metallb-system
    spec:
    addresses:
    - 192.168.10.0/24
    - 192.168.9.1-192.168.9.5
