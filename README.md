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
aws ec2 create-vpc --cidr-block 10.0.0.0/16
```

### 2️⃣ Create a Public Subnet

```bash
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block 10.0.1.0/24
```

### 3️⃣ Create an Internet Gateway

```bash
aws ec2 create-internet-gateway
```

### 4️⃣ Attach the Internet Gateway to the VPC

```bash
aws ec2 attach-internet-gateway --internet-gateway-id <igw-id> --vpc-id <vpc-id>
```

### 5️⃣ Create a Route Table

```bash
aws ec2 create-route-table --vpc-id <vpc-id>
```

### 6️⃣ Create a Route to the Internet

```bash
aws ec2 create-route --route-table-id <rtb-id> --destination-cidr-block 0.0.0.0/0 --gateway-id <igw-id>
```

### 7️⃣ Associate Route Table with Subnet

```bash
aws ec2 associate-route-table --subnet-id <subnet-id> --route-table-id <rtb-id>
```

### 8️⃣ Modify Subnet to Enable Auto-assign Public IP

```bash
aws ec2 modify-subnet-attribute --subnet-id <subnet-id> --map-public-ip-on-launch
```

### 9️⃣ Launch EC2 Instance in the Subnet

```bash
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --count 1 \
  --instance-type t2.micro \
  --key-name my-key \
  --security-group-ids <sg-id> \
  --subnet-id <subnet-id> \
  --associate-public-ip-address
```

---

## 🔐 **Security Group Setup:**

Ensure your security group allows inbound traffic:

* **SSH** (port 22) from your IP
* **HTTP** (port 80) from anywhere if you're testing with a web server

---

## 🧪 **Test:**

* SSH into the instance.
* `curl google.com` to verify internet access.

---

If you'd like this as a **Terraform project**, a **CloudFormation template**, or a **detailed lab guide with screenshots**, just let me know!
