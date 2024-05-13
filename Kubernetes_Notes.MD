# Kubernetes
- Kubernetes is also known as K8s.
- It was developed by Google.
- It is now open source and one of the best and most popular container orchestration tool.
- To understand kubernetes, we need to understand Container + Orchestration.

## Containers:
- Specifically, we will look at the most popular container technology out there that is Docker.
- When instructor was working on the project, one of the biggest challenges he faced was the requirement to set up an end to end stack, including various different technologies like a web server using Node.js, a database such as MongoDB, a messaging system like Redis, and an orchestration tool like Ansible.
- He had a lot of issues developing this application with all these different components.
- First, the compatibility of the tools with the underlying operating system.
- He had to ensure that all these different services were compatible with the version of the operating system we were planning to use. - There have been times when certain version of these services were not compatible with the OS and we have had to go back and look for another OS that was compatible with all these different services.
- Secondly, we had to check the compatibility between these services and the libraries and dependencies on the OS.
- We've had issues where one service requires one version of a dependent library, whereas another service required another version.
- So, all of these made our life in developing, building and shipping the application really difficult.
- The answer for this is Docker.
- With Docker, I was able to run each component in a separate container with its own libraries and its own dependencies, all on the same VM and the OS, but within separate environments or containers.
- We just had to build the Docker configuration once and all. Our developers could now get started with a simple docker run command, irrespective of what the underlying operating system they run.
- All they needed to do was to make sure they had Docker installed on their systems.
### - So, what are the containers?
----------------------
- Containers are completely isolated environments. As in, they can have their own processes or services, their own networking interfaces, their own mounts, just like virtual machines, except they're all shared the same operating system kernel.
- “Docker containers share the underlying kernel.”
### - What does “Sharing the kernel” actually mean? 
---------------------------
Let's say we have a system with an ubuntu OS with Docker installed on it.
- Docker can run any flavor of OS on top of it, as long as they are all based on the same kernel, in this case Linux.
- If the underlying operating system is ubuntu, Docker can run a container based on another distribution like Debian, fedora, Suse or CentOS.
- Each Docker container only has the additional software that we just talked about in the previous slide that makes these operating systems different and Docker utilizes the underlying kernel of Docker host.
- And so you won't be able to run a windows based container on a Docker host with Linux OS on it.
- For that you would require Docker on a windows server.
- As you can see, each virtual machine has its own operating system inside it, then the dependencies and then the application.
- This overhead causes higher utilization of underlying resources, as there are multiple virtual operating systems and kernels, running the virtual machines also consume higher disk space, as each VM is heavy and is usually in gigabytes in size.
### - Whereas, Docker containers are lightweight and are usually in megabytes in size.
- This allows Docker containers to boot up faster, usually in a matter of seconds, whereas virtual machines, as we know, takes minutes to boot up as it needs to boot up the entire operating system.
- There are a lot of containerized versions of applications readily available as of today, so most organizations have their products containerized and available in a public Docker registry called Docker Hub or Docker Store already.
- Once you identify the images you need and you install Docker on your host, bringing up an application stack is as easy as running a docker run command with the name of the image.
- In this case, running a docker run ansible command will run an instance of Ansible on the Docker host. Similarly, run an instance of MongoDB, Redis, and NodeJS using the docker run command. When you run NodeJS, just point to the location of the code repository on the host.
- If you need to run multiple instances of the web service, simply add as many instances as you need and configure a load balancer of some kind in the front. In case one of the instances was to fail, simply destroy that instance and launch a new instance.
- An image is a package or a template, just like a VM template that you might have worked with in the virtualization world. It is used to create one or more containers.
- Containers are running instances of images that are isolated and have their own environments and set of processes. As we have seen before, a lot of products have been dockerized already.
- In case, you cannot find what you're looking for, you could create an image yourself and push it to the docker hub repository making it available for the public.

## Container Orchestration
- Suppose, we now have our application packaged into a Docker container.
- But what's next?
- How do you run it in production?
- What if your application relies on other containers such as databases or messaging services or other backend services?
- What if the number of users increase and you need to scale your application?
- How do you scale down when the load decreases?
- To enable these functionalities, you need an underlying platform with a set of resources and capabilities.
- The platform needs to orchestrate the connectivity between the containers and automatically scale-up or down on the load.
- This whole process of automatically deploying and managing containers is known as Container Orchestration.
- kubernetes is just a container orchestration technology.
- There are multiple such technologies available today.
#### - Docker has its own tool called Docker Swarm, Kubernetes from Google and MESOS from Apache.
- While Docker Swarm is really easy to set up and get started, it lacks some of the advanced features required for complex applications.
- MESOS on the other hand is quite difficult to set up and get started, but supports many advanced features.
 - Kubernetes arguably the most popular of it all, is a bit difficult to set up and get started as it provides a lot of options to customize deployments and supports deployment of complex architectures.
