###<TASK><br>
--Task is to install two linux machines in which one is public and other is private.Now i have to access this linux machine from public to private through ssh.<br>

--Task Overview<br>
*You need:<br>
*Two Linux EC2 instances:<br>
*Public instance → Has public IP and internet access.<br>
*Private instance → No public IP, only private subnet.<br>
*Goal: SSH into the private instance from the public one.<br>

###<What is the task?><br>

*You were asked to create two Linux machines (typically AWS EC2 instances) so that:<br>

*One machine is public (it has a public IP and you can reach it directly from your laptop).<br>

*The other is private (no public IP — not directly reachable from the internet).<br>

*Then you must use the public machine as a bridge to access the private machine by SSHing (logging in securely) from the public machine into the private machine.<br>

###<Why do we do this?><br>
*Think of a house with a guarded inner room:<br>

*The public machine is like the gatehouse — it faces the street and has a guard who checks visitors.<br>

*The private machine is the inner room — only people who got in via the gatehouse and have permission can enter.<br>

##<Why this is useful:><br>

*Security. The private machine is not exposed to the whole internet — fewer attack paths.<br>

*Controlled access. You let only trusted machines (the public/bastion host) talk to the private one.<br>

*Common in real systems. Production databases, internal services often live in private subnets.<br>

##<High-level steps (what you will do)><br>

*Launch a public EC2 instance (bastion/jump host) in a public subnet with a public IP.<br>

*Launch a private EC2 instance in a private subnet (no public IP).<br>

*Set up security groups (firewall rules):<br>

*Public SG: allow SSH (port 22) from your laptop IP.<br>

*Private SG: allow SSH only from the public instance (or the public instance’s security group).<br>

*From your laptop: SSH into the public instance.<br>

*From the public instance: SSH into the private instance.<br>

###<STEPS:><br>
--Step 1: Create the Network (VPC)<br>
*A VPC (Virtual Private Cloud) is your private network inside AWS.<br>
*Steps:<br>
*Go to AWS Console → VPC → Your VPCs<br>
*Click Create VPC<br>
*Choose:<br>
*Resources to create: VPC only<br>
*Name: my-vpc<br>
*IPv4 CIDR block: 10.0.0.0/16<br>
*Leave others default<br>
→ Click Create VPC<br>
=>You now have a virtual network with range 10.0.0.0 - 10.0.255.255<br>

--step 2:Create Two Subnets<br>
Subnets divide your VPC into smaller networks.<br>
Public Subnet: connects to the internet<br>
Private Subnet: internal-only<br>
Steps:<br>
Go to VPC → Subnets → Create Subnet<br>
Select your VPC: my-vpc<br>
Create two subnets:<br>

Name	        Availability Zone	            IPv4 CIDR	      Purpose
public-subnet	choose ap-south-1a (example)	10.0.1.0/24	      public
private-subnet	choose ap-south-1a	            10.0.2.0/24	      private

=>You now have one public and one private subnet.<br>

--Step 3: Create and Attach Internet Gateway<br>

An Internet Gateway (IGW) allows internet access for public subnet.<br>

Steps:<br>

Go to VPC → Internet Gateways<br>

Click Create Internet Gateway<br>

Name: my-igw<br>
After creation, select it → Click Actions → Attach to VPC<br>

Choose my-vpc<br>

=>Now your VPC is connected to the internet.<br>

--Step 4: Configure Route Tables<br>
*This decides which subnet can reach the internet.<br>
Steps:<br>
a.Go to VPC → Route Tables<br>
b.There should already be one route table for your VPC.<br>
c.Rename it to public-route-table.<br>
d.Select public-route-table → Go to Routes tab → Edit routes<br>
e.Add new route:<br>
f.Destination: 0.0.0.0/0<br>
g.Target: your Internet Gateway (my-igw)<br>
h.Save changes.<br>
i.Go to Subnet associations → Edit subnet associations<br>
j.Select public-subnet and save.<br>
=>Now only the public subnet has internet access.<br>

--Step 5: Create Security Groups<br>
*Security groups = firewalls that control what traffic is allowed.<br>
*We’ll create two:<br>
->>Public Security Group<br>
a.Go to EC2 → Security Groups → Create Security Group<br>
b.Name: public-sg<br>
c.VPC: my-vpc<br>
d.Inbound rules:<br>
e.Type: SSH<br>
f.Port: 22<br>
g.Source: My IP (so only my local system can SSH)<br>
h.Outbound rules: leave default (Allow all)<br>
=> public-sg created.<br>
->> Private Security Group<br>
Create another one:<br>
a.Name: private-sg<br>
b.VPC: my-vpc<br>
c.Inbound rules:<br>
d.Type: SSH<br>
e.Port: 22<br>
f.Source: public-sg (you can choose from dropdown)<br>
g.Outbound rules: leave default<br>
=> private-sg created.<br>

--Step 6: Launch EC2 Instances<br>
*Public EC2 Instance<br>
a.Go to EC2 → Instances → Launch Instance<br>
b.Name: public-ec2<br>
c.AMI: Amazon Linux 2 (free tier eligible) or Ubuntu<br>
d.Instance type: t3.micro<br>
e.Key pair: choose or create one (download .pem file)<br>
f.Network settings:<br>
VPC: my-vpc<br>
Subnet: public-subnet<br>
Auto-assign public IP: Enable<br>
Security group: public-sg<br>
Launch instance.<br>
Wait until the instance is Running.<br>
=>You’ll get a Public IPv4 address like 3.110.45.22.<br>
*similarly create private instance with private-sg.<br>

--step 8 (connecting local system to public ec2 instance):<br>
*cd downloads<br>
*chmod 400 mykey.pem<br>
*ssh -i mykey.pem ubuntu@<PUBLIC_IP><br>
=>You are now inside your public EC2 machine.<br>

--step 9: connect from public instance to private instance<br>
*scp -i mykey.pem mykey.pem ec2-user@<PUBLIC_IP>:/home/ec2-user/<br>
*ssh -i mykey.pem ubuntu@<PRIVATE_IP><br>

