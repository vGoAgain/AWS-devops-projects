Here’s a clean, **GitHub-ready `README.md`** version of your lab. It keeps the technical depth but reads smoothly, looks professional, and is easy to skim for learners and interview prep.

---

# 🧪 AWS Networking – Hands-On VPC Lab

Build a **complete AWS VPC architecture** from scratch and understand **how traffic flows** between public and private resources in AWS.

This lab walks you through creating a production-style network using **VPC, subnets, route tables, Internet Gateway, NAT Gateway, security groups, and EC2 instances**.

---

## 🎯 Objective

By the end of this lab, you will be able to:

* Design a custom VPC with public and private subnets
* Configure routing for internet and private access
* Launch EC2 instances in different network zones
* Use a **jump box (bastion host)** to access private instances
* Enable outbound-only internet access using a **NAT Gateway**
* Understand real-world AWS networking interview concepts

---

## 🧱 Architecture Overview

* **VPC CIDR:** `10.0.0.0/16`
* **Public Subnet:** `10.0.1.0/24`
* **Private Subnet:** `10.0.2.0/24`
* **Internet Access:** Internet Gateway
* **Outbound Private Access:** NAT Gateway
* **Access Pattern:**

  * Internet → Public EC2
  * Public EC2 → Private EC2 (Jump Box)
  * Private EC2 → Internet (Outbound only)

---

## 📌 Part 1: VPC Creation

### Create a Custom VPC

* **Name:** `project1-vpc`
* **IPv4 CIDR:** `10.0.0.0/16`
* ~65,534 usable IPs

### CIDR Basics

| CIDR | Total IPs | Usable |
| ---- | --------- | ------ |
| /32  | 1         | 0      |
| /31  | 2         | 0      |
| /24  | 256       | 254    |
| /16  | 65,536    | 65,534 |

**Rule:**

* First IP → Gateway
* Last IP → Broadcast
* Usable = Total − 2

---

## 🌐 Part 2: Subnet Creation

### Why Subnets?

* Separate public and private access
* Multi-AZ deployments
* Reduced broadcast traffic
* Better security boundaries

### Create Subnets

**Public Subnet**

* Name: `public-project1`
* AZ: `ap-south-1a`
* CIDR: `10.0.1.0/24`

**Private Subnet**

* Name: `private-project1`
* AZ: `ap-south-1a`
* CIDR: `10.0.2.0/24`

> 🔎 *Production Tip:* Use multiple subnets across multiple AZs.

---

## 🌍 Part 3: Internet Gateway (IGW)

### Purpose

Enables **bidirectional internet access**.

### Steps

* Create IGW: `project1-igw`
* Attach it to `project1-vpc`

**Rule:**

> One VPC ↔ One Internet Gateway

---

## 🧭 Part 4: Route Tables

### What Are Route Tables?

They define **where traffic goes**.

### Public Route Table

* Name: `public-rt-project1`
* Route:

  ```
  0.0.0.0/0 → Internet Gateway
  ```
* Associate with `public-project1`

**Default Route (Auto-Created):**

```
10.0.0.0/16 → local
```

---

## 🔐 Part 5: Security Groups

### What is a Security Group?

* Instance-level firewall
* Stateful (return traffic allowed automatically)

### SSH Rule (Demo Only ⚠️)

```
Type: SSH
Port: 22
Source: 0.0.0.0/0
```

> 🚨 **Production Warning:** Never expose SSH to the entire internet.

---

## 🖥️ Part 6: Public EC2 Instance

### Launch Public Instance

* Name: `demo-public`
* AMI: Amazon Linux 2
* Type: `t2.micro`
* Key Pair: `project1-key`
* Subnet: `public-project1`
* Auto-assign Public IP: **Enabled**

### Connect via SSH

```bash
chmod 400 project1-key.pem
ssh -i project1-key.pem ec2-user@<PUBLIC_IP>
```

✅ Connection should succeed.

---

## 🔒 Part 7: Private EC2 Instance

### Launch Private Instance

* Name: `demo-private`
* Subnet: `private-project1`
* Public IP: **Disabled**
* Same key pair & security group

### Why Direct SSH Fails?

❌ No public IP
❌ No internet route

---

### Jump Box Access (Bastion Host)

**Copy key to public instance**

```bash
scp -i project1-key.pem project1-key.pem ec2-user@<PUBLIC_IP>:~/
```

**From Public → Private**

```bash
chmod 400 project1-key.pem
ssh -i project1-key.pem ec2-user@<PRIVATE_IP>
```

✅ You are now inside the private instance.

---

## 🚪 Part 8: NAT Gateway (Outbound Internet)

### Problem

Private instance cannot download updates.

### Solution

**NAT Gateway** → outbound internet only.

### Steps

1. Allocate Elastic IP
2. Create NAT Gateway in **public subnet**
3. Create `private-rt-project1`
4. Route:

   ```
   0.0.0.0/0 → NAT Gateway
   ```
5. Associate with `private-project1`

### Test

```bash
sudo yum update -y
```

✅ Works
❌ Still no inbound access

---

## 🔁 Part 9: Traffic Flow Explained

### Inbound (Internet → Public)

```
Internet → IGW → Public RT → Public Subnet → SG → EC2
```

### Jump Box (Public → Private)

```
Public EC2 → Local Route → Private Subnet → SG → Private EC2
```

### Outbound (Private → Internet)

```
Private EC2 → Private RT → NAT → IGW → Internet
```

### Key Differences

| Component        | Direction          |
| ---------------- | ------------------ |
| Internet Gateway | Inbound + Outbound |
| NAT Gateway      | Outbound only      |
| Local Route      | Internal VPC       |

---

## 🧹 Part 10: Cleanup (VERY IMPORTANT)

⚠️ **Avoid charges by deleting resources in order:**

1. Terminate EC2 instances
2. Delete NAT Gateway
3. Release Elastic IP
4. Delete subnets
5. Detach & delete IGW
6. Delete route tables
7. Delete VPC

> 💰 **Cost Trap:** NAT Gateways and Elastic IPs are billed hourly.

---

## 🎤 Part 11: Interview Questions

* Public IP vs Elastic IP
* SSH without public IP (SSM Session Manager)
* Security Group vs NACL
* Why NAT must be in a public subnet
* Jump box / bastion host concept
* Overlapping VPC CIDR blocks
* High-availability NAT design

---

## 🛠️ Part 12: Common Mistakes

### Can’t SSH to Public EC2?

* Public IP enabled?
* Port 22 open?
* IGW attached?
* Route `0.0.0.0/0 → IGW`?

### Private EC2 No Internet?

* NAT in public subnet?
* Private RT pointing to NAT?
* NAT status = Available?
* Elastic IP attached?

---

## ✅ Final Notes

This lab mirrors **real production AWS networking patterns** and covers **most VPC interview questions**.

If you truly understand this setup, **AWS networking stops being scary**.

Happy building 🚀
