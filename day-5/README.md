 # AWS Security using Security Groups and NACL 

AWS (Amazon Web Services) provides multiple layers of security to protect resources and data within its cloud infrastructure. Two important components for network security in AWS are Security Groups and Network Access Control Lists (NACLs). Let's explore how each of them works:

    Security Groups:
        Security Groups act as virtual firewalls for Amazon EC2 instances (virtual servers) at the instance level. They control inbound and outbound traffic by allowing or denying specific protocols, ports, and IP addresses.
        Each EC2 instance can be associated with one or more security groups, and each security group consists of inbound and outbound rules.
        Inbound rules determine the traffic that is allowed to reach the EC2 instance, whereas outbound rules control the traffic leaving the instance.
        Security Groups can be configured using IP addresses, CIDR blocks, security group IDs, or DNS names to specify the source or destination of the traffic.
        They operate at the instance level and evaluate the rules before allowing traffic to reach the instance.
        Security Groups are stateful, meaning that if an inbound rule allows traffic, the corresponding outbound traffic is automatically allowed, and vice versa.
        Changes made to security group rules take effect immediately.

    Network Access Control Lists (NACLs):
        NACLs are an additional layer of security that operates at the subnet level. They act as stateless traffic filters for inbound and outbound traffic at the subnet boundary.
        Unlike Security Groups, NACLs are associated with subnets, and each subnet can have only one NACL. However, multiple subnets can share the same NACL.
        NACLs consist of a numbered list of rules (numbered in ascending order) that are evaluated in order from lowest to highest.
        Each rule in the NACL includes a rule number, protocol, rule action (allow or deny), source or destination IP address range, port range, and ICMP (Internet Control Message Protocol) type.
        NACL rules can be configured to allow or deny specific types of traffic based on the defined criteria.
        They are stateless, which means that if an inbound rule allows traffic, the corresponding outbound traffic must be explicitly allowed using a separate outbound rule.
        Changes made to NACL rules may take some time to propagate to all the resources using the associated subnet.

## Project Implemented in the video


