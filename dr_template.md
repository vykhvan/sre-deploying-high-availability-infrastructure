# Infrastructure

## AWS Zones
* **us-east-2**  us-east-2a, us-east-2b, us-east-2c
* **us-west-1**  us-west-1a, us-west-1c

## Servers and Clusters

### Table 1.1 Summary

| Asset | Purpose | Size | Qty | DR |
|-------|---------|------|-----|----|
| EC2 | Running the web application | t3.micro | 3 | Replicated in us-east-2 |
| EC2 | Running the web application | t3.micro | 3 | Deployed to DR in us-west-1 |
| EKS | For monitoring stack | t3.medium | 1 cluster, 2 nodes | Replicated in us-east-2 |
| EKS | For monitoring stack | t3.medium | 1 cluster, 2 nodes | Deployed to DR in us-west-1 |
| VPC | Virtual private network | 3 subnets | 1 | Created in multiple locations us-east-2a, us-east-2b, us-east-2c |
| VPC | Virtual private network | 2 subnets | 1 | Created in multiple locations us-west-1a, us-west-1b |
| ALB | Load balancer for the web application | N/A | 1 | Created in us-east-2 |
| ALB | Load balancer for the web application | N/A | 1 |Created in us-west-1 |
| RDS | Backend database running for the web application | db.t3.micro | 1 cluster, 2 nodes | Replicated in us-east-2 |
| RDS | Backend database running for the web application | db.t3.micro | 1 cluster, 2 nodes | Deployed to DR in us-west-1 |
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

1. Create a cloud load balancer and point DNS to the load balancer. This way you can have multiple instances behind 1 IP in a region. During a failover scenario, you would fail over the single DNS entry at your DNS provider to point to the DR site. This is much more intelligent than pointing to a single instance of a web server.
2. Have a replicated database and perform a failover on the database. While a backup is good and necessary, it is time-consuming to restore from backup. In this DR step, you would have already configured replication and would perform the database failover. Ideally, your application would be using a generic CNAME DNS record and would just connect to the DR instance of the database.