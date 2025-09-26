📌 Project Objective :-
=
The objective of this project is to design and deploy a highly available, scalable, and fault-tolerant web application architecture on AWS. It ensures seamless access for users over the internet while leveraging AWS managed services for compute, storage, and database needs.

📌 Project Description:-
=
This setup uses Amazon Route 53, Application Load Balancer, Auto Scaling, Amazon Aurora, and Amazon EFS to provide a cloud-native architecture.

Users access the application via the internet.

Amazon Route 53 manages DNS routing.

Traffic is directed through an Application Load Balancer to application instances running in private subnets.

Auto Scaling Groups ensure that the application layer can handle variable loads.

Amazon Aurora serves as a managed, high-performance relational database.

Amazon EFS provides shared file storage accessible across multiple availability zones.

The architecture spans multiple Availability Zones for redundancy and high availability.

📌 Architecture Flow :-
=
1.Users → Internet → Route 53: Users request the application via the internet, and Amazon Route 53 resolves the domain name to the Load Balancer.

2.Route 53 → Internet Gateway → Application Load Balancer (ALB): The ALB routes traffic to EC2 instances in the private subnets.

3.EC2 Instances (App Layer) in Auto Scaling Group: The application servers run inside private subnets, scaling automatically based on demand.

4.Data Layer (Amazon Aurora & EFS):

Application servers store relational data in Amazon Aurora.

Shared files are stored in Amazon EFS, mounted across instances.

5.High Availability & Fault Tolerance: Resources are distributed across multiple Availability Zones.
<img width="562" height="525" alt="image" src="https://github.com/user-attachments/assets/770b7757-33e1-4b5f-ac27-ee0196c89cd9" />

📌 Benefits of This Setup:-
=
✅High Availability:-Multiple Availability Zones ensure the application remains available even if one zone fails.

✅Scalability:-Auto Scaling Groups dynamically adjust the number of EC2 instances based on load.

✅Fault Tolerance:-Redundant architecture design with ALB, Aurora (multi-AZ), and EFS ensures minimal downtime.

✅Security:-Applications run in private subnets while only the Load Balancer and NAT gateways face the internet.

✅Performance & Reliability:-Aurora provides high-performance database operations.Route 53 ensures fast and reliable DNS resolution.

✅Cost Efficiency:-Pay-as-you-go model with scaling ensures cost optimization.
