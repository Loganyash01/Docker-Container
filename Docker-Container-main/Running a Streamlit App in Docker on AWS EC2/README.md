# 🚀 Deploying a Streamlit App in Docker on AWS EC2

## 📌 Overview
This guide provides a step-by-step approach to deploying a Streamlit app inside a Docker container on an AWS EC2 instance with a custom network setup. It covers:

✅ Setting up a VPC, Subnet, Route Table, and Internet Gateway

✅ Launching and configuring an EC2 instance

✅ Installing and configuring Docker

✅ Transferring project files to EC2

✅ Running the Streamlit app inside a Docker container

✅ Managing the Docker container

---

## 📖 Table of Contents

1. Setting Up a VPC, Subnet, Route Table, and Internet Gateway
2. Launching and Configuring an EC2 Instance
3. Connecting to EC2 
4. Setting Permissions for the PEM Key 
5. Installing and Configuring Docker 
6. Copying Project Files to EC2
7. Building and Running the Docker Container
8. Accessing the Streamlit App 
9. Managing the Docker Container

---

## 1️⃣ Setting Up a VPC, Subnet, Route Table, and Internet Gateway

🔹 **Create a New VPC**  
Go to AWS Console → VPC Dashboard → Create VPC  
- Name: `MyCustomVPC`
- IPv4 CIDR block: `10.0.0.0/16`

🔹 **Create a Subnet**  
Go to VPC Dashboard → Subnets → Create Subnet  
- Select `MyCustomVPC`
- Subnet name: `MyPublicSubnet`
- CIDR block: `10.0.1.0/24`
- Enable **Auto-assign Public IPv4**

🔹 **Create an Internet Gateway and Attach to VPC**  
- Name: `MyIGW`
- Attach it to `MyCustomVPC`

🔹 **Create and Associate a Route Table**  
- Name: `MyPublicRouteTable`
- Destination: `0.0.0.0/0`, Target: `MyIGW`
- Associate with `MyPublicSubnet`

---

## 2️⃣ Launching and Configuring an EC2 Instance

🔹 **Launch an EC2 Instance**  
- Name: `Streamlit-EC2`
- AMI: `Amazon Linux 2023`
- Instance Type: `t2.micro (Free Tier)`
- Key Pair: **Select/Create a key pair**
- Network: `MyCustomVPC`
- Subnet: `MyPublicSubnet`
- Enable **Auto-assign Public IP**
- Security Group: Allow `SSH (22)`, `HTTP (80)`, `Streamlit (8501)`

---

## 3️⃣ Connecting to EC2

🔹 **Via EC2 Instance Connect**  
Go to EC2 Dashboard → Select Instance → Click **Connect**  
Choose **EC2 Instance Connect** → Click **Connect**

---

## 4️⃣ Setting Permissions for the PEM Key

1.	Move your .pem key to your work directory:
```bash
mv /path/to/your-key.pem ~/your-work-directory/
```
or you can directly copy-paste it in your work-directory

2.	Set the correct permissions(Run this command in Git Bash):
```bash
chmod 600 your-key.pem
```
<p align="center">
  <img src="https://raw.githubusercontent.com/TarakKatoch/My-Docker-Dockyard/ba5e5c2e19d6d2012c8de8f17f23902c817805da/Running%20a%20Streamlit%20App%20in%20Docker%20on%20AWS%20EC2/assets/Screenshot%202025-02-20%20031810.png" alt="Streamlit App Screenshot">
</p>

---

## 5️⃣ Installing and Configuring Docker

```bash
sudo yum update -y
sudo yum install -y docker
sudo systemctl enable docker
sudo systemctl start docker
```
<p align="center">
  <img src="https://raw.githubusercontent.com/TarakKatoch/My-Docker-Dockyard/832b5736931414654f7f86f829d835fd54015c7f/Running%20a%20Streamlit%20App%20in%20Docker%20on%20AWS%20EC2/assets/Screenshot%202025-02-20%20031258.png" alt="Streamlit App Screenshot">
</p>
<p align="center">
  <img src="https://raw.githubusercontent.com/TarakKatoch/My-Docker-Dockyard/832b5736931414654f7f86f829d835fd54015c7f/Running%20a%20Streamlit%20App%20in%20Docker%20on%20AWS%20EC2/assets/Screenshot%202025-02-20%20031336.png" alt="Streamlit App Screenshot">
</p>

---

## 6️⃣ Copying Project Files to EC2

From your local machine, transfer files using SCP(Run this command in Git Bash):

```bash
scp -i your-key.pem app.py Dockerfile requirements.txt mushrooms.csv ec2-user@your-ec2-public-ip:/home/ec2-user/
```
<p align="center">
  <img src="https://raw.githubusercontent.com/TarakKatoch/My-Docker-Dockyard/832b5736931414654f7f86f829d835fd54015c7f/Running%20a%20Streamlit%20App%20in%20Docker%20on%20AWS%20EC2/assets/Screenshot%202025-02-20%20031902.png" alt="Streamlit App Screenshot">
</p>

---

## 7️⃣ Building and Running the Docker Container

🔹 **Connect to EC2 and navigate to project directory**

```bash
cd /home/ec2-user
```

🔹 **Build the Docker image**

```bash
sudo docker build -t streamlit-app .
```

🔹 **Run the container**

```bash
sudo docker run -d -p 8501:8501 --name streamlit_container streamlit-app
```
<p align="center">
  <img src="https://raw.githubusercontent.com/TarakKatoch/My-Docker-Dockyard/832b5736931414654f7f86f829d835fd54015c7f/Running%20a%20Streamlit%20App%20in%20Docker%20on%20AWS%20EC2/assets/Screenshot%202025-02-20%20032050.png" alt="Streamlit App Screenshot">
</p>

---

## 8️⃣ Accessing the Streamlit App

🌐 Open your browser and visit:

```text
http://your-ec2-public-ip:8501
```
<p align="center">
  <img src="https://raw.githubusercontent.com/TarakKatoch/My-Docker-Dockyard/832b5736931414654f7f86f829d835fd54015c7f/Running%20a%20Streamlit%20App%20in%20Docker%20on%20AWS%20EC2/assets/Screenshot%202025-02-20%20032157.png" alt="Streamlit App Screenshot">
</p>

---

## 9️⃣ Managing the Docker Container

🔹 **Check running containers**
```bash
sudo docker ps
```

🔹 **Stop the container**
```bash
sudo docker stop streamlit_container
```

🔹 **Remove the container**
```bash
sudo docker rm streamlit_container
```

🔹 **Restart the container**
```bash
sudo docker start streamlit_container
```

---

## 🎯 Conclusion
This guide helps you deploy a Streamlit app inside a Docker container on AWS EC2 with a custom VPC setup. The deployment ensures scalability, security, and high availability for your application. 🚀🎉

---

✅ Happy Deploying! 🖥️🐳☁️
