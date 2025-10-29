1. ##**VPC(Virtual private cloud)** <br>
 #*WHY VPC*:
 *Let there be a region(mumbai,usa etc) and there are companies X,Y,Z.<br>
 *These companies initially maintained their own data centers and now they cant afford 
  the datacenters as the require high maintainance.<br>
  *AWS as a cloud platform supported them with building datacenters at their own regions.<br>
  *AWS hosts their applications as per their requirements(resources) for which they costed        
   accordingly.<br>
  *These companies shared the resources from same server provided by AWS.<br>
  *previously this went fine and AWS provided vm's to the organizations as per the requirements.<br>
  *Around 2013/2014,a problem arised by companies who seems to be unethical.<br>
  *The problem is while the companies can share the resoures from a single server,the security  
   for their data comes in risk.<br>
  *To avoid this risk of hacking of organizations data,AWS has come up with VPC which is maintained 
   by devops engineer.<br>

  #*WHAT IS VPC*:<br>
  *VPC is called as Virtual private cloud.<br>
  *Whenever there is a requirement of vm,devops engineer requests AWS for a VPC at a particular 
   region.<br>
  *A VPC (Virtual Private Cloud) in AWS is basically your own private network inside the AWS cloud.<br>
  *Think of it as creating your own mini data center inside AWS where you have full control over:<br>
        -how your network is structured,<br>
        -which systems can talk to each other, and<br>
        -which ones are exposed to the internet.<br>
  *Simply,when you create a VPC, AWS gives you a virtual network — like a private LAN — that’s  
   isolated from other users’ networks in AWS.<br>
  #*KEY COMPONENTS*:<br>
        1.Subnets:<br>
            -Divide your VPC into smaller sections (like rooms in a house).<br>
            -Public Subnet → Connected to the internet.<br>
            -Private Subnet → Not connected to the internet (for backend services).<br>
        2.Internet Gateway (IGW):<br>
            -Lets your VPC connect to the internet.<br>
            -Needed if you want your EC2 instances to be publicly accessible.<br>
        3.Route Tables: <br>
            -Define where traffic goes (for example, to the internet, or within VPC).<br>
        4.NAT Gateway:<br>
            -Allows private subnet instances to access the internet (e.g., for updates) without being 
             accessible from outside.<br>
        5.Security Groups:<br>
            -Act like firewalls that control inbound/outbound traffic to your instances.<br>
        6.Network ACLs (Access Control Lists):<br>
            -Another layer of network security that works at the subnet level.<br>
        
2. ##**Core Components of Amazon VPC(In the Architecture)**<br>
The following are the components of Amazon VPC:<br>
1. VPC<br>
-Virtual Private Cloud is a logically isolated section of the AWS cloud where resources can be launched, secured, and interconnected<br>
-Each VPC is defined by an IPv4 CIDR block (for example, 10.0.0.0/16), representing a user-defined address space of up to 65,536 IP addresses. This virtual network mirrors an on-premises data center but inherits the elasticity, scalability, and resilience of AWS’s global infrastructure.<br>
2. Subnets<br>
-A Subnet is a segment of a VPC’s IP address range that resides entirely within a single Availability Zone (AZ).<br>
Subnets serve two primary purposes:<br>
-Segmentation: They divide large networks into smaller, isolated zones for better organization and control.<br>
-High Availability: Distributing subnets across multiple AZs ensures fault tolerance and operational continuity.<br>
-Subnets are typically categorized as:<br>
    --Public Subnets, which route internet-bound traffic through an Internet Gateway (IGW).<br>
    --Private Subnets, which are isolated from direct internet access and communicate externally through a NAT Gateway.<br>
-This distinction forms the foundation of secure, multi-tier cloud architectures.<br>
3. Route Tables<br>
-EachVPC contains an implicit virtual router that relies on Route Tables to direct traffic. Every subnet must be associated with exactly one route table, and each route defines:<br>
-A destination CIDR block (where the traffic is headed).<br>
-A target (where the traffic should be sent, such as an Internet Gateway, NAT Gateway, or another instance).<br>
Route tables determine whether traffic remains internal to the VPC or is sent to external networks. They are the digital roadmap of the cloud network.<br>
4. Network Access Control Lists<br>
-Network Access Control Lists (NACLs) are stateless firewalls that govern inbound and outbound traffic at the subnet level. Each VPC comes with a default NACL, which can be modified but not deleted. Unlike Security Groups, NACLs evaluate each packet individually and require explicit rules for both inbound and outbound flows. They are typically used to implement broad subnet-level controls, such as blocking specific IP ranges or enforcing compliance requirements across multiple instances.<br>
5. Internet Gateway(IGW)<br>
-An Internet Gateway is a horizontally scaled, redundant component that enables bidirectional communication between public subnets and the internet. By attaching an IGW to a VPC and creating a route (e.g., 0.0.0.0/0) to it in a public subnet’s route table, instances within that subnet can send and receive traffic from the internet. A VPC can have only one attached Internet Gateway at a time. Without an IGW, no instance within the VPC can directly communicate with the public internet.<br>
6. Network Address Translation (NAT)<br>
-NAT Gateways allow instances in private subnets to initiate outbound internet connections such as downloading updates or accessing APIs while preventing unsolicited inbound connections. This enables backend systems to communicate securely with external services without compromising isolation. In most architectures, NAT Gateways are deployed in public subnets, serving as controlled egress points for private subnets.<br>
7. Security Groups<br>
-While NACLs operate at the subnet level, Security Groups act as stateful firewalls at the instance level. They define granular inbound and outbound rules based on IP ranges, ports, and protocols. Statefulness means that if an instance initiates an outbound connection, the return traffic is automatically allowed simplifying rule management. Security Groups form the first layer of defense for EC2 instances, RDS databases, and other VPC resources.<br>
8. Classless Inter-Domain Routing (CIDR):<br>
-CIDR notation defines the IP range of a VPC or subnet, using syntax such as 10.0.0.0/16.<br>
-It provides flexibility in assigning and subdividing private address ranges as specified in RFC 1918, which includes:<br>
    10.0.0.0 - 10.255.255.255 (10/8)<br>
    172.16.0.0 - 172.31.255.255 (172.16/12)<br>
    192.168.0.0 - 192.168.255.255 (192.168/16)<br>
-These address blocks ensure isolation within private networks and avoid conflicts with public IP space<br>

![VPC Diagram](https://media.geeksforgeeks.org/wp-content/uploads/20230414094756/aws-vpc-(1).png "AWS VPC Architecture")

3. ##**PUBLIC INSTANCE AND PRIVATE INSTANCE**<br>
    *In AWS, an instance usually means an EC2 virtual machine.<br>
    *Whether an EC2 instance is public or private depends on how its networking is configured inside your VPC.<br>
    #*Public Instance:*
        *A public instance is one that can be accessed from the internet.
    -Key Features:
        -It’s launched in a Public Subnet.
        -The subnet has a route to the Internet Gateway (IGW).
        -The instance has a Public IP address or Elastic IP.
        -Used for web servers, applications, or anything that needs public access.
    -Example:
        -A web server hosting your website.
        -IP example: 13.234.56.12 (publicly reachable).
    #*private instance:*
        *A private instance is one that cannot be accessed directly from the internet.
    -Key Features:
        -It’s launched in a Private Subnet.
        -The subnet does not have a route to the Internet Gateway.
        -It has only a private IP address (used within the VPC).
        -Used for databases, backend servers, or internal applications.
    -Example:
        -A database server (like MySQL or MongoDB) that your web server connects to internally.
        -IP example: 10.0.1.5 (private, not reachable publicly).



