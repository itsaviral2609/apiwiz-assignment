# Linux:

## 1. Provide steps to create a directory inside a directory where the parent directory does not exist.

### Solution
```
Command to create a directory inside another directory when a parent directory don't exists

mkdir -p PARENT_DIRECTORY/CHILD_DIRECTORY
```

![Sol1](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-07%2021-33-02.png)


---

## 2. How to install a package on a Linux server when there is no internet connection?

### Solution
```
Installing a package on a Linux server without an internet connection can be achieved by manually downloading the package and its dependencies on a machine with internet access, transferring them to the target server, and then installing them.

On a machine with internet access:

Find and download the .deb package and its dependencies

- apt-get download <package-name>

To download dependencies, you might need to use a tool like apt-rdepends to recursively download all dependencies:

-apt-get install apt-rdepends  # Install apt-rdepends
apt-rdepends <package-name> | grep -v "^ " | xargs apt-get download

Copy all the .deb files to a USB drive or use scp if you can connect to the target server over a local network

On the target server:

Transfer the .deb files from the USB drive to the server.
Install the package using dpkg:

- dpkg -i /path/to/package.deb


```
   
---

## 3. How to access specific folders of Server A from Server B and Server C?


you can use SSH to access specific folders on Server A from Server B and Server C. SSH (Secure Shell) can be used to log into another computer over a network, to execute commands on that computer, and to move files from one computer to another.

You can log in from Server B or Server C to Server A using the following command:
```
ssh user@ServerA
```

### Solution

---

## 4. How to check all the running processes from a server?

### Solution

```
One can use commands like 
- ps aux
- top
```

---

## 5. Provide the command to delete all the files older than X days inside a specific directory.

### Solution
```
For example, to delete files older than 2 days in the directory /home/user/files, you would use:

find /home/user/files -type f -mtime +2 -exec rm {} \;

Where:
- type f ensures that only files are considered, not directories.
- mtime +X selects files with a modification time older than X days.
- exec rm {} \; removes each file that meets the criteria. The {} is a placeholder for the current file that find is processing, and \; ends the -exec command.

```

---

## 6.Create a shell script to identify the process ID

a. script should as a user input for process ID

b. If the process exists script should print the process ID and exit 

c. If the process doesn't exist script should print the process doesn't exist and asks for another input

Script

```
#!/bin/bash

# Function to check if the process exists
check_process() {
    local pid=$1
    if ps -p "$pid" > /dev/null 2>&1; then
        echo "Process with PID $pid is running."
        exit 0
    else
        echo "Process with PID $pid does not exist."
    fi
}

while true; do
    read -p "Enter the PID provided by the user: " pid
    if [[ ! $pid =~ ^[0-9]+$ ]]; then
        echo "Please enter a valid numeric PID."
    else
        check_process "$pid"
    fi
done

```


 ### Solution     ![Steps](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-07%2022-36-55.png)  
![Script](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-07%2022-37-15-1.png)

---

# 2. Docker:

### 1. What is docker and why do we need it?

```
Docker is a tool that allows developers to package an application and its dependencies together into a container, which is like a lightweight, stand-alone package that can run almost anywhere. This containerization is beneficial because it ensures that the application will run the same, regardless of where it is.

Imagine you’re an author and you’ve written a book. Now, you want to send it to various publishers so they can read and publish it. However, different publishers might have different preferences for receiving manuscripts. Some might want it in a Word document, others in a PDF, and some may even want a printed copy. This is a problem because you'd have to prepare your manuscript in different formats to satisfy each publisher's requirements.

Now, imagine if instead, there was a standard ‘book container’ that all publishers accepted. You could put your manuscript in this container and send it to any publisher, confident that they could open it and read it without any issues, regardless of their preferences.

That's what Docker does for applications. It takes your application and puts it into a container which includes everything the application needs to run: the code, a runtime, libraries, and environment variables. When you send this container to different environments (like a developer's laptop, a test environment, or a live server in the cloud), the application will run in the same way, because it has everything it needs inside the container.

Why do we need Docker?

Consistency: Docker ensures that your application works the same way in development, testing, and production environments because it includes all the necessary components.

Efficiency: Containers share the machine's OS kernel and do not require an OS per application, making them lighter than traditional virtual machines. This means you can run more applications on the same hardware than if you were using VMs.

Isolation: Applications in Docker containers are isolated from each other and the underlying system. This means that if one container goes down, it won't affect the others.

Portability: Since a Docker container includes everything it needs, you can easily move it between different machines running Docker without worrying about dependencies.

Microservices Architecture: Docker is well-suited for microservices, where applications are built as a set of smaller services that run in their own containers. This can make applications easier to scale and maintain.
```