- Kubernetes is now supported on all public cloud service providers like GCP, Azure and AWS and Kubernetes project is one of the top ranked projects in Github.
### - There are various advantages of Container Orchestration:
1. Your application is now highly available as hardware failures do not bring your application down because we have multiple instances of your application running on different nodes.
2. The user traffic is load balanced across the various containers. When demand increases, deploy more instances of the applications seamlessly and within a matter of seconds.
--------------------------------
## Pod is the group of containers. Node is a machine on which pods are running.
3. We have the ability to do that at a service level when we run out of hardware resources, scale the number of underlying nodes up or down without having to take down the application and do all of these easily with a set of declarative object configuration files and that is Kubernetes.

## Kubernetes Architecture
### Node:
- A node is a machine, either physical or virtual on which Kubernetes is installed.
- A node is a worker machine where containers will be launched by Kubernetes.
- Earlier Nodes were known as Minions.
- But what if the node on which your application is running fails?
- Well, obviously our application goes down, so you need to have more than one node.

### Cluster:
- A Cluster is a set of nodes grouped together. This way even if one node fails you have your application still accessible from the other nodes.
- Moreover, having multiple nodes helps in sharing load as well.

 
- Now we have a cluster. But,
1. Who is responsible for managing the cluster?
2. Where is the information about members of the cluster stored?
3. How are the nodes monitored?
4. When a node fails how do you move the workload of the field node to another worker node?
That’s where the Master comes in.

### Master Node:
- The Master is another node with Kubernetes installed on it and is configured as a Master.
- The master watches over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.
- There are multiple master nodes but you have to assign priority to them.



### When you install Kubernetes on a system, you're actually installing the following components:
1. API Server
2. etcd
3. Kubelet
4. Container runtime
5. Controller
6. Scheduler


### API Server:
------------------------------
- The API server acts as the front-end for Kubernetes.
- The users, management devices, Command Line Interfaces all of them interact with the API server to interact with Kubernetes cluster.

### etcd:
------------------------------
- etcd is a distributed reliable key value store used by Kubernetes to store all data used to manage the cluster.
- When you have multiple nodes and multiple Masters in your cluster, etcd stores all that information on all the nodes in the cluster in a distributed manner.
- Etcd stores configuration data, state data, and metadata, on master node itself and this data is accessible to each node in a Kubernetes cluster.
- Etcd is also responsible for implementing logs within the cluster to ensure that there are no conflicts between the Masters.

### Controller:
----------------------------
- The controllers are the brain behind orchestration.
- They are responsible for noticing and responding when nodes, containers or end-points goes down.
- The controllers make decisions to bring up new containers in such cases.

### Scheduler:
-------------------------------
- The Scheduler is responsible for distributing containers across multiple nodes.
- It looks for newly created containers and assigns them to nodes.

### Container Runtime:
-----------------------------
- The Container Runtime is the underlying software that is used to run containers.
 
### Kubelet:
----------------------------
- Kubelet is the agent that runs on each node in the cluster.
- The agent is responsible for making sure that the containers are running on the nodes as expected.
- kubelet agent that is also responsible for interacting with a master to provide health information of the worker node and carry out actions requested by the Master on the worker nodes.

#### - Till now, we saw two types of servers:
1. Master
2. Worker

and a set of components that make up Kubernetes.
- But how are these components distributed across different types of servers?
- In other words, how does one server become a master and the other the slave, the worker node or minion.
- Worker nodes are where the containers are hosted. Containers such as docker containers or it can be other containers as well such as rocket or cryo.

- To run Docker containers on a system, we need container runtime installed.
- But throughout this course, we are going to use Docker as our container runtime engine.
- The master server has the kube API server and that’s what makes it a master.
- kubelet agent that is also responsible for interacting with a master to provide health information of the worker node and carry out actions requested by the Master on the worker nodes.
- All the information gathered are stored in a key value store on the master.
- The key value store is based on the popular etcd framework as we just discussed.
- The master also has the control manager and the Scheduler.
- When we set up our infrastructure, we also need to learn a little bit about one of the command line utilities known as the kube command line tool or kubectl or kube control as it is also called.

## - The kubectl tool is used:
1. To deploy and manage applications on a Kubernetes cluster.
2. To get cluster information.
3. To get the status of other nodes in the cluster.
4. To manage many other things.


### kubectl run hello-minikube
---------------------------------
- The kubectl run command is used to deploy an application on the cluster.


### kubectl cluster-info
----------------------------------
- The kubectl-cluster info command is used to view information about the cluster.

### kubectl get nodes
----------------------------------
- The kubectl get nodes command is used to list all the nodes part of the cluster.

