# Linux Virtualization 

## Part 1: Introduction to Virtualization Concepts

### Research

- **Virtualization**:  
  A technology that allows you to create multiple simulated environments or dedicated resources from a single physical hardware system. It enables efficient utilization of hardware resources.

- **Hypervisor**:  
  A software layer that enables virtualization by managing and allocating resources to virtual machines (VMs). Types include:
  - Type 1 (bare-metal)
  - Type 2 (hosted)

- **Virtual Machines (VMs)**:  
  Emulations of physical computers that run an operating system and applications. Each VM has its own OS and is isolated from the host and other VMs.

- **Containers**:  
  Lightweight, portable, and isolated environments that share the host OS kernel but package applications and their dependencies. Examples include Docker and LXC.

### Summary of Core Differences Between VMs and Containers

| **Aspect**      | **Virtual Machines (VMs)**                  | **Containers**                          |
|------------------|---------------------------------------------|-----------------------------------------|
| **Architecture** | Each VM has its own OS, running on a hypervisor. | Containers share the host OS kernel.    |
| **Resource Usage** | Heavy, as each VM requires a full OS.      | Lightweight, as they share the host OS. |
| **Isolation**    | Strong isolation due to separate OS instances. | Less isolation, as they share the kernel. |
| **Startup Time** | Slower, as the entire OS must boot.         | Faster, as they leverage the host OS.   |
| **Use Case**     | Ideal for running multiple OSes on one machine. | Ideal for microservices and app deployment. |

---

![Screenshot 2025-02-14 173956](https://github.com/user-attachments/assets/e979e842-7685-4a5e-b508-861c5c7df2fb)

## Part 2: Working with Multipass

### Installation
Multipass was installed using the following command:
```bash
sudo snap install multipass
```

### Basic Commands
Launch a default Ubuntu instance:
```bash
multipass launch --name my-lin4-VM 
```

List running instances:
```bash
multipass list
```

View details about an instance:
```bash
multipass info my-lin4-VM 
```

Access the shell of a running instance:
```bash
multipass shell my-lin4-VM 
```

Run a command on the instance:
```bash
multipass exec my-lin4-VM  -- lsb_release -a
```

Stop an instance:
```bash
multipass stop my-lin4-VM 
```

![Screenshot 2025-02-14 174048](https://github.com/user-attachments/assets/1ff01d86-d6c8-493b-a88f-be038611b273)

Delete an instance:
```bash
multipass delete my-lin4-VM 
multipass purge
```

![Screenshot 2025-02-14 173840](https://github.com/user-attachments/assets/7698aeea-2049-45cd-82fb-39760e1dc443)

### Cloud-init
**Study:**  
Cloud-init is a tool for initializing cloud instances. It can configure users, install packages, and run scripts during VM creation.

**Experiment:**  
Created a `cloud-init.txt` file to install nginx:
```yaml
#cloud-config
packages:
  - nginx
```

Launched an instance with the configuration:
```bash
multipass launch --name my-cloud-vm --cloud-init cloud-init.txt
```

### File Sharing  
Created a shared folder between the host and Multipass instance:
```bash
multipass mount /path/to/host/folder my-vm:/path/to/guest/folder
```

**Access:**  
Verified access to the shared folder from both the host and the VM.

---

## Part 3: Exploring LXD

### Study
LXD is a system container manager that provides a user-friendly experience for managing Linux containers and VMs.

### Setup
Installed LXD:
```bash
sudo snap install lxd
sudo apt install snap -y
lxd --version
Id
sudo usermod -aG lxd $USER
#Then log out and login again
lxc list
#Respond: no to clustering, yes to storage, dir for backend, yes to network bridge, no for LXD server...
lxc profile list
lxc network list
lxc storage list
To run all available images: 
$ lxc image list images:
```

![Screenshot 2025-02-19 235549](https://github.com/user-attachments/assets/b2b1845c-b339-4834-adcd-85a6d4812c88)

Initialized LXD:
```bash
lxd init
```

### Basic Commands
Create a container:
```bash
lxc launch ubuntu:24.04 el-container
```

List containers:
```bash
lxc list
```

![Screenshot 2025-02-19 235624](https://github.com/user-attachments/assets/e509144e-0f71-4519-a206-ab70817899bc)

Access the container shell:
```bash
lxc exec el-container -- /bin/bash
```

Stop and delete a container:
```bash
lxc stop el-container
lxc delete el-container
```

![Screenshot 2025-02-19 235640](https://github.com/user-attachments/assets/15173982-a793-4378-ad8e-fcba773bf364)

---

## Part 4: Working with Docker

### Installation
Installed Docker Engine:
```bash
sudo apt update #if exists.
sudo apt install docker.io
#For post installation guide
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```

### Basic Concepts
- **Images**: Templates for creating containers.
- **Containers**: Running instances of images.
- **Dockerfile**: A script to build Docker images.

### Experiment
Pulled an Ubuntu image:
```bash
docker pull ubuntu
```

Ran a container:
```bash
docker run -it ubuntu /bin/bash
```

![Screenshot 2025-02-19 235834](https://github.com/user-attachments/assets/9ce53ad2-a03b-4795-9ea2-40cba4297ecc)

Created a `Dockerfile` to build a custom image:
```dockerfile
FROM ubuntu
apt update && apt install -y nginx
CMD ["nginx", "-g", "daemon off;"]
```

Built and ran the image:
```bash
docker build -t my-nginx .
docker run -d -p 8080:80 my-nginx
```

---

## Part 5: Snaps for Self-Contained Applications

### Research
Snaps are containerized software packages that include all dependencies for consistent execution across Linux distributions.

### Experiment
Installed Snapcraft:
```bash
sudo snap install snapcraft --classic
```

Created a simple Snap package app:
```sh
 mkdir hello-snap
cd hello-snap
```

nano snapcraft.yaml file: 
```yaml
name: hello-snap
base: core22
version: '0.1'
summary: A simple snap that prints "Hello, Snap!".
description: |
  This snap packages a very basic Bash script that prints "Hello, Snap!".
  It demonstrates how to create a snap from a minimal shell application.

grade: devel      # Change to 'stable' when releasing.
confinement: devmode  # Change to 'strict' when releasing.

parts:
  hello-snap:
    plugin: dump
    source: .
    organize:
      hello-snap.sh: bin/hello-snap.sh

apps:
  hello-snap:
    command: bin/hello-snap.sh

```

Built and installed the Snap:
```bash
snapcraft

 sudo snap install ./hello-snap_0.1_amd64.snap --dangerous --devmode
 snap list
```

![Screenshot 2025-02-20 173702](https://github.com/user-attachments/assets/7f23c0f2-9008-4ef7-8cb7-e037a9255133)

---

## Challenges Faced

### Multipass File Sharing
Initially struggled with permissions when mounting folders. Resolved by ensuring proper user permissions on the host.
### LXC int
After installation, I do not know that I have to log out and login again, so I CONTINUE STRAIGHT WITH LXC INIT, but this gave me so much error. Solved it by loging out and then in again. 

### Docker Networking
Had issues accessing the Nginx container from the host. Fixed by mapping the container port to the host port.
### Snapcraft app creation 
It was very Challenging, until i disable temporary the firewall. But I learn a lot.
