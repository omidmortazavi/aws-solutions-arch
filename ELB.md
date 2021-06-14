# ELB
## Summary
- **ELB** : Elastic Load Balancers
  - **CLB** : Classic Load Balancer
    - HTTP,HTTPS,TCP

  - **ALB** : Application Load Balancer
    - HTTP,HTTPS,WebSocket
    - Listeners
    - **Target Groups**
    - **Health Checks**
    - Rules
    - Routing

  - **NLB** : Network Load Balancer
    - TCP,TLS (Secure TCP) & UDP
    - Rules

  - **GLB** : Gateway Load Balancer
    - *To be added not in exam scope atm*

- **Stickiness**
  - CLB and ALB using Cookies

- **Cross Zone LB**
  - Each LB in an AZ evenly distributes traffic across instances in *all* zones
  - Supported on ALB, NLB and CLB *different costing & default

- **SSL Certificates**
  - **SNI** - Enables multiple target groups for different websites with using different SSL/TLS certs

- **ASG** : Auto Scaling Groups
  - Horizontal Elastic Scaling
  - Automatic LB registration
  - **Launch Config**
    - AMI, Instance, User Data etc.
  - **Scaling Policies**
    - Min/Actual/Max Size
    - EC2 Metrics, CloudWatch Alarms, Custom, Scheduled


---
## Notes
- **ELB** : Elastic Load Balancers
  - Internal (Private) or External (Public)
  - **CLB** : Classic Load Balancer
    - v1
    - HTTP,HTTPS,TCP
    - Fixed hostname : xxx.region.elb.amazonaws.com
  - **ALB** : Application Load Balancer
    - v2
    - HTTP,HTTPS,WebSocket
    - Fixed hostname : xxx.region.elb.amazonaws.com
    - True IP in X-Forwarded-For, also x-forwarded-port and x-forwarded-proto
    - **Target Groups** 
      - Multple apps on multiple machines
      - Multiple apps on same machine (e.g containers)
      - Targets
        - EC2 Instance
        - ECS Task
        - Lamda Function
        - Private IP's
    - Rules
    - Redirects
    - Routing 
      - Path in URL (example.com/users)
      - Hostname in URL (one.example.com)
      - Query Strings and Headers (example.com/users?id=123&order=false)
  - **NLB** : Network Load Balancer
    - v2
    - TCP,TLS (Secure TCP) & UDP
    - Millions of requests
    - Low Latency ~100MS vs ~400ms for ALB
    - One *static IP per AZ*
    - Supports Elastic IP

  - **Health Checks**
    - Specify Port and Route (*/health* is common)
    - If response is not 200 then the instnace is unhealthy

  - **Security Groups**
    - **Application Security Gruop** - On EC2, reference the LB SG


- **ASG** : Auto Scaling Groups
  - Horizontal Elastic Scaling
  - Automatic LB registration
  - **Launch Config**
    - AMI + Instance Type
    - EC2 User Data
    - EBS Volumes
    - Sec Groups
    - SSH Kep Pair
  - **Scaling Policies**
    - Min/Actual/Max Size
    - Possible to scale on CloudWatch Alarms
    - *Newer* rules based on EC2 metrics (CPU, Requests, Network In/Out)
    - *Custom* metric based on API PUT
    - *Scheduled*

- **Stickiness**
  - Client is always redirected to the same instance. 
  - Supported on CLB and ALB, under *target group*
  - Uses a Cookie with expiration date

- **Cross Zone LB**
  - Each LB in an AZ evenly distributes traffic across instances in *all* zones
  - ALB (Default, no inter AZ data cost)
  - NLB (Disabled, inter AZ cost)
  - CLB (Consle : Default, CLI : Disabled, no inter AZ cost)

- **SSL Certs**
  - In-Flight Encryption
  - TLS : Transport Layer Security 
    - Newer and used but typically refered to as SSL
  - SSL : Secure Sockets Layer
  - Public SSL certificate issued by Certifate Authority (CA)
  - Expire and must be reset
  - Load Balancer can offload SSL
  - Load Balancer used an X.509 certificate (TLS Server Certificate)
  - Manage Certificated using ACM (AWS Certifcate Manager)
  - Can upload your own certs
  - HTTPS Listener:
    - Specify default cert
    - Optional certs to support multiple domains
  - **SNI (Server Name Indication)**
    - Solves the problem of multiple TLS certificated on a web server (serving multiple sites)
    - Newer protocol requires client to *indicate* hostname of the target server in SSL handshake
    - Supported only on ALB, NLB and CloudFront

- **Connection Draining**
  - CLB : Connection Draining
  - ALB & NLB : Target Group - Deregistration Delay
  - Time to complete in-flight requests
  - 1 to 3600 seconds, default is 300 sec

- **Other**
  - Scaling is not instance, needs to "Warm Up"

- **TShoot**
  - 4xx errors : Client
  - 5xx errors : Application
  - LB 503 : at capcity or no registered target
  - If LB cannot connect to application check SG 
- **Monitoring**
  - ELB Access Logs
  - CloudWatch Metrics