## Quiz 1
### 1. What is a worker machine in Kubernetes known as?
#### Ans. Node or Minion
--------------------------------
### 2. A Node in Kubernetes can only be a physical machine and can never be a virtual machine.
#### Ans. False
-------------------------------
### 3. Multiple Nodes together form a _________
#### Ans. Cluster
-------------------------------
### 4. Which of the following processes runs on Kubernetes Master Node ______
#### Ans. Kube API Server
----------------------------------
### 5. Which of the following is a distributed reliable key-value store used by kubernetes to store all data used to manage the cluster
#### Ans. etcd
------------------------------------
### 6. Which of the following services is responsible for distributing work or containers across multiple nodes.
#### Ans. Scheduler
---------------------------------------

### 7. Which of the following is the underlying framework that is responsible for running application in containers like Docker?
#### Ans. Container Runtime
--------------------------------------
### 8. Which is the command line utility used to manage a kubernetes cluster?
#### Ans. kubectl

--------------------------------------------
## Pods
- As we discussed before with Kubernetes, our ultimate aim is to deploy our application in the form of containers on a set of machines that are configured as worker nodes in the cluster.
- However, Kubernetes does not deploy containers directly on the worker nodes.
- The containers are encapsulated into a Kubernetes object known as pods.
- A pod is a single instance of an application.
#### - A pod is the smallest object that you can create in Kubernetes.
- What if the number of users accessing your application increase and you need to scale your application and you need to add additional instances of your web application to share the load?
- Do we bring up new container instance within the same pod? No. - We create new pod altogether with a new instance on the same node.
- What if the user base further increases and your current node has no sufficient capacity?
- Well, then you can always deploy additional pods on a new node in the cluster.
- You will have a new node added to the cluster to expand the cluster's physical capacity.
- Pods usually have a 1 to 1 relationship with containers running your application.
- So, to scale up, you create new pods and to scale down you delete existing pods.
- You do not add additional containers to an existing pod to scale your application.
#### - We just said that pods usually have a 1 to 1 relationship with the containers, but are we restricted of having a single container in a single pod? No.
- A single pod can have multiple containers except for the fact that they're usually not multiple containers of the same kind.
- As we discussed in the previous slide, if our intention was to scale our application, then we would need to create additional pods.
- But sometimes you might have a scenario where you have a helper container that might be doing some kind of supporting task for our web application, such as processing a user entered data processing a file uploaded by the user. ET cetera.
- And you want these helper containers to live alongside your application container. In that case, you can have both of these containers as part of the same pod so that when a new application container is created, the helper is also created and when it dies, the helper also dies since they are part of the same pod.
#### - If the two containers share the same network, they can communicate with each other directly.
#### - Not only this, but two containers in the same pod can also share the same storage space.
- Let's assume we are developing a process or a script to deploy our application on a Docker host.
- Then we would first simply deploy our application using a simple Docker run Python app command, and the application runs fine and our users are able to access it.
- When the load increases, we deploy more instances of our application by running the Docker run commands many more times.
- This works fine and we are all happy now.


- Sometime in the future our application is further developed, undergoes architectural changes and grows and gets complex.
- We now have a new helper container that helps our web application by processing or fetching data from elsewhere.
- These helper containers maintain a 1 to 1 relationship with our application container and thus needs to communicate with the application containers directly and access data from those containers.


- For this, we need to maintain data that what app and helper containers are connected with each other.
- We would need to establish network connectivity between these containers ourselves using links and custom networks.
- We would need to create shareable volumes and share it among the containers.
- We need to maintain the data of that as well.
- Because, when the appliction container within a pod dies, we have to kill helper container manually.
- Same way, when a new container is created, we need to create helper container manually.
- Kubernetes does all of this for us automatically.
- We just need to define what containers a pod is consist of and the containers in a pod by default will have access to the same storage, the same network namespace and same fate as in they will be created together and destroyed together.
### How to deploy pods?

#### kubectl run nginx
- The above command will deploy Docker container by creating a pod.
- So it first creates a pod automatically and deploys an instance of the Nginx docker image	.
- But where does it get the application image from?
- For that you need to specify the image name using the image parameter which is --image, then image name.

	

- In this case, the NginX image is downloaded from the Docker Hub Registry.




## Demo-Minikube

### Commands to install kubectl utility and minikube:

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
	
kubectl version --client

kubectl version --client --output=yaml

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
 
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
 
sudo apt-get install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
 
sudo apt-get install minikube-linux-amd64
 
sudo apt-get install minikube
 
minikube --version
 
#### minikube start

#### minikube status




#### kubectl get nodes
O/P:	
NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane     3m58s      v1.30.0


#### kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10

#### kubectl get deployment
O/P:
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
hello-minikube   1/1     			1         	    1 	               47s


#### kubectl expose deployment hello-minikube --type=NodePort --port=8080

