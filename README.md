# VProfile — Multi-Tier Web Application Deployment on AWS

A reference deployment of the VProfile Java web application on Amazon Web Services (AWS). This repository documents a scalable, load-balanced architecture using managed services for compute, database, caching, messaging, content delivery, and monitoring.

<img width="772" height="623" alt="image" src="https://github.com/user-attachments/assets/ca5e564e-be62-4390-acf9-fa5d29d09a76" />

Table of contents

- Project overview
- Architecture
- AWS services used
- Technology stack
- Quick start (high level)
- Deployment checklist
- Configuration
- Monitoring & troubleshooting
- Security considerations
- Contributing
- License

Project overview

This repository demonstrates how to deploy a Java web application (VProfile) on AWS using AWS Elastic Beanstalk (Tomcat), an Application Load Balancer, Amazon RDS (MySQL), caching, messaging, S3 artifact storage, CloudFront and Route 53 for content delivery and DNS, and CloudWatch for monitoring.

Architecture

The deployment follows a multi-tier architecture:

- Users -> Route 53 -> CloudFront (optional CDN) -> Application Load Balancer -> Elastic Beanstalk (EC2 + Tomcat)
- Application tier connects to Amazon RDS (MySQL) for persistence
- Memcached for caching layer (ElastiCache or self-managed)
- Amazon MQ for asynchronous messaging between services
- Amazon S3 stores deployment artifacts and static assets
- CloudWatch collects logs and metrics

If available, open docs/architecture.png for a visual diagram.

AWS services used

| Service | Purpose |
|---|---|
| Amazon Route 53 | DNS routing for the application domain |
| Amazon CloudFront | Content delivery and edge caching (optional) |
| AWS Elastic Beanstalk | Application environment management and deployment (Tomcat) |
| Amazon EC2 | Compute instances serving the application |
| Application Load Balancer | Distributes incoming HTTP(S) traffic across instances |
| Auto Scaling | Scales EC2 instances based on load |
| Amazon RDS (MySQL) | Managed relational database |
| Memcached (ElastiCache or self-managed) | Caching to reduce repeated DB queries |
| Amazon MQ | Message broker for asynchronous communication |
| Amazon S3 | Stores deployment artifacts and static assets |
| Amazon CloudWatch | Monitoring, logs, alarms |

Technology stack

- Application: VProfile Java web application
- Application server: Apache Tomcat
- Cloud: AWS (Elastic Beanstalk, EC2)
- Database: MySQL (Amazon RDS)
- Caching: Memcached (ElastiCache or on-EC2)
- Messaging: Amazon MQ
- CDN/DNS: CloudFront, Route 53
- Monitoring: CloudWatch

Quick start (high level)

Prerequisites

- An AWS account with permissions to create Elastic Beanstalk applications, RDS instances, S3 buckets, IAM roles/policies, and ElastiCache/Amazon MQ resources.
- AWS CLI and EB CLI installed and configured locally with appropriate credentials.
- A built WAR file of the VProfile application.

High-level deployment steps

1. Build the application and produce a WAR file (for Tomcat).
2. Create an S3 bucket to hold application versions (Elastic Beanstalk can use the bucket automatically).
3. Provision an Amazon RDS MySQL instance (private subnet recommended).
4. Configure security groups so the EB environment can reach the RDS instance on the MySQL port.
5. Create an Elastic Beanstalk application and a Tomcat environment; upload the WAR as an application version.
6. Configure environment properties in Elastic Beanstalk for database connection (JDBC URL, username, password stored in EB environment configuration or AWS Secrets Manager).
7. Configure and attach an Application Load Balancer for the environment (EB can create this automatically).
8. Optionally provision ElastiCache (Memcached) and Amazon MQ, and configure application properties to use them.
9. Configure Route 53 records and CloudFront distribution (if using CDN) to point to the load balancer or CloudFront origin.
10. Verify health via the Elastic Beanstalk console and monitor logs with CloudWatch.

Deployment checklist (recommended)

- [ ] Use separate environments for dev/stage/prod
- [ ] Place database in private subnets, application instances in public/private subnets as appropriate
- [ ] Use IAM roles for EC2/Elastic Beanstalk with least privilege
- [ ] Store secrets in AWS Secrets Manager or SSM Parameter Store (do not hard-code credentials)
- [ ] Enable automated backups for RDS and enable Multi-AZ for production
- [ ] Configure HTTPS on the load balancer and redirect HTTP to HTTPS
- [ ] Configure health checks for the ALB and EB environment

Configuration

Important environment configuration keys (example names)

- DB_URL: jdbc:mysql://<rds-endpoint>:3306/vprofile
- DB_USER: vprofile_user
- DB_PASSWORD: (store in Secrets Manager; reference from EB env or use SSM)
- CACHE_HOST: host:port for Memcached
- MQ_BROKER_URL: connection URL for Amazon MQ
- AWS_REGION: AWS region where the resources are deployed

Monitoring & troubleshooting

- Use Elastic Beanstalk environment health and events to inspect immediate deployment issues.
- Stream application logs to CloudWatch Logs (configure EB log streaming or use a sidecar/agent).
- Monitor RDS metrics (CPU, connections, FreeableMemory) and set alarms on critical thresholds.
- Use CloudWatch metrics and ELB target health to scale and diagnose availability issues.

Security considerations

- Limit inbound access to the database and cache to only the application security group(s).
- Use HTTPS/TLS for all external traffic. Terminate TLS at the ALB and use HTTPS between ALB and backend if required.
- Use IAM roles for EC2/Beanstalk and grant least privilege for S3, RDS, MQ, and CloudWatch access.
- Audit S3 bucket permissions and avoid public read/write access unless explicitly required.
- Rotate credentials periodically and use AWS Secrets Manager or SSM Parameter Store for secrets.

Contributing

Contributions, suggestions and improvements are welcome. Please open an issue or create a pull request.

License

Specify your project license here (e.g., MIT, Apache-2.0) or remove this section if you prefer a different license.

Contact

For questions about this deployment or the VProfile application, open an issue in this repository.