### 2. Write a docker file for a sample Java/python application.

Dockerfile for a flask application 

Flask app.py
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_geek():
    return '<h1>Hello from Flask & Docker</h2>'


if __name__ == "__main__":
    app.run(debug=True)
```

Dockerfile
```
FROM python:3.8-slim-buster

WORKDIR /python-docker

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0"]
```

###	3. What is the docker lifecycle?

### Solution

```
The Docker lifecycle outlines the various states and phases a Docker container goes through during its existence. Here's a detailed look into the lifecycle stages:

Create: This is the initial phase where a Docker container is instantiated but not yet running. At this stage, the container has been created with all the necessary configuration but awaits to be started​​​​.

Running: Once a container is started using the Docker run command, it enters the Running state, where it is actively executing its processes. The Running state is the active phase of a container's lifecycle where it serves its purpose of hosting applications or services​​​​.

Paused/Unpaused: During its operation, a container can be paused, which suspends its processes. Unpausing resumes the operation from the exact state it was in when paused. This is useful for temporarily halting container execution without stopping it entirely​​​​.

Stopped: A container is stopped when it has completed its task or when it is manually stopped. This state is not the end of the container's lifecycle as it can be restarted unless it is removed​​.

Killed/Deleted: The final phase of the lifecycle is when a container is killed or deleted. This happens when a container is no longer needed or to free up resources. Once a container is killed, it is removed from the system and cannot be restarted​​​​.

```

###	4. What is the difference between an image and a container?

```
Image

An image is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files. In the Docker ecosystem, an image is built from a series of layers that are stacked on top of each other to form a filesystem. These layers are created by instructions in a Dockerfile, which is a script composed of various commands and arguments needed for the image construction.

Images are immutable, meaning that once they are created, they do not change. If you need to make changes, you create a new image with the desired modifications. Images are used to create containers.

```
```
Container

A container is a running instance of an image. When you start an image, the containerization system (like Docker) sets up an isolated environment for that container, which is then used to run the application or service. This isolation leverages kernel features such as namespaces and cgroups in Linux to allow multiple containers to run on the same system without interference.

Containers are ephemeral and mutable. This means that they can be started, stopped, deleted, and replaced by new containers based on the same image or a different one. Any changes made to the running container are lost when that container is deleted unless those changes are committed to a new image
```

###	5. How to check docker container logs? Provide the command for the same

### Solution

![Alt text](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-08%2001-09-40.png)
---

# 3. Kubernetes:
### 1. What are different types of services?
```
In Kubernetes, there are primarily four types of services designed to manage different types of access to applications. Here's an overview of each:

ClusterIP: This is the default Kubernetes service type and is used for internal communication within the cluster. It exposes the service on an internal IP in the cluster, making it only reachable from within the cluster​​​​.

NodePort: This type of service exposes the service on a static port on each node’s IP address. This means that the service can be accessed from outside the cluster by hitting the NodePort on any node's IP​​​​.

LoadBalancer: This service type exposes the service externally using a cloud provider's load balancer. The external load balancer directs traffic to the NodePort service, which then routes to the ClusterIP service inside the cluster​​​​.

ExternalName: Maps a service to the externalName field by returning a CNAME record with its value. This is not a type of service that gets exposed like the others but serves as a sort of alias to more easily refer to an external service​​.
```
###	2. What is a pod? 

```
In Kubernetes, a pod is the smallest deployable unit that can be created and managed. It is a logical collection of one or more containers that are deployed together on a single host machine. Here are some key points about a pod in Kubernetes:

Containers in a Pod: Containers within a pod share the same network IP address and port space, and can find each other via localhost. They can also communicate with each other using standard inter-process communications like SystemV semaphores or POSIX shared memory.

Shared Resources: Containers in the same pod share the same IPC namespace, and can also share volumes. This allows data to be shared between the containers.