#### minikube service <Name of the deployment> --url
O/P:
you will get the url, click on it.



## Demo-Pods
- As we already know, a pod is the most smallest unit in Kubernetes.
- So, we will use the kubectl command line utility to interact with the Kubernetes cluster.

#### kubectl run nginx --image=nginx

- By default, Kubernetes will take the image of nginx from the Docker Hub.
- Check the status using below command:

#### kubectl get pods

O/P:
NAME	READY   STATUS    RESTARTS   AGE
nginx          1/1         Running               0          34s

#### To get the complete description of the pod:

#### kubectl describe pod nginx

IP address of the pods:
#### kubectl get pods -o wide
## Quiz 2
### 1. The smallest unit you can create in Kubernetes object model is:
#### Ans. Pod
-----------------------------------------
### 2. True or False: A Pod can only have one container in it.
#### Ans. False
---------------------------------------
### 3. What is the right approach to scale an application?
#### Ans. Deploy additional pods.

---------------------------------------
## Creating Pods using YAML
- Kubernetes uses YAML file as input for the creation of objects such as pods, replicas, deployment services 	etc.
### - Kubernetes definition file always contains 4 top level fields:
-----------------------------
1. apiVersion
2. kind
3. metadata
4. spec



## apiVersion:
- The first one is the API version.
- This is the version of the Kubernetes API you're using to create the objects.
- Depending on what we are trying to create we must use the right API version.
- For now since we are working on pod we will set the API version as v1.
- Other possible values for this field are:

## kind:
- The full form of kind is Kubernetes in Docker.
- The kind refers to the type of object we are trying to create which in this case happens to be a pod.
- So we will set it as pod.
- Some other possible values here could be replica set, deployment or service.

## metadata:
- The metadata is data about the object like its name labels etc.
- As you can see unlike the first two (apiVersion, kind) where you have specified a string value, metadata is in the form of a dictionary.
- So, everything under metadata is intended to the right a little bit and so names and labels are children of metadata.

## spec:
- The last section in the configuration file is the specification section which is written as spec depending on the object we are going to create.
- This is where we would provide additional information to Kubernetes pertaining to that object.
- Spec is a dictionary so add a property under it called containers. Containers is a list or an array.
- The reason this property is a list is because the pods can have multiple containers within them as we learned in the lecture.
- In this case though, we will only add a single item in the list since we plan to have only a single container.
- Suppose, the name of yaml file is pod_definition.yml. So, in order to run this file, you will have to write

#### kubectl create -f pod_definition.yml

- After running this command, kubernetes will create pod as mentioned in the yml file.

- To see the list of pods available
### kubectl get pods

pods using YAML file
- In this demo, we're going to create a pod again.
- But this time, instead of making use of the kubctl run command, we are going to create it using a YAML definition file.
- vim pod.yml



apiVersion: v1
kind: Pod
metadata:
	name: nginx
	labels: 
		app: nginx
		tier: frontend
spec:
	containers: 
	- name: nginx
	   image: nginx

kubectl apply -f pod.yml
kubectl get pods

## Difference between kubectl create and kubectl apply:

#### - The kubectl create command can only be used when the resource does not already exist. The kubectl apply command can be used to update an existing deployment.

#### - The kubectl create command is imperative, following instructions defined in the YAML. The kubectl apply command is declarative, reading the YAML and deciding what needs to be done to bring the infrastructure to the expected state.

Tips & Tricks – Developing Kubernetes Menifest Files with Visual Studio Code.
- VS Code is basically an IDE tool.
- IDE stands for (Integrated Development Environment).
- To use kubectl commands in VS code, first of all, you will have to download extention of YAML file.
- Go to 4 square icon in left side bar of VS Code.
- Search YAML by Red Hat.
- Paste the below code:

apiVersion: v1
kind: Pod
metadata:
	name: nginx-2
	labels: 
		env: production
spec:
	containers: 
	- name: nginx
	   image: nginx

Open the terminal and run command:
	
#### kubectl create -f pod_definition.yml


Replication controllers and Replication Sets
- Controllers are the brain behind the Kubernetes.
- They are the processes that monitor Kubernetes objects and respond accordingly. In this lecture, we will discuss about one controller in particular, and that is the replication controller.
- What if, for some reason our application crashes and the pod fails, users will no longer be able to access our application?
- To prevent users from losing access to our application, we would like to have more than one instance or pod running at the same time.
- That way, if one fails, we still have our application running on the other one.
- The replication controller helps us run multiple instances of a single pod in the Kubernetes cluster which provids high availability.
- So, does that mean you can't use a replication controller if you plan to have a single pod? No.
- Even if you have a single pod, the replication controller can help by automatically bringing up a new pod when the existing one fails.
- Thus, the replication controller ensures that the specified number of pods are running at all times, even if it is just 1 or 100.
- Another reason we need replication controller is to share the load across by creating multiple ports.
- For example, in this simple scenario, we have a single pod serving a set of users.
- When the number of users increase, we deploy additional pod to balance the load across the two pods.
- If the demand further increases and we run out of resources on the first node, we could deploy additional pods across the other nodes in the cluster.


