# Relational Database Service
## Summary

---
## Notes
- Relational Database Service (RDS)
  - Postgres
  - MySQL
  - MariaDB
  - Oracle
  - MSQL
  - Aurora (AWS proprietary)
- Managed Service
  - Automated provisioning, patching
  - Backup and Point in Time Restore
  - Dashboard
  - Read Replicas
  - Multi AZ
  - Maintenance Windows
  - Scaling
  - Storage backed by EBS
  - Cannot SSH into Instance

- **Backup**
  - **Automated**
    - Daily full backup
    - Transaction logs every 5min
    - 7 Day retention (upto 35d)
  - **DB Snapshot**
    - Manually triggered
    - User defined retention

- **Storage Autoscaling**
  - Horizontal scaling
  - Set Maximum Storage Threshold

- **Read Replicas**
  - Replication is ASYNC so reads are eventually consistent
  - Scalability NOT availability
  - Scale Reads to DB
  - Only support SELECT type commands
  - Upto 5 Replicaes
  - Regionally scoped
  - Replicas can be promoted
  - Application *must* update connection string for replicas
  - No Inter AZ cost for Read Replica within region
  - **Use Cases**
    - Run reporting on read-replicas

- **RDS Multi AZ**
  - SYNC Replication
  - Availability not scalability
  - One *DNS* name
  - Read Replicas can be setup as Multi-AZ for DR
  - Single -> Multi AZ
    - Modify DB, zero downtime operation

- **RDS Security**
  - **Encryption**
    - **At-Rest**
      - Encrypt master & replicas with AWS KMS - AES256
      - Defined at launch
      - If master not encrypted cannot encrypt replicas
    - **In-Flight**
      - SSL Certificate based
      - Provide SSL option with trust cert
      - Enabled in *parameter group*
    - **Operations**
      - Encrypted RDS DB -> Encrypted snapshot
      - Unencrypted RDS DB -> Unencrypted snapshot
      - To Encrypt a DB
        - Create snapshot (unencrypted)
        - Copy and encrypt snapshot
        - Restore DB from encrypted snapshot
        - Migrate apps to new DB, delete old one
  - **Network and IAM**
    - **Network Security**
      - Usually deployed to a private subnet
      - Levarages security groups
    - **Access Manangement**
      - IAM controls who can *manage* RDS instance
      - Traditional username/pw to *login* to db
      - IAM-Based Auth for RDS MySQL and Postgres
        - Auth via token obtained through IAM & RDS API Calls
        - Lifetime of 15mins
        - SSL encrypted

- **Amazon Aurora**
  - Amazon propriety
  - Postgres & MySQL support
  - Cloud optimised
    - 5x faster than MySQL, 3x than Postgres
  - 15 read replicas vs mySQL 5
  - Replication is faster (sub 10ms)
  - Failover is instant, its HA native (faster than RDS Multi-AZ)
  - Costs 20% more than RDS
  - **High Availability**
    - 6 copies of your data across 3 AZ
    - Self healing with peer-to-peer replication
    - Storage Striped across 100s of volumes
      - Horizontally Scaled - 10GB to 64 TB
    - One Instance takes writes (master)
    - Automated failover of master in less than 30s
    - Master + upto 15 Read Replicas
    - Support for Cross Region Replication

