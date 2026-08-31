# On-Premises to AWS Cloud Migration
## Step 2: Migration Planning Document

### 1. Project Overview

This project demonstrates the planning and execution of a migration from a small on-premises Linux environment to AWS.

The current environment consists of a Linux virtual machine running a Python application, MySQL database, and Nginx web server. The environment was intentionally designed to resemble a traditional small on-premises deployment.

The goal of this phase is to analyze the existing environment, identify its limitations, define the target AWS architecture, and create a migration strategy.

---

## 2. Current On-Premises Environment

The current environment is hosted locally using VirtualBox.

### Current Technology Stack

| Component | Current Technology |
|---|---|
| Virtualization | VirtualBox |
| Operating System | Lubuntu |
| Application | Python |
| Database | MySQL |
| Web Server | Nginx |
| Infrastructure | Single Virtual Machine |

The application and database currently reside within the same virtual machine.

### Current Architecture

```text
                    Local Network
                         |
                         |
                  +-------------+
                  | Lubuntu VM  |
                  | VirtualBox  |
                  +-------------+
                         |
             +-----------+-----------+
             |                       |
        +---------+             +---------+
        |  Nginx  |             |  MySQL  |
        +---------+             +---------+
             |
        +---------+
        | Python  |
        |  App    |
        +---------+
```

---

## 3. Current Environment Challenges

The existing environment works, but several limitations make it difficult to scale, maintain, and recover.

### 3.1 Single Point of Failure

The application, web server, and database depend on a single virtual machine.

If the VM becomes unavailable, the entire application becomes unavailable.

### 3.2 Manual System Maintenance

Operating system updates currently require manual intervention.

This increases the operational burden and creates the possibility of systems becoming outdated.

### 3.3 No Configured Backups

The current environment does not have a configured backup strategy.

A hardware failure, VM corruption, or accidental deletion could result in data loss.

### 3.4 Application and Database Coupling

The application and database currently operate on the same machine.

This creates resource contention and makes independent scaling difficult.

### 3.5 Limited Monitoring

There is currently no centralized monitoring or alerting solution.

Infrastructure problems may therefore only become apparent after the application is affected.

---

## 4. Migration Goals

The migration to AWS will aim to achieve the following:

- Improve application availability
- Separate application and database workloads
- Reduce infrastructure maintenance
- Introduce automated backups
- Improve monitoring and visibility
- Create a more scalable architecture
- Improve security through AWS networking and IAM
- Establish a foundation that can be expanded in the future

---

## 5. Proposed AWS Architecture

The target architecture will separate the web/application layer from the database layer.

The proposed AWS environment will use AWS networking, compute, database, storage, security, and monitoring services.

### Target Architecture

```text
                         Internet
                            |
                            |
                     +--------------+
                     | Application  |
                     | Load Balancer|
                     +--------------+
                            |
                            |
                  +-------------------+
                  |   Application     |
                  |      Layer        |
                  |      EC2         |
                  +-------------------+
                            |
                            |
                     +--------------+
                     |    Managed   |
                     |   Database   |
                     |     RDS     |
                     +--------------+
                            |
                     +--------------+
                     |    Backup    |
                     |   / Storage  |
                     +--------------+

              AWS VPC
```

The final architecture diagram will be created separately and stored in the `diagrams/` directory.

---

## 6. On-Premises to AWS Service Mapping

| Current Component | AWS Target | Purpose |
|---|---|---|
| VirtualBox VM | Amazon EC2 | Cloud compute |
| Lubuntu Linux VM | EC2 Linux instance | Application hosting |
| Nginx | Nginx on EC2 / Load Balancer | Web traffic handling |
| Python application | EC2 | Application runtime |
| MySQL | Amazon RDS for MySQL | Managed relational database |
| Local backups | Amazon S3 / RDS backups | Durable backup storage |
| Local firewall | Security Groups / Network ACLs | Network security |
| Manual monitoring | Amazon CloudWatch | Monitoring and alerting |
| Local networking | Amazon VPC | Isolated AWS network |

---

## 7. Migration Strategy

The migration will follow a phased approach rather than attempting to move the entire environment simultaneously.

### Phase 1 — Assessment

Document the existing application, database, infrastructure, dependencies, configuration, and operational requirements.

Identify:

- Application dependencies
- Database schema and data
- Required ports
- Network requirements
- Storage requirements
- Configuration files
- Environment variables
- Backup requirements

### Phase 2 — AWS Foundation

Create the AWS networking and security foundation.

Planned components include:

- VPC
- Public and private subnets
- Route tables
- Internet Gateway
- Security Groups
- IAM roles

### Phase 3 — Application Migration

Deploy the Python application to an EC2 instance.

Configure:

- Linux operating system
- Python runtime
- Application dependencies
- Nginx
- Application service
- Security Groups

### Phase 4 — Database Migration

Move the MySQL database from the local environment to Amazon RDS for MySQL.

The migration process will include:

1. Export the existing database.
2. Create the RDS database.
3. Import the database data.
4. Configure the application to connect to RDS.
5. Validate application functionality.
6. Verify database integrity.

### Phase 5 — Testing

Validate the migrated environment before considering the migration complete.

Testing will include:

- Application accessibility
- Database connectivity
- CRUD operations
- Network connectivity
- Security Group rules
- Application performance
- Backup functionality
- Monitoring and logging

### Phase 6 — Optimization

After the migration is functional, review the architecture for:

- Cost optimization
- Security improvements
- High availability
- Scalability
- Monitoring
- Operational improvements

---

## 8. Security Considerations

Security will be incorporated into the AWS architecture rather than added after migration.

Key considerations include:

- Use IAM roles instead of embedding AWS credentials in the application.
- Restrict Security Group rules to only required traffic.
- Keep the database inaccessible directly from the public internet.
- Use private networking for internal services where appropriate.
- Avoid using the root AWS account for normal operations.
- Apply the principle of least privilege.
- Protect database credentials and application secrets.
- Enable appropriate logging and monitoring.

---

## 9. Backup and Disaster Recovery

The current environment does not have a configured backup strategy.

The AWS environment will introduce managed backup capabilities.

The migration plan will include:

- Database backups
- Application and configuration backups where appropriate
- Recovery testing
- Documented recovery procedures

The objective is not only to create backups but also to verify that the backups can actually be used for recovery.

---

## 10. Monitoring and Logging

The migrated environment will introduce centralized monitoring.

Amazon CloudWatch will be used to monitor relevant AWS resources and application infrastructure.

Potential monitoring areas include:

- EC2 CPU utilization
- Instance health
- Database metrics
- Application availability
- System logs
- Application logs
- Alerts for abnormal conditions

---

## 11. Migration Success Criteria

The migration will be considered successful when:

- The Python application runs successfully on AWS.
- Users can access the application.
- The application successfully connects to the AWS-hosted database.
- Existing database data is preserved.
- Database backups are configured.
- Network access is appropriately restricted.
- AWS monitoring is operational.
- The original on-premises environment is no longer required for normal application operation.

---

## 12. Expected Outcome

The final environment will demonstrate how a traditional single-server application can be transitioned into a more structured AWS architecture.

The project will demonstrate practical knowledge of:

- Linux administration
- Web server configuration
- Python application deployment
- MySQL
- AWS networking
- Amazon EC2
- Amazon RDS
- Amazon S3
- IAM
- Security Groups
- CloudWatch
- Cloud migration planning
- Infrastructure security
- Backup and recovery planning

This project focuses not only on deploying resources in AWS, but also on understanding **why** each architectural change is being made and how it addresses limitations in the original environment.