## - It's important to note that there are two similar terms:
#### 1. Replication controller
#### 2. Replica set.

- Both have the same purpose, but they are not the same.
- Replication controller is the older technology that is being replaced by replica set.
- Replica set is the new recommended way to set up replication.
- However, whatever we discussed in the previous few slides remain applicable to both these technologies.
- To create replication controller, we start by creating replication controller definition file.

apiVersion: v1
kind: ReplicationController
metadata:
	name: myapp-rc
	labels:
		app: myapp
		type: front-end
spec:
	template:
		metadata:
			name: myapp-pod
			labels: 
				app: myapp
				type: frontpod
		spec:
			containers: 
				- name: nginx-container
				   image: nginx
	replicas: 3

- From apiVersion to metadata, everything is quite similar with the way we created pod earlier. However, specification part in Replication Controller is quite crucial.
- We know that the replication controller creates multiple instances of a pod, but what pod?
- We create a template section under spec to provide a pod template to be used by the replication controller to create replicas.
- Now, how do we define the pod template? It's not that hard because we have already done that in the previous exercise.
- Remember, we created a pod definition file in the previous exercise!
- We could reuse the contents of the file to populate the template section.
- Move all the contents of the pod definition file into the template section of the replication controller, except for the first few lines which are API version and kind.
- We now have two metadata sections, one is for the replication controller and another for the pod.
- Same way, we have two spec sections, one for each.
- In the above code, the replication controller part is the parent and pod definition is the child.
- In the end, we have mentioned that the number of replicas that we want to create is 3.
- Now to create the replication controller:

#### kubectl create -f rc_definition.yml

- To get the details of replication controller:
		
#### kubectl get replicationcontroller

- It will show you the details like:
Name, desired, current, ready, age 
- If you would like to see the pods that were created by the replication controller, run the kubectl get pods.
- All the pods created by the replication controller will have the name starting from the replication controller itself.

- Now, lets get into Replica Set.

vim replicaset-definition.yml

apiVersion: app/v1
kind: ReplicaSet
metadata:
	name: myapp-replicaset 
	labels: front-end
spec:
	template:
		metadata:
			name: myapp-pod
			labels: 
				app: myapp
				type: frontpod
		spec:
			containers:
				name: nginx-container
				image: nginx
	replicas: 3
	selector:
		matchLabels:
			type: front-end

### - To create the above given replica-set file.
#### kubectl create -f replicaset-definition.yml

### - To see the created replicas:
#### kubectl get replicaset

### - To get the list of pod:		
#### kubectl get pods

### - To delete a particuler pod fron the node:
#### kubectl delete pod <Name of the pod>

## Labels and Selectors
- So what is the deal with labels and selectors?
- Why do we label our pods and objects in Kubernetes?
- Let us look at a simple scenario.
- Suppose, we deployed three instances of our front end web application as three pods.
- We would like to create a replication controller or replica set to ensure that we have three active pods at any time which is one of the use cases of replica sets.
- You can use replication controller to monitor existing pods if you have already created them.
- In case they were not created, the replica set will create them for you.
- The role of the replica set is to monitor the pods and if any of them were to fail deploy new ones.
- The replica set is in fact a process that monitors the parts.
#### - Now how does the replica set know which pods to monitor?
- There could be hundreds of other pods in the cluster running different applications.
- This is where labeling our pods during their creation comes in.
- We can use labels to filter the pods for monitoring.
- This way replica set would know which pods it has to monitor.
- The template definition section plays a vital role in labels part.
- Let's now look at how we scale the replica set.
- Say we started with three replicas and in the future, we decided to scale to six.
#### - How do we update our replica set to scale to six replicas?
- Well, there are multiple ways to do it.
- The first, is to update the number of replicas in the definition file to six.
- Then run the below command to apply that change.
#### kubectl replace -f replicaset-definition.yml
- Another way of doing it is by running the below command:
#### kubectl scale --replicas=6 -f replicaset-definition.yml

### To delete the replicaset:
#### kubectl delete replicaset myapp-replicaset

- The above command also delete all the underlying pods.

- Now, we are going to create a replica set based on the pod definition file that we created earlier.

