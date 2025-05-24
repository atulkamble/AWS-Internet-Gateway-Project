**“Internet Gateway” project on AWS** that demonstrates how to set up a VPC and allow EC2 instances to access the internet. This is useful for understanding **VPC networking basics** like subnets, route tables, and gateways.

---

## 🔧 **Project Title:** Setting Up an Internet Gateway in AWS

### ✅ **Objective:**

To create a custom VPC with an Internet Gateway and launch an EC2 instance that can access the internet.

---

## 🛠️ **Components Used:**

* **Amazon VPC**
* **Subnet**
* **Internet Gateway**
* **Route Table**
* **EC2 Instance**
* **Security Group**

---

## 📝 **Steps:**

### 1️⃣ Create a VPC

```bash
aws ec2 create-default-vpc
aws ec2 create-vpc --cidr-block 10.0.0.0/16
aws ec2 create-vpc \
  --cidr-block 10.0.0.0/16 \
  --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=myvpc}]'
aws ec2 delete-vpc --vpc-id vpc-0b39f70cdf4afc638
```

### 2️⃣ Create a Public Subnet

```bash
aws ec2 create-subnet --vpc-id vpc-0170bc4d060213ea2 --cidr-block 10.0.1.0/24
```

### 3️⃣ Create an Internet Gateway

```bash
aws ec2 create-internet-gateway
```

### 4️⃣ Attach the Internet Gateway to the VPC

```bash
aws ec2 attach-internet-gateway --internet-gateway-id igw-03eaa26f4fe5bdb45 --vpc-id vpc-0170bc4d060213ea2
```

### 5️⃣ Create a Route Table

```bash
aws ec2 create-route-table --vpc-id vpc-0170bc4d060213ea2
```

### 6️⃣ Create a Route to the Internet

```bash
aws ec2 create-route --route-table-id rtb-0afda24b8d3dd7e36 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-03eaa26f4fe5bdb45
```

### 7️⃣ Associate Route Table with Subnet

```bash
aws ec2 associate-route-table --subnet-id subnet-0e677c82616ee740e --route-table-id rtb-0afda24b8d3dd7e36
```

### 8️⃣ Modify Subnet to Enable Auto-assign Public IP

```bash
aws ec2 modify-subnet-attribute --subnet-id subnet-0e677c82616ee740e --map-public-ip-on-launch
```
### Create mykey.pem
```
aws ec2 create-key-pair \
  --key-name mykey \
  --query 'KeyMaterial' \
  --output text > mykey.pem
```
### 9️⃣ Launch EC2 Instance in the Subnet

```
aws ec2 run-instances \
  --image-id ami-0953476d60561c955 \
  --count 1 \
  --instance-type t3.micro \
  --key-name mykey \
  --security-group-ids sg-0d91531613c3e6618 \
  --subnet-id subnet-0e677c82616ee740e \
  --associate-public-ip-address \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=MyEC2Instance}]'
```
---

## Note Down Security Group then run following script 🔐 **Security Group Setup:**
```
git clone https://github.com/atulkamble/AWS-Internet-Gateway-Project.git
cd AWS-Internet-Gateway-Project
chmod +x add-sg-rules.sh
sudo sh add-sg-rules.sh
OR
./add-sg-rules.sh
```
Ensure your security group allows inbound traffic:

* **SSH** (port 22) from your IP
* **HTTP** (port 80) from anywhere if you're testing with a web server

---

## 🧪 **Test:**

* SSH into the instance.
* `curl google.com` to verify internet access.

## Delete 
```
delete ec2 instance 
detach internet gateway | delete internet gateway 
delete vpc (automatically deleted subnet, route table)
```
---
