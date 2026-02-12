# Week 2 – Session 1  
## Podman Installation and Basic Usage on Linux (WSL)

### Step 1: Update the Linux System
Before installing any packages, update and upgrade your system to ensure all dependencies are current.

```bash
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Podman on Linux
Install Podman using the following command:

```bash
sudo apt-get -y install podman
```

### Step 3: Setup Configuration (QEMU Installation)
This command installs **QEMU**, an open-source tool used for hardware emulation and virtualization, which helps support container execution in WSL environments.

```bash
sudo apt install qemu-system-x86
```

---

## Pulling Ready-Made Images from Container Registries

To pull a prebuilt image from a container registry (example: quay.io):

```bash
podman pull quay.io/jupyter/base-notebook
```

- **quay.io** is a container registry where container images are stored.  
- Other registries include **docker.io**, **ghcr.io**, etc.

To list all downloaded images on your system:

```bash
podman images
```

To remove or delete an image:

```bash
podman rmi <img_id>
```

---

## Running Containers

To run a container from an image:

```bash
podman run <image_id>
```

Run a container in **detached mode** (background execution):

```bash
podman run -d <image_id>
```

Assign a custom name to a container:

```bash
podman run -d --name <your_own_name> <image_id>
```

View all running containers:

```bash
podman ps
```

Stop a running container:

```bash
podman stop <image_name / image_id>
```
## Port Building For Container Access

See the images that are present 
```
podman images
```
Run the image in the detached mode and name the image
```
podman run -d --name <ur_name> -p 8888:8888 <img_id>
```
To see the link where the conatiner is running
```
podman logs <img_name>
```
## Mounting the Cintainer file structure with local file Structure
```
podman run -d --name <your name> -p 8888:8888 -v~/<HostFilePath>:<ContainerFilePath> <Repo_name>
```
## Containerzing ollama for local LLM's
```
podman pull docker.io/alpine/ollama:0.15.6
```
## Integrating LLM inside a container
```
podman run -d --name ollama docker.io/alpine/ollama

podman exec -it ollama sh 
```
It will open the interactive shell
```
ollama run gemma3:270m
```
It will install the gemma3:270m model inside the container

## Podman Networking for inter-container Communication

- First Create a container
```
podman network create ai        # this is to create a network

podman network ls               # List the available network

podman network rm ai            # remove the network
```
Here **ai** is the network name

- add / connect the two containers to the network.
```
podman run -d --name jupylab -p 8888:8888 -v ~/Dummy:/home/jovyan/work --network ai -p 5000:5000 quay.io/jupyter/base-notebook

podman run -d --name ollama -p 11434:11434 --network ai -v ollama:/root/ollama docker.io/alpine/ollama
```


