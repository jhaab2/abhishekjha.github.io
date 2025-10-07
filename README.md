# abhishekjha.github.io

flowchart TB
  subgraph Internet
    User[(User)]
  end

  User -->|DNS query| R53[/"Route 53 (Failover)"/]
  R53 -->|Primary healthy ->| ALB_WE_PRIMARY[/"ALB - Web (us-east-1)"/]
  R53 -->|If primary down ->| ALB_WE_DR[/"ALB - Web (ap-south-1) (DR)"/]

  subgraph "us-east-1 (Primary)"
    direction TB
    ALB_WE_PRIMARY --> ECS_WEB_PRIMARY["ECS Web Server (Fargate/EC2)"]
    ECS_WEB_PRIMARY --> ALB_BE_PRIMARY[/"ALB - Backend UI (internal)"/]
    ALB_BE_PRIMARY --> ECS_BE_PRIMARY["ECS Backend UI"]
    ECS_WEB_PRIMARY --> S3_PRIMARY[(S3 - assets)]
    ECS_BE_PRIMARY --> RDS_PRIMARY[(RDS / DB)]
    ECS_BE_PRIMARY --> ECR_PRIMARY[(ECR / images)]
  end

  subgraph "ap-south-1 (DR, Passive)"
    direction TB
    ALB_WE_DR --> ECS_WEB_DR["ECS Web Server (warm or scaled-down)"]
    ECS_WEB_DR --> ALB_BE_DR[/"ALB - Backend UI (internal)"/]
    ALB_BE_DR --> ECS_BE_DR["ECS Backend UI"]
    ECS_WEB_DR --> S3_DR[(S3 - replicated assets)]
    ECS_BE_DR --> RDS_DR[(RDS - cross-region replica)]
    ECS_BE_DR --> ECR_DR[(ECR - replicated images)]
  end

  %% Data replication arrows
  RDS_PRIMARY ---|async replication| RDS_DR
  S3_PRIMARY ---|CRR| S3_DR
  ECR_PRIMARY ---|replication| ECR_DR

  style R53 stroke:#333,stroke-width:1px
  classDef awsIcon fill:#FFF,stroke:#E67E22,stroke-width:2px
  class ALB_WE_PRIMARY,ALB_WE_DR,ALB_BE_PRIMARY,ALB_BE_DR awsIcon
