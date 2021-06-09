# ELB
## Summary
- **ELB** : Elastic Load Balancers
  - **CLB** : Classic Load Balancer
    - HTTP,HTTPS,TCP
  - **ALB** : Application Load Balancer
    - HTTP,HTTPS,WebSocket
  - **NLB** : Network Load Balancer
    - TCP,TLS (Secure TCP) & UDP
  - **GLB** : Gateway Load Balancer
    - *To be added not in exam scope atm*

- **ASG** : Auto Scaling Groups

- **Health Checks**

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
  - **NLB** : Network Load Balancer
    - v2
    - TCP,TLS (Secure TCP) & UDP

  - **Health Checks**
    - Specify Port and Route (*/health* is common)
    - If response is not 200 then the instnace is unhealthy

  - **Security Groups**
    - **Application Security Gruop** - On EC2, reference the LB SG


- **ASG** : Auto Scaling Groups
  - **Scaling Policies**



- **Stickiness**
- **Cross Zone LB**
- **SSL Certs**
- **Connection Draining**
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


