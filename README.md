# Architecting a Simple but Resilient App on AWS: ECS, Multi-AZ Deployment, and Microservice Architecture



<img width="221" height="616" alt="Diag-1 0" src="https://github.com/user-attachments/assets/de4b44ac-72af-46f0-8637-501b8437917d" />



1. Route 53
   Route 53 is s designed for extremely high availability and is inherently resilient, as it is a global, distributed service utilizing an Anycast network.
   
3. ALB WEB
   Route 53 will redirect incoming request to WEB ALB. It will accept incoming https request, and will forward it to ECS tasks running Apache webserver.
   
5. ECS WEB
   Two replicas of Apache 2.4 tasks would be running to server the incoming request. Replicas will be running in different AZs to provide resilience to the ECS Service. It will forward incoming request to the ALB UI service. 
   
7. ALB UI
   Thie is the entrypoint for backend UI service, running inside ECS cluster.
   
9. ECS UI
   It's the second ECS cluster hosting two replicas of tasks hosting React App.
