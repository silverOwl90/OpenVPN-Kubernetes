# OpenVPN-Kubernetes

Setting up OpenVPN Access Server on Kubernetes requires several key steps, including installation, user configuration, and remote access. Here is a step-by-step guide.

## 🔹 **How to deploy it**

🔹 1. Deploy OpenVPN Access Server on Kubernetes

Apply the manifests:

    kubectl apply -f deployment.yaml 
    kubectl apply -f namespace.yaml 
    kubectl apply -f pvc.yaml
    kubectl apply -f service-vpn.yaml
    kubectl apply -f service-web.yaml

   Verify that the pod is running:

       kubectl get pods -l app=openvpn  

🔹 **2. Access the Web Administration Interface**

Once deployed, OpenVPN-AS exposes a web interface on ports **943** and **443**.

Get the service IP:

    kubectl get svc openvpn-web

-   The external IP.
    -   Example: If the IP is **192.168.1.100**, access:
        -   **Admin Panel**: [https://192.168.1.100:943/admin](https://192.168.1.100:943/admin)
        -   **Client Panel**: [https://192.168.1.100:943](https://192.168.1.100:943)

🔹 **3. Configure the Admin User**  
If setting up for the first time, set a password for the **openvpn** user:

    kubectl exec -it <pod-openvpn> -- sacli --user "openvpn" --new_pass "WhateverPasswordYouWant" SetLocalPassword  

Replace `<pod-openvpn>` with the actual pod name.

Then, log in at **https://<IP>:943/admin** using:

-   **Username**: `openvpn`
-   **Password**: (the one you set)

🔹 **4. Configure VPN Clients**  
To allow users to connect, generate client profiles:

-   **Create a VPN user**:
    
    1.  Navigate to **User Management > User Permissions**.
    2.  Add a new user and enable **Allow Access**.
    3.  Save changes.
-   **Download the connection profile**:
    
	1.  Go to **https://<IP>:943/**.
	2.  Log in with the created user.
	3.  Download the `.ovpn` file and use it with an OpenVPN client (e.g., OpenVPN Connect).

Now edit the downloaded file and replace the IP with public IP address wherever the IP address is mentioned.

🔹 **5. Connect to the VPN from a Client**

1.  Download the `.ovpn` file from step 4.
2.  Use **OpenVPN Connect** (Windows/macOS/Linux) or a mobile app.
3.  Import the profile and connect.