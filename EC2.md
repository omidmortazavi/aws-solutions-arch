# EC2
## Summary
- EC2 : VM
- EBS : Virtual Drives
- ELB : Load Balancing
- ASG : Auto-Scaling
- EC2 User Data: Bootstrap sciprt

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
Storage Optimized (Ix/Dx): 