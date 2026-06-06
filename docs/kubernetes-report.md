# Kubernetes Deployment Report – CampusFit Application

## Kubernetes vs Docker Networking

Docker networking allows containers to communicate with each other using Docker networks. In the CampusFit Docker deployment, the Node.js application connected to MongoDB using the hostname `mongo`, which was automatically resolved by Docker's internal DNS system. Docker networking works well for a small number of containers running on a single machine.

Kubernetes networking is more advanced. Every Pod receives its own IP address, and communication between Pods is managed using Services. Instead of connecting directly to Pod IPs, applications connect to stable Service names such as `mongodb`. Kubernetes DNS resolves the Service name to the correct Pod. This provides reliability because Pods can be recreated at any time while the Service endpoint remains unchanged.

## How PVC Works

Pods are ephemeral, meaning they can be deleted and recreated by Kubernetes. If MongoDB stored data only inside the Pod filesystem, all data would be lost when the Pod was recreated.

To solve this problem, a Persistent Volume Claim (PVC) was created. The PVC requested 1GB of storage from Kubernetes. Kubernetes automatically provisioned a Persistent Volume (PV) and bound it to the PVC. MongoDB mounted this storage at `/data/db`.

As a result, even if the MongoDB Pod is deleted and recreated, the database files remain stored in the Persistent Volume. The new Pod automatically reattaches to the same storage and continues using the existing data.

## Scaling Behavior

The CampusFit application was deployed using a Deployment with two replicas. Kubernetes created a ReplicaSet that maintained the desired number of Pods.

When a Pod was manually deleted using:

```bash
kubectl delete pod <pod-name>
```

the ReplicaSet detected that only one Pod was running while two were required. Kubernetes automatically created a replacement Pod to restore the desired state.

Scaling was also demonstrated using:

```bash
kubectl scale deployment campusfit-app --replicas=3
```

which increased the number of application Pods. This allows Kubernetes applications to handle increased traffic without modifying the application code.

## Challenges Faced

The first challenge was understanding the relationship between Deployments, ReplicaSets, and Pods. Initially it was difficult to understand which component was responsible for creating and maintaining Pods.

The second challenge was configuring persistent storage for MongoDB. Learning the difference between Persistent Volumes and Persistent Volume Claims required additional experimentation.

Another challenge was networking. Unlike Docker Compose, Kubernetes requires Services to expose applications and enable communication between Pods. Understanding ClusterIP and NodePort Services was necessary before the application became accessible.

Finally, managing configuration using ConfigMaps and Secrets required modifying the deployment manifests and understanding how Kubernetes injects environment variables into containers.

Overall, the project provided practical experience with container orchestration, networking, scaling, storage management, and application deployment using Kubernetes.