## Kubernetes Deployment
- We will now discuss about Kubernetes deployments.
- For a minute, let us forget about pods and replica sets and other Kubernetes concepts and talk about how you might want to deploy your application in a production environment.
- Say for example you have a web server that needs to be deployed in a production environment.
- You need many such instances of the web server for the purpose of high availability.
- Secondly, whenever newer versions of application builds become available on the docker registry, you would like to upgrade your docker instances seamlessly.
- However, when you upgrade your instances you do not want to upgrade all of them at once as it may impact users accessing your applications so you might want to upgrade them one after the other and that kind of upgrade is known as rolling updates.
- Suppose one of the upgrades you performed resulted in an unexpected error and you're asked to undo the recent change, you would like to be able to roll back the changes that were recently carried out.
- Finally, say for example you would like to make multiple changes to your environment such as upgrading the underlying Web Server versions as well as scaling your environment and also modifying the resource allocations etc.
- You do not want to apply each change immediately after the command is run, instead you like to apply a pause to your environment, make the changes and then resumes so that all the changes are rolled out together.
- All of these capabilities are available with the Kubernetes deployments.
- ### To see all the created objects at once run the below command:
#### kubectl get all
- To see the status of your deployment:
### kubectl get deployments
#### kubectl describe deployment <Name of the Deployment>


## Updates and rollback in Deployment
- In this lecture, we will talk about updates and rollbacks in a deployment.
- Before we look at how we upgrade our application, let's try to understand rollouts and versioning in deployment.
- When you first create a deployment, it triggers a rollout, a new rollout creates a new deployment revision. Let's call it Revision 1.
- In the future, when the application is upgraded, meaning when the container version is updated to a new one, a new rollout is triggered and a new deployment revision is created named Revision 2.
- This helps us keep track of the changes made to our deployment and enables us to roll back to a previous version of deployment if necessary.
### - You can see the status of your rollout by running the command:
#### kubectl rollout status <Name of the deployment>
- This command will show you the revisions and history of the deployments.
- There are two types of deployment strategies.
- Say, for example, you have five replicas of your web application instance deployed.
Strategy 1:
- One way to upgrade these to a newer version is to destroy all of these and then create newer versions of application instances, meaning first destroy the five running instances, and then deploy five new instances of the new application version.
- The problem with this, as you can imagine, is that during the period after the older versions are down and before any newer version is up, the application is down and inaccessible to users.
- This strategy is known as the Recreate Strategy and thankfully this is not the default deployment strategy.
Strategy 2:
- The second strategy is where we do not destroy all of them at once.
- Instead, we take down the older version and bring up a newer version one by one.
- This way the application never goes down and the upgrade is seamless.

- When I say update, it could be different things, such as updating your application version by updating the version of Docker containers used, updating their labels or updating the number of replicas, etcetera.
- Since we already have a deployment definition file, it is easy for us to modify this file by changing the version of the container.
- We run the kubectl apply command to apply the changes.
- This way, a new rollout is triggered and a new revision of the deployment is created.
- Say, for instance, once you upgrade your application, you realize something isn't very right. Something's wrong with the new version of build you use to upgrade.
- So you would like to roll back your update.
### - Kubernetes deployments allow you to roll back to a previous revision to undo a change by running the below given command:
#### kubectl rollout undo <Name of the deployment>
- The deployment will then destroy the pods in the new replica set and bring the older ones up in the old replica set and your application is back to its older format.

Demo – Deployments – Update and Rollback
- You create a file named deployment.yaml using vim.
- The file is below:








kubectl create -f deployment.yaml

kubectl rollout status deployment.apps/myapp-deployment

Now, let’s delete this deployment:

kubectl delete deployment myapp-deployment

kubectl create -f deployment.yaml

kubectl rollout status deployment.apps/myapp-deployment

kubectl rollout history deployment.apps/myapp-deployment

Now let’s delete the deployment again.

kubectl delete deployment myapp-deployment

kubectl create -f deployment.yaml --record

kubectl rollout status deployment.apps/myapp-deployment

kubectl rollout history deployment.apps/myapp-deployment

kubectl describe deployment myapp-deployment

let’s modify the deployment file:

kubectl edit deployment myapp-deployment --record

Upgrade the version of nginx


- As soon as you will exit the file by pressing :wq! The deployment will start automatically because of the last command that you ran.

kubectl describe deployment myapp-deployment

- So right now, it's running nginX version 1.18 say we have a requirement to use a different image instead of this.
- So, one way to do that is to use the edit deployment command and edit the image name within the YAML file that opens.
- Another way to do is to use another command called kubectl set image deployment and the name of the deployment, which is my app deployment and here I'm going to use the name of the container, which is nginx and this is a key:value format way of specifying the value.

kubectl set image deployment myapp-deployment nginx=nginx:1.18-perl --record

kubectl rollout history deployment/myapp-deployment  deployment.apps/myapp-deployment