![Screenshot 2023-06-29 at 12 14 32 AM](https://github.com/iam-veeramalla/aws-devops-zero-to-hero/assets/43399466/30bbc9e8-6502-438b-8adf-ece8b81edce9)






--------

one VPC have public subnet and private subnet 
In AWS, a Virtual Private Cloud (VPC) can be configured to have both public and private subnets.
Within the public subnet, resources can be directly accessed from the internet, while the private subnet contains resources that are not directly accessible from the internet.
To facilitate communication between the public and private subnets, the public subnet is associated with an Internet Gateway that allows outbound traffic to the internet and inbound traffic from the internet to resources within the public subnet.
In contrast, the private subnet typically uses a NAT Gateway to enable outbound internet access for resources without exposing them directly to the internet.
Additionally, the NAT Gateway allows instances in the private subnet to initiate outbound traffic to the internet while preventing unsolicited inbound traffic from reaching those instances.
which serves as a bridge between the public subnet and the internet. This setup enhances security by isolating sensitive resources in the private subnet while still allowing necessary outbound connections to the internet.
and a NAT Gateway that facilitates secure communication between the public and private subnets.


when user want to acess application of incide VPC then it first goes inside the internet gatway. after that devos will place loadbalancer,
loadbalancer will talk with the outer world to talk with the private subnet. loadbalncer have acce to private subnet and private subnet have access to loadbalcer
which sits behind the Internet Gateway
and distribute incoming traffic to the instances running in the private subnet. This architecture ensures that the application remains secure while providing high availability and scalability.
Furthermore, the load balancer can perform health checks on the instances in the private subnet, ensuring that only healthy instances receive traffic. This setup allows for seamless scaling of the application as demand increases, while also maintaining the security of sensitive resources by keeping them isolated from direct internet access.

NACL
When configuring NACLs for the VPC, it is important to define rules that allow necessary traffic while denying unwanted access. For example, inbound rules should permit traffic from the load balancer to the instances in the private subnet, while outbound rules should allow the instances to communicate with the internet through the NAT Gateway. This ensures that the application can function properly without compromising security.

Additionally, it is crucial to monitor and audit the NACL rules regularly to ensure they align with the security policies and traffic requirements of the application. Proper documentation of the rules and their intended purposes can help in troubleshooting and maintaining the security posture of the VPC.


______

be default EC2 will give default VPC
which includes a default subnet in each Availability Zone, enabling users to quickly launch instances without additional configuration.
Users can customize the default VPC by adding or removing subnets, configuring security groups, and modifying route tables to meet their specific networking needs.
Inboud and outBoud trafic   rules are defined in the associated security groups and NACLs, determining which traffic is allowed or denied based on specified criteria.
outbound only restrict port 25 
Port 25 is restricted by default to prevent spam and abuse
for instances in the default VPC. Users can request the removal of this restriction by submitting a request to AWS support, allowing them to send email traffic through port


NACL: network access control list
A Network Access Control List (NACL) is a virtual firewall that controls inbound and outbound traffic at the subnet level within a VPC. It provides an additional layer of security by allowing users to define rules that specify which traffic is permitted
security level applied to EC2 level where as NACL applied at the subnet level.
with NACL devops play critical role



each subnet layer you can define security rules to control access and manage traffic flow, you can add whihc to have access or not 
if 50 ec2 instance for some instances, you may need to implement specific NACL rules to manage traffic effectively and ensure secure communication between them.
security group only have allowing con nections, whereas NACLs can explicitly allow or deny both inbound and outbound traffic.





------ chatgpt


 VPC with Public and Private Subnets
In AWS, a VPC (Virtual Private Cloud) can have two types of subnets:

Public Subnet: Resources here can directly connect to the internet.

Private Subnet: Resources here cannot be accessed directly from the internet.

🌐 How they connect to the internet:
The Public Subnet is connected to an Internet Gateway, which allows:

Outbound traffic (from subnet to internet)

Inbound traffic (from internet to public resources)

The Private Subnet connects to the internet using a NAT Gateway placed in the public subnet. This allows:

Outbound internet access from private instances (e.g., to download updates)

No inbound internet traffic (more secure)

⚖️ Load Balancer and Internet Access
When a user tries to access your application:

The request goes through the Internet Gateway.

Then it hits a Load Balancer placed in the public subnet.

The Load Balancer:

Has access to the private subnet.

Sends traffic to EC2 instances running inside the private subnet.

This setup makes your app:

Secure (because the actual EC2 instances are not exposed to the internet)

Scalable & Reliable (Load Balancer checks health and distributes traffic evenly)

🔐 NACL (Network Access Control List)
NACL is like a firewall for each subnet (not for each instance).

It controls both inbound and outbound traffic.

You can allow or deny specific traffic by adding rules.

Example:
Allow traffic from Load Balancer to private subnet

Allow private subnet to talk to the internet via NAT Gateway

Security groups only allow, but NACLs can allow or deny traffic.

Pro Tip:
Always monitor and audit your NACL rules.

Document what each rule is for — helps with troubleshooting and security.

🧰 Default VPC
When you create an AWS account, a default VPC is provided.

It includes:

One subnet in each Availability Zone (AZ)

Internet access setup

Security groups and route tables

Port 25 restriction:
By default, outbound traffic on port 25 (SMTP for email) is blocked to prevent spam.

You can request AWS to unblock it if you need it.

🔐 Security Group vs NACL

Feature	Security Group	NACL
Level	EC2 Instance	Subnet
Default Action	Allow only	Allow or Deny
Use Case	Protect specific EC2s	Control subnet-level traffic
Direction	Stateful	Stateless
DevOps engineers use NACLs to fine-tune subnet-level traffic control, especially in complex environments with many EC2s.