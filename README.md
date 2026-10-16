Project Overview

This project demonstrates the deployment of the VProfile multi-tier Java web application on Amazon Web Services (AWS). The architecture uses AWS services for application hosting, traffic distribution, database management, caching, message brokering, content delivery, and monitoring.

Deploy the VProfile Java web application on AWS using a scalable, load-balanced architecture with managed database services, caching, messaging, content delivery, and monitoring.


The application is deployed using AWS Elastic Beanstalk with Apache Tomcat, with an Application Load Balancer and EC2 instances to support scalable application hosting.<img width="769" height="625" alt="image" src="https://github.com/user-attachments/assets/89bb28ae-2bea-4516-8869-d720db9ae3e9" />

The architecture illustrates the request flow from users through Amazon Route 53 and Amazon CloudFront to the application hosted on AWS Elastic Beanstalk.

The application communicates with supporting services, including Amazon RDS for MySQL, Memcached, and Amazon MQ. Amazon S3 stores application artifacts, while Amazon CloudWatch provides monitoring and logging capabilities.

AWS Services Used

AWS Service

Purpose

Amazon Route 53

DNS routing for the application domain

Amazon CloudFront

Content delivery and edge caching

AWS Elastic Beanstalk

Application deployment and environment management

Amazon EC2

Compute instances running the application

Application Load Balancer

Distributes incoming traffic across application instances

Auto Scaling

Adjusts the number of EC2 instances based on configured policies

Amazon RDS for MySQL

Managed relational database

Memcached

Caching to reduce repeated database queries where supported

Amazon MQ

Message brokering between application components

Amazon S3

Stores application deployment artifacts

Amazon CloudWatch

Monitoring, metrics, logs, and alarms

Technology Stack

Application: VProfile Java web application

Application Server: Apache Tomcat

Cloud Platform: Amazon Web Services (AWS)

Compute: Amazon EC2

Deployment: AWS Elastic Beanstalk

Database: MySQL on Amazon RDS

Caching: Memcached

Messaging: Amazon MQ

Networking and Content Delivery: Route 53, CloudFront, Application Load Balancer

Monitoring: Amazon CloudWatch

Artifact Storage: Amazon S3

Architecture Workflow

Users access the application through its configured domain name.

Amazon Route 53 resolves the domain to the configured endpoint.

Amazon CloudFront handles content delivery according to its distribution and origin configuration.

Requests reach the application through the configured Elastic Beanstalk environment and Application Load Balancer.

EC2 instances run the Java application using Apache Tomcat.

The application connects to Amazon RDS for MySQL to store and retrieve application data.

Memcached provides caching for supported application operations.

Amazon MQ supports asynchronous communication between components where configured.

Amazon S3 stores application deployment artifacts.

Amazon CloudWatch collects relevant metrics and logs for monitoring and troubleshooting.

Note: The actual request flow depends on the configured CloudFront origin, routing rules, and application settings.

Project Implementation

1. Application Deployment

Prepared the VProfile application package for deployment.

Configured an AWS Elastic Beanstalk environment for the Java application.

Deployed the application to the Apache Tomcat environment.

Verified application availability using the configured endpoint.

2. Load Balancing and Scaling

Configured an Application Load Balancer for incoming application traffic.

Used EC2 instances to host the application.

Configured Auto Scaling policies where required.

Validated the health of registered targets and application instances.

3. Database Integration

Configured Amazon RDS for MySQL.

Established database connectivity from the application environment.

Configured database connection parameters securely.

Verified database connectivity and application functionality.

4. Caching and Messaging

Configured Memcached for supported caching operations.

Integrated Amazon MQ for message-broker communication.

Verified connectivity between the application and the required supporting services.

5. Content Delivery and Artifact Storage

Configured Amazon Route 53 for DNS resolution.

Used Amazon CloudFront for content delivery.

Stored application deployment artifacts in Amazon S3.

6. Monitoring and Troubleshooting

Used Amazon CloudWatch to monitor available infrastructure metrics and application logs.

Checked Elastic Beanstalk environment health.

Investigated application deployment and connectivity issues using logs and service metrics.

Security Considerations

Restricted database access to authorized application resources.

Used security groups to control inbound and outbound network traffic.

Applied least-privilege IAM permissions wherever possible.

Avoided storing database credentials and AWS access keys in source code.

Used HTTPS where configured and ensured sensitive application settings were protected.

Reviewed access permissions for S3 buckets and other AWS resources.
