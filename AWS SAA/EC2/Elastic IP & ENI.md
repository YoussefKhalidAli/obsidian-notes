## 1. Elastic IP (EIP)

**Definition:**  
A **static, public IPv4 address** that can be associated with your **ENI or EC2 instance**.

- **Elastic:** Can move between instances in the same region for failover.  
- **Useful for:**  
  - Maintaining a fixed public IP for servers (DNS entries, external access)  
  - High availability & failover setups  

### 1.1 Key Points

- Can **map to a private IPv4** on an ENI  
- Can **reassociate to another ENI or EC2 instance** without changing DNS  
- **Charges:** You’re charged if an EIP is allocated but not associated with a running instance  

---

## 2. Elastic Network Interface (ENI)

**Definition:**  
A **logical network card** in a VPC that provides **network connectivity** to EC2 instances (and other resources).

- **Primary use:** Gives an EC2 instance a network identity (private IPv4, MAC, security groups).  
- **Can be used outside EC2:** e.g., for failover scenarios or attaching to other resources.  

### 2.1 Attributes of an ENI

| Attribute                     | Description |
|--------------------------------|------------|
| Primary Private IPv4           | Main IP address for the ENI/EC2 instance |
| Secondary Private IPv4         | Optional additional IP addresses |
| Elastic IPv4 / Public IPv4     | Optional public IP attached to private IP |
| Security Groups                | One or more can be attached |
| MAC Address                    | Unique hardware-like identifier |
| Availability Zone (AZ)         | ENIs are **bound to a specific AZ** |
| Attachment                     | Can attach/detach **on the fly** to EC2 instances |

### 2.2 Use Cases

- Attach multiple ENIs to **single EC2 instance** for network separation  
- **Move ENI between instances** → maintains private IP for failover  
- **High availability setups** (static private IP can move to standby instance)  

---

## 3. ENI + EIP Workflow

1. Allocate an **Elastic IP** (EIP) if public access is needed  
2. Create an ENI in an **AZ**  
3. Attach it to an EC2 instance → instance gets private IP (and optionally public EIP)  
4. If instance fails → detach ENI and attach to standby instance  
5. Private IP (and EIP if attached) **moves with the ENI** → failover with minimal downtime  

---

## 4. Summary

- **EIP** = static public IP, can move between ENIs or instances  
- **ENI** = virtual network card (can move between instances)  
- **Primary vs Secondary IPs:** Primary always stays with ENI, secondaries optional  
- ENIs are **AZ-bound**, cannot move to another AZ  
- Useful for **failover & high availability** scenarios