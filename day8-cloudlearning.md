# TASK
About Route53 in AWS.
Architecture of Route53.



# Route53 in AWS

- Amazon Route 53 is AWS’s highly available, scalable, and global Domain Name System (DNS) web service. Its main purpose is to translate human-readable domain names (like _amazon.com_) into IP addresses (like _192.0.2.1_) so that browsers and devices can find your application.

 ## Working of Route53 


- Amazon Route 53 is a highly available and scalable DNS service from AWS. It converts human-readable domain names into IP addresses, enabling seamless internet communication.
## Features of Route53
 **1. Domain Registration & Management:**

Route 53 lets users register new domains or transfer existing ones. After registration, users can easily manage DNS settings such as MX records, CNAMEs, and aliases through a simple interface.

### **2. Global DNS Resolution:**

Route 53 uses a worldwide network of DNS servers placed in multiple regions. When a user enters a domain name, Route 53 quickly returns the matching IP address using its global, low-latency infrastructure, ensuring fast access from anywhere.

### **3. Traffic Routing & Load Balancing:**

Route 53 supports advanced traffic-routing methods like weighted routing and latency-based routing. These allow users to distribute traffic across multiple endpoints such as EC2 instances, load balancers, or external servers and make sure to keep applications available even during outages.
## Supported Record types by Route53

####  1. A Record (Address Record):

- Maps a domain or subdomain to an "IPv4 address".

#### 2. **AAAA Record (IPv6 Address Record):**

- Same as A record but maps the domain to an "IPv6 address".

#### 3. **CNAME Record (Canonical Name Record):**

- Creates an alias that points one domain name to another. Commonly used for subdomains or when multiple domains need to point to the same server.

#### 4. **MX Record (Mail Exchange Record):**

- Specifies the mail servers responsible for receiving emails for a domain. Used for "email routing". 

## Use cases of Route53/Benefits of Route53
### **1. High Availability & Reliability**

- Route 53 provides a highly available DNS service using a globally distributed network of DNS servers, ensuring fast and accurate DNS resolutions.

### **2. Scalability**

- It automatically scales to handle millions of DNS queries per second, allowing users to access applications smoothly even during high-traffic periods.

### **3. Traffic Management**

- Route 53 routes users to the best possible resource using features like latency-based routing, geolocation routing, health checks, and other routing policies.

### **4. Health Checks & Failover**

- It monitors the health of application endpoints and automatically redirects traffic to healthy resources if any failures occur.

### **5. Integration with AWS Services**

- Route 53 works seamlessly with services like S3, CloudFront, and Elastic Load Balancing, making it easy to route traffic within scalable AWS architectures.

## Types of AWS Routing Policies (Simplified)

### **1. Simple Routing Policy**

- Routes traffic to one single resource. You can add multiple IPs in one record, but all act as a single response.
	-  **Example:**  
You have a single website hosted on **one EC2 instance**.

	   Domain: www.amazon.com
    
		 Resource: One EC2 public IP (e.g., 192.23.30.9)
    

	-  **How it works:**  
All users, from anywhere, are always routed to this one server.

### **2. Failover Routing Policy**

- Automatically sends traffic to a backup resource if the primary one becomes unhealthy.
	- **Example:**  
You want a backup server ready in case the primary server fails.

		Primary: EC2 instance A → Healthy? Yes
    
		 Secondary: EC2 instance B → Used only if A fails
    

  - **How it works:**  
If EC2 A becomes unhealthy (detected via health check), Route 53 automatically sends all traffic → EC2 B.

### **3. Geolocation Routing Policy**

- Routes users based on their geographic location (continent, country, or state).  
Example: France → French site, US → English site.
	-  **Example:**  
You want to show different websites/content based on the user’s country.

		India users → in.amazon.com
    
		US users → us.amazon.com
	    Europe → eu.amazon.com
    

	-  **How it works:**  
A user from France automatically gets the French version of your website.

### **4. Geoproximity Routing Policy**

- Routes traffic based on location of users and resources and allows shifting more traffic to one region using bias.
	- **Example:**  
Your company has two data centers:

		One in India
    
		One in Singapore
    

		You want most Indian traffic to go to India, but also want to shift 20% of Indian traffic toward the Singapore backend using a “bias”.

	- **How it works:**  
Route 53 routes based on geographic proximity but allows manual traffic shifting using **bias values**.

### **5. Latency Routing Policy**

- Routes traffic to the AWS region with the lowest latency, improving performance for users.
	 - **Example:**  
		Your application is hosted in multiple AWS regions:

		  US-East (Virginia)
    
		  Asia Pacific (Mumbai)
    
		  Europe (Ireland)
    

	- **How it works:**

		A user from Germany → routed to Ireland
	    
		A user from Bangalore → routed to Mumbai
    
	    A user from New York → routed to Virginia
    

		This gives the best speed and performance.

### **6. Multivalue Routing Policy**

- Returns multiple IP addresses, but only for healthy resources, providing basic load balancing.
	-  **Example:**  
		You have several web servers and want simple load balancing  without using ELB.

		8 EC2 instances running the same site
    
		Each instance has its own IP
    

	- **How it works:**  
Route 53 returns multiple healthy IPs to the client. Usually the browser picks one → basic load balancing.

### **7. Weighted Routing Policy**

- Routes traffic based on weights assigned by the user (e.g., 70% to Server A, 30% to Server B).
	- **Example:**  
You are doing A/B testing for your website.

		New version → 30% traffic
    
		Old version → 70% traffic
    

	- **How it works:**  
Route 53 distributes traffic based on weights:

		Server A → weight 70
    
		 Server B → weight 30
    

		You can adjust weights anytime (e.g., slowly shift to 100% new version).
 