## Basics of networking in Kubernetes
- In this lecture, we will discuss about networking in Kubernetes. Let us look at the very basics of networking in Kubernetes.
- We will start with a single node, Kubernetes cluster.
- The node has an IP address say it is 192.168.1.2.
- In this case, this is the IP address we use to access the Kubernetes node, SSH into it, etc.
- So on the single node Kubernetes cluster, we have created a single pod.
- As you know, a pod hosts a container, unlike in the Docker world where an IP address is always assigned to a Docker container, in the Kubernetes world, the IP address is assigned to a pod.
- Suppose, the IP address assigned to the pod is 10.244.0.2 .
- So how is it getting this IP address? When Kubernetes is initially configured?
- We create an internal private network with the address 10.244.0.0 and all the ports are attached to it.
- When you deploy multiple pods, they all get a separate IP assigned from this network.
- The ports can communicate to each other through this IP, but accessing the other pods using this internal IP address may not be a good idea as it is subject to a change when pods are recreated.
- For now, it's important to understand how the internal networking works in Kubernetes.
- So it's all easy and simple to understand when it comes to networking on a single node.
- But how does it work when you have multiple nodes in your cluster?
- In this case, we have two nodes running Kubernetes and they have IP addresses 192.168.1.2 and 192.168.1.3 assigned to them.
- Note that they are not part of the cluster yet. Each of them has a single pod deployed as discussed in the previous slide.
- These pods are attached to an internal network and they have their own IP addresses assigned.
- However, if you look at the internal network addresses, you can see that they are the same.
- The two networks have an address 10.244.0.0 and the paths deployed have the same address.
- This is not going to work well.
- Because, When the nodes are part of the same cluster, the parts have the same IP addresses assigned to them, and that will lead to IP conflicts in the network.
- As a matter of fact, when a Kubernetes cluster is set up, Kubernetes does not automatically set up any kind of networking to handle these issues. Instead, it expects us to set up networking to meet certain fundamental requirements.
- So, that all the containers or pods in a Kubernetes cluster can be able to communicate with one another without having to networking conflict.
- All nodes must be able to communicate with containers and all containers must be able to communicate with the nodes in the cluster.
- Fortunately, we don't have to set it up all on our own as there are multiple pre-built solutions available.
- Some of them are the Cisco ACI networks psyllium, big cloud, fabric, flannel, VMware and Zest and Calico, depending on the platform you're deploying your Kubernetes cluster on.
- You may use one of these solutions.

## Services - NodePort
- Now, we will discuss about Kubernetes services.
- Kubernetes services enable communication between various components within and outside of the application.
- Kubernetes Services helps us connect applications together with other applications or users.
- For example, our application has groups of pods running various sections, such as a group for serving a frontend load to users and other group for running backend processes, and a third group connecting to an external data source.
- It is a services that enable connectivity between these groups of pods.
- Services enable the frontend application to be made available to end users.
- Let's look at some other aspects of networking in this lecture.
- Let's start with external communication.
- So we deployed our pod, having a web application running on it.
- How do we, as an external user access the web page?
- First of all, let's look at the existing setup.
- The Kubernetes node has an IP address, and that is 192.168.1.2 and your laptop is on the same network as well, so it has an IP address 192.168.1.10.
- However, the internal pod network is in the range 10.244.0.0 and the pod has an IP address of 10.244.0.2.
- It is obvious that we can not ping the address 10.244.0.2.
- So, what are the options of seeing that webpage?
- First, if we need to SSH into the Kubernetes node at 192.168.1.2 from the node and then we would be able to access the pod's web page by doing a curl.
ssh 192.168.1.2	>	curl http://10.244.0.2	>	Web Page!
- However, this is from inside the Kubernetes node, and that's not what I really want.
- What I want is to to access the web server from my own laptop without having to switch into the node and simply by accessing the IP of the Kubernetes node.
- So we need something in the middle to help us with mapping the requests from our laptop through the node to the pod running the web container.
- This is where kubernetes services come into play.
- The Kubernetes service is an object, just like replicaset or deployments that we worked with earlier.
- One of its use cases is to listen to the pod on the node and forward requests to that pod running the web application.
- This type of service is known as a node port service because the service listens to a port of the node and forwards the request to the pod.
###  - There are other services as well:
1. NodePort
2. Cluster IP
3. Load Balancer

## NodePort:
- Earlier, we talked about a concept of gettig the web-page using SSH and curl.
- Let’s go deeper in it.
- There are three ports involved in this process.
- One is the port of pod which is 10.244.0.2.
- This port is also known as target port because this is where the request is forwarded.
- The second port is the port of service itself which is 80 for webservers such as apache and nginx.
- That port also has an IP address say 10.106.1.12
- Lastly, we have a port of node which is known as NodePort. For now, let’s set Node Port to the 30008.
- Node port have a default range which is 30000 to 32767.


- Now, we have to create the service for which we will use a definition file.


kubectl create -f service-definition.yml
kubectl get services
curl http://192.168.1.2:30008


