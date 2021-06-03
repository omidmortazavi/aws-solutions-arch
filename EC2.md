# EC2
## Summary
- EC2 : VM
- EBS : Virtual Drives
- ELB : Load Balancing
- ASG : Auto-Scaling
- EC2 User Data: Bootstrap sciprt

- Instance Types 
  - General
  - Memory
  - CPU
- Instance Launch Types 
  - On-Demand 
  - Reserved
  - Spot
    - one-time | persistent
  - Spot Fleets
    - Spot Instance + (optinal) On-Demand
    - Launch Pool Stratagies
      - lowestPrice
      - diversified
      - capacityOptimized
  - Dedicated

- Secuirity Groups : FW applied to an **instance**
  - Can reference another security group
  - Ports : SSH(22), FTP(21), SFTP(22), HTTP(80), HTTPS(443), RDP(3389)

- Networking 
  - Elastic Network Interface (ENI)
    - Supports multiple ENI's per Instance
    - Portable within an AZ
  - Elastic IP : Fixed Public IP

- Placement Groups
  - Cluster : low-latency
  - Spread : critical-apps
  - Partition : distributed-scale

- Hibernate

---
## Notes
### EC2 Basics
EC2 Sizing & Config
- OS
- CPU
- RAM
- Storage Space
  - Network Attached (EBS/EFS)
  - Hardware (EC2 Instance Store)
- Network
- Firewall
- Bootstrap

### EC2 User data
- Bootstap an instance using an **EC2 User Data** script
- Runs as Root User

### EC2 Instance Types
**Format**
m5.2xlarge
m : instance class
5 : generation
2xlarge : size within instance

General Purpose (Tx/Mx): Balanced CPU,RAM,NW
Compute Optimized (Cx): Processor
Memory Optimized (Rx): RAM
Storage Optimized (Ix/Dx): Disk

### EC2 Launch Types
- On-Demand
  - High Cost, but no upfront
  - linux : per sec
  - windows : per min
- Reserved 
  - 1 Year or 3 Year
  - Upfront/Partial/All Upfront
  - Steady State
  - Types : 
    - Reserved: long
    - Convertible: long, flexible
    - Scheduled: e.g Thu 3 - 6
- Spot 
  - Short workloads, cheap
  - Highest Discount - upto 90%
  - Can be lost at any point if price drops below spot
  - Spot changes over time
  - Resilient workloads (Batch, Image, Distributed)
  - SpotBlock : block for a period of time
  - Request Type
    - One-Time | Persistent
    - Cancel **Spot Request** first, then terminate instance
- Spot Fleets
  - Spot Instance + (optinal) On-Demand
  - Launch Pools
    - Strategies (lowestPrice, diversified, capacityOptimized)
- Dedicated Host
  - Entire Physical Host
  - Compliance, Server-Bound Software (e.g BYOL)
  - 3Yr period
- Dedicated Instances
  - Instances running on Harware dedicated to you
  - May share hardware with other instances in same account
  - No control over instance placement

### Security Groups
- FW applied to NIC on instance
- Control Inbound (Default deny any)
- Control Outbound (Default allow all)
- Can be attached to multiple instances
- Can attach multiple SG's to an ENI
- Scoped to VPC in region
- A Security Group can reference another Security Group

### SSH
`ssh -i privatekey.pem ec2-user@myip`
- SSH Key Pair 
  - chmod 0400 to change permission on key (owner-read)
- Linux User : ec2-user
- EC2 Instance Connect (Works on limited AMIs)

### Networking
- Elastic Network Interface (ENI)
  - vNIC on Instance
    - 1x Primary IPv4 (Eth0)
    - 1+ Secondary IPv4 (eth1 .. ethN)
  - One Public IPv4
  - One Elastic IP per private IPv4
  - One or More Security Groups
  - Portable within an AZ

- Elastic IP : Fixed Public IP
  - Max of 5 per account

### Placement Groups
- Control over EC2 Instance placement
  - Cluster (low-latency in AZ)
  - Spread (Across different AZ - max 7 per group per az)
  - Partition (Speads across different patitions in an AZ, highley scalable)

### Instance Roles
- Use IAM Role to grant privileges to EC2 Instances

### EC2 Hibernate
