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
      eksctl create cluster --name cluster2 --region us-east-1 --nodegroup-name ng1 --node-type t2.medium --nodes 2
 
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






