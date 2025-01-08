

### 1. **Difference Between Virtual Machines (VMs) and Containers**
![screen](Docker/images/1.1.png)

| **Feature**         | **Virtual Machines (VMs)**                                        | **Containers**                                                 |
| ------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------- |
| **Isolation**       | VMs provide full hardware-level isolation.                        | Containers provide OS-level isolation.                         |
| **OS Requirements** | Each VM has its own full operating system, including kernel.      | Containers share the host OS kernel.                           |
| **Resource Usage**  | More resource-intensive due to separate OS for each VM.           | Lightweight, as they share the host OS and are more efficient. |
| **Startup Time**    | VMs take longer to boot (minutes) due to full OS initialization.  | Containers start almost instantly (seconds or less).           |
| **Portability**     | VMs are less portable and depend on the hypervisor and VM format. | Containers are highly portable using tools like Docker.        |
| **Use Cases**       | Used for running completely different OSs or legacy applications. | Used for microservices and lightweight deployments.            |
| **Security**        | Stronger isolation due to separate kernels.                       | Relatively weaker, but improving with namespaces and cgroups.  |


### 2. **VM Template Export/Import**

- **Export Template**:
    
    - A VM template is a pre-configured virtual machine that can be reused to deploy multiple VMs.
    - Exporting creates a file or a set of files that represent the VM, including its configuration, disk image, and sometimes its network settings.
    - Tools like **VirtualBox**, **VMware**, or **Hyper-V** have export options (`OVF`, `OVA`, etc.) for creating portable templates.
- **Import Template**:
    
    - To use a template, you import the exported file into another system or hypervisor.
    - The new VM is instantiated with the same configurations as the template.
    - For example:
        - In VirtualBox: Use the _"Import Appliance"_ feature to load `.ova` or `.ovf` files.
        - In VMware: Use the _"Deploy OVF Template"_ option.


### **3. Container Image Build and Pull**

**3.1 Building a Container Image**

**What It Means**:

- Building a container image involves creating a package that contains everything needed for an application to run. This includes the application code, runtime, libraries, dependencies, and settings.
**How It Works**:

1. **Base Image Selection**:
    - You start with a base image (e.g., Ubuntu or Alpine Linux) that provides the foundational operating system or runtime environment.
2. **Custom Layers**:
    - You add layers to this base image, such as installing software packages, adding configuration files, or copying your application into the image.
3. **Image Configuration**:
    - You define metadata like the entry point (how the application should start) or environment variables needed for the container.
4. **Final Packaging**:
    - The result is a container image, which is a static snapshot of the application and its environment.
**Outcome**:

- The built image can be reused, shared, and deployed to run consistent environments across different machines.

**3.2 Pulling a Container Image**

- **What It Means**:
    
    - Pulling a container image means downloading an existing image from a centralized repository (like Docker Hub) to use it on your system.
- **How It Works**:
    
    1. **Find the Image**:
        - A user searches or knows the name of a pre-built image, such as "nginx" or "node.js."
    2. **Connect to the Registry**:
        - The local system communicates with a container registry to locate the specified image.
    3. **Download the Image**:
        - The system downloads the layers of the image from the registry and stores them locally. If some layers are already available locally, they are reused.
    4. **Verify Integrity**:
        - The image is verified to ensure it hasn’t been tampered with during the download.
- **Outcome**:
    
    - The pulled image is now available locally and can be used to create containers for deployment.




### How to install docker ?

**Step 1: Download the Installation Script**

- Use the `curl` or `wget` command to download the script:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
```
**Step 2: Run the Script**

- Execute the script to install Docker:
```bash
sudo sh get-docker.sh
```
This script automatically installs Docker Engine, CLI, and containerd.

**Step 3: Add Your User to the Docker Group (Optional)**

- To run Docker without `sudo`, add your user to the `docker` group:
```bash
sudo usermod -aG docker $USER
```
- Log out and log back in for this change to take effect.

**Step 4: Verify the Installation**

- Check the installed Docker version:
```bash
docker --version
```
- Test Docker with a simple container run:
```bash
docker run hello-world
```

### commands in docker
**1. Pull the Image**
Downloads the image from Docker Hub to your local machine.
```bash
docker image pull <image_name>
```
**2. List Available Images**
Displays a list of all locally stored Docker images.
```bash
docker image ls
```
**3. Create a Container from the Image**
- Creates a new container from specific image in an interactive mode (`-it`).
- The container is created but not started yet.
```bash
docker container create -it <image>
```
**4. List All Containers**
- Lists all containers on your system, including both running and stopped ones.
- This helps you find the **Container ID** or **Name** for the next step.
```bash
docker container ls -a
```
**5. Start the Container**
- Starts the container with the **ID** (e.g., `271d6` is an example ID obtained from the `docker container ls -a` output).
- The `-i` flag attaches you interactively to the container, providing access to its terminal.
```bash
docker container start -i <container_id>
docker container start -i 271d6
```

**6-Stop the Container**
If the container is running in the background:
```bash
docker container stop <container_id>
```
**7-Remove the Container**

To delete the container after stopping it:
```bash
docker container rm <container_id>
```

**8-Remove the Image**

To delete the image after stopping and removing all associated containers:
```bash
docker image rm <image_name>
```