- In Kubernetes, the selector field is a crucial part of a ReplicaSet's specification as it specifies how the ReplicaSet identifies which Pods it is responsible for managing.


- To summarize in any case, whether it be a single pod on a single node, multiple pods on a single node or multiple pods on multiple nodes, the service is created exactly the same without you having to do any additional steps during the service creation.
Services: Cluster IP
- A full stack web application typically has different kinds of pods hosting different parts of an application.
- You may have a number of pods running a frontend web server, another set of pods running a backend server, a set of pods running a persistent database like my SQL.
- The Web frontend server needs to communicate to the backend servers and the backend servers need to communicate with the database.
- So what is the right way of establishing connectivity between these services?
- We know that these pods all have an IP address assigned to them, as we can see on the screen. But these IPS are not static.
- These pods can go down any time and new pods are created all the time. Therefore, we cannot rely on these IP addresses for internal communication between the application.
- So, the solution is that the Kubernetes service can help us group the pods together and provide a single interface of access the pods in a group.
- For example, a service created for the back-end pods will help group all the back-end pods together and provide a single point of interface for other 	pods to access this service.
- Same goes for front-end and same goes for the database pods.
- This enables us to easily and effectively deploy a microservices based application on Kubernetes cluster.
- Each layer can now scale or move as required without impacting communication between the various services.
- Every group will become a cluster and every cluster will have an IP address which will be known as Cluster IP.


kubectl create -f service-definition.yml
kubectl get service 


Services: Load Balancer
- So, we have seen the node port service that helps us make an external facing application available on a port on the worker nodes.
- So let's turn our focus to the frontend applications, which are the voting app and the result app.
- Now, we know that these pods are hosted on the worker nodes in a cluster.
- So, let's say we have a four node cluster and to make the applications accessible to external users, we create the services of type node port.
- Instead of node-port in the definition file you need to mention load-balancer and everything will take place properly.


## Microservices Application
- In this lecture, we will try and understand microservices architecture using a simple web application.
- We will then deploy that web application on multiple different Kubernetes platforms, such as AWS.
- So let's first get familiarized with the application because we will be working with the same application in different sections through the rest of this course.



- This is a sample voting application which provides an interface for a user to vote and another interface to show the results.
- The application consists of various components such as the voting app, which is a web application developed in Python to provide the user with an interface to choose between two options a cat and a dog.
- When you make a selection, the vote is stored in Redis.
- For those of you who are new to read, this read is in this case serves as a database in memory.
- This vote is then processed by the worker, which is an application written in .net.
- The worker application takes the new vote and updates the persistent database, which is a PostgreSQL.
- In our case, the PostgreSQL simply has a table with a number of votes for each category which are cats and dogs. In this case, it increments the number of votes for cats as our vote was for cats.
- Finally, the result of the vote is displayed in a web interface, which is another web application developed in Node.js.
- This resulting application rates the count of votes from the Postgres SQL database and displays it to the user.
- So this is the architecture and data flow of this sample voting application stack.
- Now, we have to put together this application stack on a single Docker engine using Docker run commands.
- We know that the images of the above applications are already available on Docker Repository.
- Below is the docker containers that we will create:

docker run -d --name=redis redis
docker run -d --name=db postgres:9.4
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
docker run -d --name=result -p 5001:80 --link db:db result-app
docker run -d --name=worker --link db:db --link redis:redis worker
 
Link is a docker command line option which can be used to link two containers together.


Microservices Application on Kubernetes
- So, we just saw how the voting application works on ## Docker. Let's now see how to deploy it on Kubernetes.
- Goals:
#### 1. Deploy Containers
#### 2. Enable Connectivity
#### 3. External Access

- Steps:
1. Deploy pods:
- First of all we need to deploy this simple web app application container on the pod.
2. Networking:
- So once the pods are deployed, the next step is to enable connectivity between the services.
- So it's important to know what the connectivity requirements are. - So we must be very clear about what application requires access to what services.




- We know that the redis database is accessed by the voting app and the worker app.
- The voting app saves the vote to the Redis database, and the worker app reads the vote from there. We know that the PostgreSQL database is accessed by the Worker app to update it with the total count of votes, and it's also accessed by the result app to read the total count of votes to be displayed in the resulting web page in the browser.
- So how do you make one component accessible by another?
- Say, for example, how do you make the Redis database accessible by the voting app?
- The right way of doing is by services.
- So, we will create a service for the redis pod so that it can be aceessed by the voting app and the worker app and we will name it “Redis Service”.
- Same way, we will create a service for PostgreSQL pod so that PostgreSQL DB can be accessed by the worker and the result app.
- Now the next task is to enable external access. So for this we saw that we could use a service with a type set to node port.
- So we create services for voting app and the result app and set their type to node port.
- Lastly, we will have to decide on what port we are going to make them available on.