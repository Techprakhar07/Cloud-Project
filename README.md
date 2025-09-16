
---

# 📄 README 2 – **Secure VPC with Bastion Host and Private EC2**

```markdown
# 🔒 Virtual Private Cloud (VPC) with Secure Architecture

## 🎯 Objective
The goal of this project is to design a **secure AWS VPC architecture** with public and private subnets:
- Public subnet → Bastion host (jump server).  
- Private subnet → Application server (Nginx).  
- NAT Gateway → Allow outbound internet for private instances.  
- Security groups and routing → Enforce least privilege access.  

---

## 🛠️ AWS Services Used
- **Amazon VPC** → Custom VPC, subnets, route tables.  
- **Internet Gateway (IGW)** → Internet access for Bastion.  
- **NAT Gateway** → Outbound access for private instances.  
- **EC2 Instances** → Bastion host & private app server.  
- **Elastic IP (EIP)** → Static IP for NAT Gateway.  
- **Security Groups** → Fine-grained network security.  
- **SSH Tunneling** → Access private instance from laptop via Bastion.  

---

## 📌 Steps Performed

### 1. VPC & Subnets
- Created **Custom VPC**: `10.0.0.0/16`.  
- Created **Public Subnet**: `10.0.1.0/24`.  
- Created **Private Subnet**: `10.0.2.0/24`.

### 2. Routing
- Public subnet route → Internet Gateway.  
- Private subnet route → NAT Gateway (for outbound traffic).  

### 3. Bastion Host
- Launched EC2 in **Public Subnet** with public IP.  
- Allowed **SSH (22)** from `My IP`.  
- Attached `bastionkey07.pem` for authentication.  

### 4. Private Instance
- Launched EC2 in **Private Subnet** (no public IP).  
- Installed **Nginx** as web server:
  ```bash
  sudo dnf install nginx -y
  echo "Hello from Private EC2" | sudo tee /usr/share/nginx/html/index.html
  sudo systemctl start nginx
Allowed only HTTP (80) traffic inside VPC.

5. SSH Tunnel from Laptop

From local machine:

ssh -i "bastionkey07.pem" -L 8080:10.0.2.117:80 ec2-user@<BASTION_PUBLIC_IP>


Opened in browser:

http://localhost:8080


Successfully accessed Nginx page from private EC2 securely.
