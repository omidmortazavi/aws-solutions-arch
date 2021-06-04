# EC2
## Summary
- **Instance Types**
  - General
  - Memory
  - CPU
- **Instance Launch Types**
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

- **EC2 User Data**
  - Bootstrap sciprt

- **Storage**
  - **EBS** 
    - NAS
    - gp2/gp3 : SDD - General Purpose
    - io1/io2 : SDD - PIOPS
    - st1/sc1 : Throughput Optimized HDD / Cold HDD
    - **Snapshot** : Volume Backup
    - **AMI** : Amazon Machine Image
      - AWS
      - Personal
      - MarketPlace
    - **Instance Stores** : High-Perf HD attached to Physical Host -  Very high IOPS
    - EBS Multi-Attach : (io1/io2 only)
    - EBS Raid : RAID0 (Perf.) and RAID1 (Mirror)

  - **EFS**
    - Managed Network File System (NFS) 
    - Multi-AZ mountable to multiple EC2
    - HA, Scalable, Expensive (3x gp2), pay per use
    - Linux Only (POSIX)
    - Performance Mode
      - General Purpose
      - Max I/O
    - Throughput Mode
      - Bursting : Sclaed with file system size
      - Provisioned : Fixed at specified ammount
    - Storage Tiers
      - Standard
      - Infrequent Access (EFS-IA)
      - Lifecycle Management

- **Secuirity Groups** 
  - FW applied to an **instance**
  - Can reference another security group
  - Ports : SSH(22), FTP(21), SFTP(22), HTTP(80), HTTPS(443), RDP(3389)

- **Networking**
  - Elastic Network Interface (ENI)
    - Supports multiple ENI's per Instance
    - Portable within an AZ
  - Elastic IP : Fixed Public IP

- **Placement Groups**
  - Cluster : low-latency
  - Spread : critical-apps
  - Partition : distributed-scale

- **Hibernate** 
  - RAM state written to EBS volume

- **Advanced** 
  - Nitro
  - vCPU
  - Capacity Reservations

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

### EC2 Instance Storage
- **Elastic Block Storage (EBS)**
  - Network attached Volume
  - Data persists after instance
  - Can only be mounted to one instance at a time *multi-attach 
  feature
  - Can attach multiple EBS volumes to an EC2
  - Delete on Termination (Default on Root)
  - AZ Scoped

  - **Snapshots**
    - Backup of EBS Volume
    - Not necassary to detach, but recommended.
    - Can copy across AZ or Region
    - Can create a volume from a snapshot

  - **Amazon Machine Image**
    - Customisation of an EC2 Instance
    - Regionally Scoped
    - Public AMI
    - Own AMI
    - Marketplace AMI

  - ** EBS Volume Types**
    - gp2/gp3 (General Purpose SSD)
      - 1 to 16GB
      - gp3 you can adujust IOPS & Throughput, gp2 they are linked
    - io1/io2 (High Perf SSD)
      - Provisioned IOPS (PIOPS)
      - Database workloads
    - st1 (Low Cost HDD)
      - Throughput Optimised
    - sc1 (Lowest Cost HDD)
      - Archived data
    - Only gp and io can be used for boot volume
    - Measured in Size, IOPS, Throughput

    **Multi-Attach**
      - io1/io2 family
      - AZ scoped
      - App must handle concurrent read/write
      - Cluster-Aware filesystem (not XFS, EX4, etc..)
    
    **Encryption**
      - Data at rest encryption
      - Instance to Volume is encrypted
      - Snapshots are encrypted
      - Volumes from snapshot are encrypted
      - Leverages KMS (AES-256)
      - Encrypt an unencrypted volume
        - Create Snapshot
        - Encrypt Snapshot
        - Create new Volume from Snapshot
        - Attach Volume to Original Instance
    
    **EBS RAID**
      - Logical Volume of multiple EBS Volumes configured in OS
      - Already replicated in AZ
      - RAID 0, RAID 1 (Recommended for EBS)
      - RAID 0 - Performance
      - RAID 1 - Fault Tolerance by Mirroring

- **EC2 Instance Store**
  - High-Perfomance HD attached to Physical Host - Very high IOPS
  - EBS has *limited* performance
  - Ephemeral Storage
  - Good for buffer/scratch/temp content
  - Risk of HW failure

- **Elastic File Store**
  - Managed Network File System (NFS) 
  - Multi-AZ mountable to multiple EC2
  - HA, Scalable, Expensive (3x gp2), pay per use
  - ContentManagement, WebServing, Workpress
  - NFSv4.2
  - Linux Only (POSIX)
  - Encrytpion at rest with KMS
  - Performance Mode
    - General Purpose
    - Max I/O
  - Throughput Mode
    - Bursting : Sclaed with file system size
    - Provisioned : Fixed at specified ammount
  - Storage Tiers
    - Standard
    - Infrequent Access (EFS-IA)
    - Lifecycle Management

  - **EBS vs. EFS**
    - EBS 
      - One Instance, One AZ
      - Root Terminated by Default
    - EFS
      - Multi Instance, Multi AZ
      - Linux Only
      - 3x more expensive, PAYG
      - Lifecycle management

### Placement Groups
- Control over EC2 Instance placement
  - Cluster (low-latency in AZ)
  - Spread (Across different AZ - max 7 per group per az)
  - Partition (Speads across different patitions in an AZ, highley scalable)

### Instance Roles
- Use IAM Role to grant privileges to EC2 Instances

### EC2 Hibernate
- in-memory (RAM) state is preserved in root EBS volume
- volume must be encrypted
- long-running process, long-init
- only supported on certain instance types and AMI
- RAM limitation

### Advanced Instance
- EC2 Nitro : Next Gen Instances 
  - Better Network & EBS IOPS
  - Better Security
- vCPU
  - vCPU per Thread
  - Optimising CPU Options
    - Reduce # CPU Cores (Licensing) or Disable multithreading
- Capacity Reservations
  - Manual or planned end-date of reservation
  - No need for commitment