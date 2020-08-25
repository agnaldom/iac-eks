# Terraform AWS EKS

## Terraform 
Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers as well as custom in-house solutions.

## About this project
A terraform to create a managed Kubernetes cluster on AWS EKS. Elastic Kubernetes (EKS) cluster and associated worker instances on AWS

## Resources
* VPC for isolation
* IAM roles for authentication
* Security groups
* An internet gateway
* Subnets
* Elastic Load Balancing for load distribution
* Autoscaling group
* Route table
* EKS cluster
* Your kubectl configuration

## Amazon EKS Clusters
An Amazon EKS cluster consists of two primary components:

* Amazon EKS control plane
* Amazon EKS worker nodes registered with the control plane

### EKS CLuster 

* EKS Control Plane 
  
Contains Kubernetes Master components like etcd, kube-apiserver, kube-controller. 
It's managed service by AWS 

* Worker Nodes & Node Groups 

Group of EC2 instaces where we run our Applications worloads 

* Forgate Profiles (Serveless)

Instead of EC2 our Application worloads on Serverless Fargate profiles 

* VPC 

Follow secure network standards which will allow us to run production worloads on EKS.

## EKS Cluster -  Core Objects Detailed

### EKS Control Plane 
1. EKS runs a single tenant Kubernetes control plane for each cluster, and control
  plane infrastructure is not shared acrros cluster or AWS accounts.
2. This control plane consists of at least two API server nodes and three etcd nodes 
   that run across three Availability Zones within a Region.
3. EKS automatically detects and replaces unhealthy control plane instances, restarting
   them across the Availability Zones within the Region as needed.

### Worker Nodes & Node Groups 
1. Worker machines in Kubernetes are called nodes. These are EC2 instances
2. EKS Worker nodes run in our AWS account and connect to our cluster's control plane 
   via the cluster API server endpoint.
3. A node group is one or more EC2 instances that are deployed in an EC2 Autoscaling group.
4. All instances in a node group must
   1. Be the sma instance type 
   2. Be running the same AMI 
   3. Use the same EKS worker node IAM role 

## worker nodes

Worker machines in Kubernetes are called nodes. Amazon EKS worker nodes run in your AWS 
account to your cluster's control plane via the cluster API server endpoint. You deploy
onde or more worker nodes into a node group. A node group is one or more AmazonEC2 
instances that are deployed in an Amazon EC2 Auto Scaling group. 

### Forgate Profiles 
1. AWS Forgate is a technology that provide on-demand, right-sized computer capacity
   for containers
2. With Fargate, we no longer have to provision, configure, or scale groups of virtual 
   machines to run containers.
3. Each pod running on Fargete has its own isolation boundary and does not share the underlying
   kernel, CPU resources, memory resources, or elastic network interface with another pod.
4. AWS specially built Fargate controllers that recognizes the pods belonging to fargate and 
   schedules them on Farget profiles.
5. We will see more in our Farget learning section.
   
### Amazon EKS-Optimized Linux AMI
### Amazon EKS-Optimized Windows AMI 

## AWS Fargate

AWS Fargate is a technology that provides on-demand, right-sized computer capacity for
containers. With AWS Fargete, you no longer have to provision, configure, or scale groups
of virtual machines to run containers. This removes the need to choose server types, decide
when to scale your node groups, or optimize cluster packing.

You can control which pods start on Fargate and how they run with Fargate profiles, 
which are defined as part of your Amazon EKS cluster. 

### AWS Fargate Considerations

Here's some things to consider about using Fargate on Amazon EKS.

  * Classic Load Balancers and Network Load Balancers are not supported on pods running on Fargate. 

### VPC 
1. EKS uses AWS VPC network policies to restrict traffic between control plane components
   to within a single cluster.
2. Control plane components for a EKS cluster cannot view or receive communication from other
   clusters or other AWS accounts, except as authorized with Kubernetes RBAC policies.
3. This secure and highly-availability configuration makes EKS reliabla and recommended for 
   production workloads.

## Kubernetes Architeture - Master

* Kube-apiserver
  * It acts as front end for the Kubernetes control plane. It exposes the Kubernetes API 
  * CLItools (like kubectl), Users and even Master components (scheduler, controller manager, etcd)
  and Worker node components like (kubelet) everything talk with API Server.
* etcd 
  * Consistent and highly-available key value store used as Kubernetes backing store for all cluster data.
  * It stores all the masters and worker node information.
* kube-scheduler
  * scheduler is responsible for distributing containers across multiple nodes.
  * It watches for newly created Pods with no assigned node, and selects a node for them to run on.
* kube-controller-manager
  * Controllers are responsible for noticing and responding when nodes, containers or endpoints
  go down. They make decisions to bring up new containers in such cases.
  * Node Controller: Responsible for noticing and responding when nodes go down.
  * Replication Controller: Responsible for maintaining the correct number of pods for every Replication
  controller obejct in the system. 
  * Endpoints Controller: Populates the Endpoints object (that is, joins Services & Pods)
  * Service Account & Token Controller: Creates default accounts and API access for new namespaces.
* cloud-controller-manager
  * A Kubernetes control plane component that embeds cloud-specific control logic
  * It only runs controllers that are specific to your cloud provider.
  * On-Premise Kubernetes clusters will not have this component.
  * Node controller: For checking the cloud provider to determine if a node has been 
  deleted in the cloud after it stops responding.
  * Route controller: For setting up routes in the underlying cloud infrastructure
  * Service controller: Fro creating, updating and deleting cloud provider load balancer.

## Kubernetes Architeture - Worker Nodes

* Container Runtime
  * Container Runtime is the underlying software where we run all these Kubernetes components.
  * We are using Docker, but we have other runtime options like rkt, container-d etc.
* Kubelet 
  * Kubelet is the agent that runs on every node in the cluster
  * This agent is responsible for making sure that containers are running in a Pod on a Node.
* Kube-Proxy
  * It is a network proxy that runs on each node in your cluster.
  * It maintains network rules on nodes 
  * In short, these network rules allow network communication to your Pods from network
  sessions inside or outside of your cluster.

## Kubernetes - Fundamentals

* Pod 
  A POD is a single instance of an Application.
  A POD is the smallest object, that you can create in Kubernetes.
* ReplicaSet 
  * A ReplicaSet will maintain a stable set of replica Pods running at any given time.
  * In Short, it is often used to guarantee the availability of a specified number of identical Pods.
* Deployment
  * A Deployment run multiple replicas of your application and automatically replaces any instances 
  that fail or become unresponsive. 
  * Rollout & rollback changes to applications. Deployments are well-suited for stateless applications.
* Service 
  * A service is an sbstraction for pods, providing a stable, so called virtual IP(VIP) address.
  * in simple terms, service sits Infront of a POD and acts as a load balancer.

## Contributing

Report issues/questions/feature requests on in the [issues](https://github.com/agnaldom/terraform-eks/issues/new) section.

Full contributing [guidelines are covered here](https://github.com/agnaldom/terraform-eks/blob/master/.github/CONTRIBUTING.md).

## License

MIT Licensed. See [LICENSE](https://github.com/agnaldom/terraform-eks/tree/master/LICENSE) for full details.