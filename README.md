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
