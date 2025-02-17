# kubedam-photos
![image](https://github.com/user-attachments/assets/9395105c-fa89-4255-8bb8-995df658280e)
![image](https://github.com/user-attachments/assets/a7edc9a0-271c-47be-a3b3-193e94701869)
![image](https://github.com/user-attachments/assets/e5c4bb58-2e90-4c56-a037-7a9a15050281)
kubeadm join 192.168.10.4:6443 --token 3ne5op.qd57ts8uc2356ju5 --discovery-token-ca-cert-hash sha256:83b249d24ded19db1c937a73341142beb86afd4c8aea2468a3c7e7b1b80b6b40
![image](https://github.com/user-attachments/assets/2d628e54-4a3c-43b5-aabf-f81854ebc35a)
![image](https://github.com/user-attachments/assets/d4d09c31-b5ff-4a1c-8dd4-c4c325550348)

1️⃣ Verify Cluster & Workloads
Before starting the upgrade, check the current state of your cluster:

bash
Copy
Edit
kubectl get nodes
kubectl get pods -A -o wide
Ensure your worker node is Ready.
Identify which workloads are running on it.
2️⃣ Temporarily Move Critical Workloads to Master (if possible)
If your master node allows workloads (i.e., does not have the NoSchedule taint), you can schedule some workloads on it.

Check if the master allows scheduling:
bash
Copy
Edit
kubectl describe node <master-node-name> | grep Taints
If the master is tainted (NoSchedule), remove the taint temporarily:
bash
Copy
Edit
kubectl taint nodes <master-node-name> node-role.kubernetes.io/control-plane:NoSchedule-
Now, if you have any Deployment-based workloads, you can scale them up to replicate pods on the master:3️⃣ Cordon & Drain the Worker Node
Cordoning prevents new pods from being scheduled while draining safely removes existing ones.

bash
Copy
Edit
kubectl cordon <worker-node-name>
kubectl drain <worker-node-name> --ignore-daemonsets --delete-emptydir-data



bash
Copy
Edit
kubectl scale deployment <your-deployment> --replicas=2
⚠️ If your workloads require multiple nodes (e.g., stateful apps like databases), consider setting up a temporary backup.





3️⃣ Cordon & Drain the Worker Node
Cordoning prevents new pods from being scheduled while draining safely removes existing ones.

bash
Copy
Edit
kubectl cordon <worker-node-name>
kubectl drain <worker-node-name> --ignore-daemonsets --delete-emptydir-data

4️⃣ Upgrade the Worker Node OS
Now, upgrade the OS on the worker node:

For Ubuntu/Debian:

bash
Copy
Edit
sudo apt update && sudo apt upgrade -y
sudo reboot


5️⃣ Rejoin the Worker Node to the Cluster
After the node reboots, ensure it re-joins the cluster.

Check Node Status:
bash
Copy
Edit
kubectl get nodes
If the node is NotReady, restart kubelet:

bash
Copy
Edit
sudo systemctl restart kubelet
If the worker node is not joining automatically, re-run the kubeadm join command:

bash
Copy
Edit
sudo kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>


6️⃣ Uncordon the Worker Node & Verify
Once the worker node is Ready, allow scheduling again:

bash
Copy
Edit
kubectl uncordon <worker-node-name>
Verify that all pods are running correctly:

bash
Copy
Edit
kubectl get pods -A -o wide


















