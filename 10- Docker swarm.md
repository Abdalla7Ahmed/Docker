
**Docker Swarm** is a container orchestrator tool offered by Docker as a application mode, for managing multiples nodes, either physical or virtual, and it’s deployments like a whole single system, that is the “Swarm”, natively in Docker.

These nodes of the Swarm, as it was said, are connected with each other, can be either physical or virtual, and based on this, its roll can be different:

- **Manager nodes**: These contain the Swarm Manager, the process in charge of handling the commands in the Swarm mode and reconciling the desired state with the actual cluster state.
- **Worker nodes**: These contain the workloads of your system.


![screen](./images/10.1.png)

### **Key Components of Docker Swarm**

1. **Nodes**:
    
    - A node is any machine (physical or virtual) that runs Docker and participates in the swarm.
    - **Types of Nodes**:
        - **Manager Node**: Orchestrates and manages the swarm. It schedules tasks and maintains cluster state.
        - **Worker Node**: Executes the tasks (containers) assigned by the manager.
    - **Manager-to-Worker Communication**:
        - Managers send task definitions to workers.
        - Workers report their status back to managers.
2. **Manager Nodes**:
    
    - At least one manager is required, but for high availability, it's recommended to have **an odd number of managers** (e.g., 3, 5, or 7).
    - **Preferred Number**: 3 managers are enough for most cases to ensure fault tolerance while avoiding excessive overhead.
3. **Worker Nodes**:
    
    - Responsible for running containers.
    - Workers do not manage the swarm or schedule tasks.
4. **Tasks and Services**:
    
    - **Task**: A single instance of a running container.
    - **Service**: A definition of how tasks should be executed, including the number of replicas.
5. **Overlay Network**:
    
    - Swarm uses an overlay network to enable communication between services running on different nodes. 


