# docker_swarnm_2_node_cluster
# Step 1)configer the Cluster hosts file
To start, log init each of the nodes and update the file with the following entires:

    sudo vim /etc/hosts
  
    manager ip swarm-manager
    worker ip  swarm-worker

Next ensure that all the nodes can ping each other.

    $ ping manager ip
    $ ping worker ip
# Step 2) Install Docker CE on all nodes:
The next step is to install DOcker on all nodes.
   
    $ sudo apt update
next, install the prerequisites needed during the installation

    $ sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

Once all the packages have been installed, add the Docker GPG key

    $ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o       
     /etc/apt/trusted.gpg.d/docker.gpg

In the next step, add the official Docker repository to your Ubuntu 22.04 system

    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu 
      $(lsb_release -cs) stable"

Next, update the local package index to make the system, aware of the newly added repository.

    
    $ sudo apt update

Then install Docker from the official Docker repository,

    $ sudo apt install docker-ce -y


Once Docker is installed, add the currently logged-in user to the Docker group to avoid running Docker as a sudo user every time you run Docker.

    
      $ sudo usermod -aG docker ${USER}
      $ newgrp docker

# Step 3) Verify Docker is running on all the nodes

      $ sudo syatemctl status docker

Additionally, be sure to enable the Docker service so that it starts automatically on boot time.

      $ sudo systemctl enable docker

# Step 4) Create Docker Swarm Cluster

The next step is to initialize the Docker Swarm on the Manager node. Once initialized, we will then add the worker nodes to the cluster.

To create a Docker Swarm Cluster, run the command:

      $ sudo docker swarm init --advertise-addr manager ip

Once Docker Swarm has been initialized, a command for joining the worker nodes to the cluster will be displayed on the terminal. Copy the command as you will need to run it on each of the worker nodes as previously mentioned.

Next, login back to each of the worker nodes and paste the command in order to join the cluster.

      $ sudo docker swarm join --token SWMTKN-1-1k397e5o52cae0yipopqcu9werjcwuss1exbyj4635rrjjl723- 
         7ocx56uhb7p1ri7h2u6ynxyno 10.128.0.57:2377

Next, confirm that all the nodes have joined the cluster as follows.

      $ sudo docker node ls

# Step 5) Teat Docker Swarm Installation

To test docker swarm installation, head over to the manager node and deploy a container application to the cluster. In this example, we are deploying an Nginx web server container and mapping it to port 8080 on the host.

      $ sudo docker service create --name web-server --publish 8080:80 nginx:latest

Next, verify the status of the application service deployed.

      $ sudo docker service ls

# Step 6) Create a replicas of the service

      $ sudo docker service scale web-servwe=4

At this point, Nginx web server container should be running across all the nodes in the cluster on port 8080. To confirm this, head over to your browser, and access the web server from all the nodes.

      http://manager-node:8080
      



      
  
