# Infrastructure

## AWS Zones
* **Main site:** us-east-2 (Ohio)
* **DR site:** us-west-1 (N. California)

## Servers and Clusters

### Table 1.1 Summary

| Asset | Purpose | Size | Qty | DR |
|-------|---------|------|-----|----|
| EC2 | Running the web application | t3.micro | 3 | Replicated in us-east-2 |
| EC2 | Running the web application | t3.micro | 3 | Deployed to DR in us-west-1 |
| EKS | For monitoring stack | t3.medium | 2 nodes | Replicated in us-east-2 |
| EKS | For monitoring stack | t3.medium | 2 nodes | Deployed to DR in us-west-1 |
| VPC | Virtual private network | 3 subnets | 1 | Created in multiple locations us-east-2a, us-east-2b, us-east-2c |
| VPC | Virtual private network | 2 subnets | 1 | Created in multiple locations us-west-1a, us-west-1b |
| ALB | Load balancer for the web application | N/A | 1 | Created in us-east-2 |
| ALB | Load balancer for the web application | N/A | 1 |Created in us-west-1 |
| RDS | Backend database running for the web application | db.t3.micro | 2 | Replicated in us-east-2 |
| RDS | Backend database running for the web application | db.t3.micro | 2 | Deployed to DR in us-west-1 |
| GitHub | Repo for storing the Terraform Code | N/A | 1 | GitHub |

### Descriptions

* **EC2:** Virtual machines for the web application. Three instances on each region in us-east-2 and us-west-1.
* **EKS:** Elastic Kubernetes Service, Managed Kubernetes for deploying monitoring platform on each region us-east-2 and us-west-1.
* **VPC:** Virtual Private Cloud, networking for EC2 instances. Three IP in us-east-2 on each Availability Zones, and two IP in us-west-1 on each Availability Zones.
* **ALB:** Application Load Balancer on each region. Used to distribute incoming traffic accross to EC2 instances.
* **RDS:** Relational Database Service, primary and secondary clusters. Primary RDS Cluster in us-east-2 use geo-replication to Secondary RDS Cluster in us-west-1.
* **GitHub:** Git repository for versioning and store Terraform Code.

## DR Plan
### Pre-Steps:

1. Ensure the infranstructure is set up and working in the DR site.

## Steps:

1. Point DNS to us-west-1 region.
    * This can be done with a name provider in Amazon route 53.
2. Failover database replication instances to us-west-1.
    * Manually force secondary region to become primary at the database level.
    * Automatically failover the database by health checks.
