# eks-aws


EKS stands for Amazon Elastic Kubernetes Service. <br/>

It’s a managed Kubernetes service provided by AWS that makes it easy to run Kubernetes clusters on AWS without having to install, operate, and maintain your own Kubernetes control plane.  <br/>

Key points about EKS:  <br/>
Fully managed: AWS handles the Kubernetes control plane (API servers, etcd, etc.) so you don’t have to worry about the infrastructure that runs Kubernetes itself.  <br/>

Scalable and secure: Integrates with AWS networking, security (IAM, VPC, etc.), and scaling features.  <br/>

Supports standard Kubernetes: You can use all standard Kubernetes tools and APIs.  <br/>

Integrates with other AWS services: For example, you can use AWS Load Balancers, IAM Roles for service accounts, CloudWatch for monitoring, and more.  <br/>

Automates upgrades and patches for the control plane.  <br/>

Why use EKS?  <br/>
Saves operational effort managing Kubernetes.  <br/>

Runs Kubernetes workloads easily and reliably on AWS infrastructure.  <br/>

Good for teams wanting Kubernetes without managing masters/control plane. 
<br/>

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Configure EKS using eksctl command

  # steps 
  
  1) install eksctl,aws-cli,kubernetes-cli on your local machine.
  ```
      choco install eksctl
      choco install kubernetes-cli            (here choco is package management for windows) 
  ```

  2) check eksctl version
 
  ```
    eksctl version
  ```


3)Create the IAM user and add permissions  to the user like <br/>

![image](https://github.com/user-attachments/assets/46d3fb57-6be2-4e9a-a946-f46d60de7de7)


4) run command aws configure and provide region in it. <br/>

5) For create the cluster use following command: <br/>
   
   ```
      eksctl create cluster --name cluster2 --region us-east-1 --nodegroup-name ng1 --node-type t2.medium --nodes 5
 
    ```

What this does: <br/>
Creates a new EKS cluster named cluster2 in the us-east-1 region. <br/>

Adds a node group named ng1. <br/>

The node group consists of 2 nodes of instance type t2.medium. <br/>


Important tips: <br/>
Make sure your AWS CLI is configured with appropriate credentials (aws configure). <br/>

eksctl will create the control plane and worker nodes for you. <br/>

It might take some minutes for the cluster to be fully ready. <br/>

You can customize further with flags like --nodes-min, --nodes-max for autoscaling, or --managed for managed node groups. <br/>

**It creates the vpc, subnet,internet gateway,route table and nodes(instances).** 

6)  check the nodes within the cluster:

  ```
      kubectl get nodes -o wide 
  ```

7) List the nodegroup

   ```
     eksctl get nodegroup --cluster cluster2
   ```

9) create the pod
  ```
    kubectl run second --image=nginx

  ```

10) Expose the service through the nodeport

  ```
    kubectl expose pod pod --type=NodePort --port=80  
  ```
**Note to access the pods to the local machine must be provide port number t the node(instance) security group** <br/>
       - kubectl describe pod pod_name (describe the node of the pod) <br/>

11) Access the pod

```
      curl PUBLIC_IP_of_INSTANCE:port
```

  eg. curl 52.206.101.239:30394 <br/>



  # Outputs:

```
eksctl create cluster --name cluster2 --region us-east-1 --nodegroup-name ng1 --node-type t2.medium --nodes 5
```

After successfully running the above command eksctl creates the cluster, node-group, nodes,vpc,subnets for running the kubernetes application. <br/>

It creates the cluster with name cluster2   <br/>
![Screenshot 2025-06-17 193524](https://github.com/user-attachments/assets/6fc8ffa6-7b17-4ee1-81ce-631c82c5fb12)  <br/>

![Screenshot 2025-06-17 193737](https://github.com/user-attachments/assets/a94ca637-4fca-4e0f-86d7-727e53d7fb1a)

Node groups within the cluster:  <br/>

![Screenshot 2025-06-17 193720](https://github.com/user-attachments/assets/29542aa1-071f-4164-98fb-36982f8f820d)

VPC for the kubernet-cluster cluster2:  <br/>

![Screenshot 2025-06-17 193807](https://github.com/user-attachments/assets/a2a3dc11-3463-4246-9771-f774e9c45c84)  <br/>


Subnets for the cluster:  <br/>

![Screenshot 2025-06-17 193827](https://github.com/user-attachments/assets/304ec926-22ed-4a0a-b7cd-6fafb95a6dd5)  <br/>

Route table for the cluster:  <br/>

![Screenshot 2025-06-17 193845](https://github.com/user-attachments/assets/f08752c1-f73e-446d-88d9-fc281550d3b1)  <br/>

Internet Gateway-It Provide the internet access to the vpc:  <br/>

![Screenshot 2025-06-17 193900](https://github.com/user-attachments/assets/04f716b1-d184-439f-a394-fae5aea19317)  <br/>