Management: Pods are ephemeral in nature. They are not designed to run forever, and when a pod dies, it is not resurrected. Instead, Kubernetes uses higher-level constructs like Deployments or StatefulSets to manage the lifecycle of pods, including scaling and recovery from failures.

Pod Specifications: A pod is described with a YAML or JSON file called a pod manifest. This manifest describes the pod's desired state: which containers to run, what volumes to mount, what network settings to apply, and more.

Scheduling: Pods are scheduled to nodes based on resource availability and constraints. The scheduler is responsible for deciding which node a pod runs on.

Use Cases: Pods can run a single container, but they can also run multiple containers that need to work together. For instance, a pod might include both the container with your web server and a different container that feeds data to that web server from a shared data store.

Lifecycle: Kubernetes provides pod lifecycle events like Pending, Running, Succeeded, Failed, and Unknown, reflecting the state of the pod in the cluster.
```
###	3. Create a pod with the above created custom image when a pod dies k8s should automatically restart

```
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-pod
spec:
  containers:
  - name: my-custom-container
    image: my-custom-image:latest 
    ports:
    - containerPort: 80 
  restartPolicy: Always
```
### 4. How to access the custom application with a specific port?

```
To access a custom application running on a specific port in a Kubernetes (k8s) cluster, you need to expose that application to the outside world. This is typically done using Services and Ingress.

Create a Service:

A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them. The type of Service you create will depend on your needs:

ClusterIP: Exposes the service on an internal IP in the cluster. This type makes the service only reachable from within the cluster.
NodePort: Exposes the service on the same port of each selected Node in the cluster using NAT. This makes the service accessible from outside the cluster using <NodeIP>:<NodePort>.
LoadBalancer: Exposes the service externally using a cloud provider’s load balancer.

For example, if you want to expose a web application running on port 8080 using a NodePort, your Service definition might look like this:


apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: my-application
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
      nodePort: 30007

In this example, my-application is the selector that matches the label of your application's pods. targetPort is the port the container accepts traffic on, port is the abstracted Service port, which can be any port other pods use to access the Service.

- kubectl apply -f my-service.yaml

Access the Application:

For a NodePort service, find your Node's IP address (which could be a VM or a physical machine - depending on your cluster setup) and access your application using <NodeIP>:<NodePort>.
For a LoadBalancer service, the cloud provider will provision a load balancer for your service, and an external IP address will be assigned to your service. Once the load balancer is created, use the external IP to access your application.



```


---

# 4. CI/CD:

### 1. Set up a pipeline (Github actions/Gitlab runner/ Jenkins or any open source tool) to build, test, create a docker image, publish and deploy to k8s. Use the application present in this public repo https://github.com/apiwizlabs/wizdesk.

### Solution

[GitHub-Action-k8s-pipeline](https://github.com/itsaviral2609/GitHub-Action-k8s-pipeline)

Workflow.yaml
```
name: CI/CD Pipeline with Docker and kind

on:
  push:
    branches: [ main ]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v3
      with:
        repository: apiwizlabs/wizdesk
        ref: main

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18' 

    - name: Install Dependencies
      run: npm install

    - name: Start the Application
      run: npm start

    - name: Build Docker Image
      run: docker build -t aviralsingh2609/wizdesk:latest . 

    - name: Log in to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_HUB_USERNAME }}
        password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}

    - name: Push Docker Image to Docker Hub
      run: docker push aviralsingh2609/wizdesk:latest 

    - name: Install kind
      run: |
        curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
        chmod +x ./kind
        sudo mv ./kind /usr/local/bin/kind

    - name: Create a Kubernetes cluster with kind
      run: kind create cluster

    - name: Load Docker Image to kind Cluster
      run: |
        kind load docker-image itsaviral2609/wizdesk:latest

    - name: Apply Kubernetes Manifests
      run: |
        kubectl apply -f k8s/ 

    - name: Check Deployment Status
      run: |
        kubectl rollout status deployment/my-deployment 
```

### 2. Automate to spin up a network and virtual machines. Install the Nginx package and start the service(any cloud)

### Solution

#### Cloud provider is Vultr

Wrote its file but couldn't deploy.

![Alt text](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-09%2000-14-03.png)

![Alt text](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-09%2000-15-01.png)

![Alt text](https://github.com/itsaviral2609/apiwiz-assignment/blob/main/tasks/Screenshot%20from%202023-11-09%2000-15-24.png)

    
