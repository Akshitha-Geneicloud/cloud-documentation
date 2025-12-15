
#  **EC2 Deployment + CloudWatch Monitoring + SNS Alerts**

## **Task Documentation**

This project demonstrates how to:

1.  Deploy a web application on an EC2 instance
    
2.  Serve it using Nginx
    
3.  Configure CloudWatch metrics & alarms
    
4.  Create SNS notifications for failures or high CPU usage
    
5.  Test the alerting system using CPU spike simulation & instance stop
    

----------

----------

#  **1. Launch EC2 Instance**

-   Launched an **Ubuntu EC2 instance** in AWS
    
-   Chose a **t2.micro** free-tier instance
    
-   Added **HTTP (port 80)** in Security Groups
    
-   Downloaded the `.pem` key for SSH access
    
-   Connected to the instance using Git Bash:
    

`chmod 400 key.pem
ssh -i key.pem ubuntu@PUBLIC_IP` 

----------

----------

#  **2. Install & Configure Nginx Web Server**

Inside the EC2 instance:

`sudo apt update
sudo apt install nginx -y` 

Start and enable Nginx:

`sudo systemctl start nginx
sudo systemctl enable nginx` 

Confirm it works by opening EC2 Public IP in your browser.

----------

----------

#  **3. Upload Web Application (Chatbot Project)**

Copied the project from local system to EC2 using:

`scp -i key.pem -r "path/to/secure-chatbot" ubuntu@PUBLIC_IP:/home/ubuntu/` 

Moved files to Nginx root directory:

`sudo cp -r secure-chatbot/* /var/www/html/` 

My project didn't have `index.html` but had `chat.html`, so I accessed it as:

`http://PUBLIC_IP/chat.html` 

Result:  
✔ Chatbot web application successfully deployed on EC2.

----------

----------

#  **4. Simulate High CPU Utilization (for Alarm Testing)**

Created a Python script `/home/ubuntu/cpu_spike.py` with:

`import time def  simulate_cpu_spike(duration=30, cpu_percent=80): print(f"Simulating CPU spike at {cpu_percent}%...")
    start_time = time.time()

    target_percent = cpu_percent / 100 total_iterations = int(target_percent * 5_000_000) for _ in  range(total_iterations):
        result = 0  for i in  range(1, 1001):
            result += i

    elapsed_time = time.time() - start_time
    remaining_time = max(0, duration - elapsed_time)
    time.sleep(remaining_time) print("CPU spike simulation completed.") if __name__ == "__main__":
    simulate_cpu_spike()` 

Ran the script:

`python3 cpu_spike.py` 

Result:  
✔ CPU usage temporarily increased  
✔ CloudWatch captured metrics

----------

----------

#  **5. Create CloudWatch Alarms**

Created **two alarms**:

### **1. CPU Utilization Alarm**

-   Metric: **CPUUtilization**
    
-   Condition: CPU ≥ 30%
    
-   Period: 1 minute
    
-   Datapoints: 1 out of 1
    
-   Alarm triggered during CPU spike simulation
    

### **2. EC2 Status Check Alarm (To Detect Crash/Stop)**

-   Metric: **StatusCheckFailed**
    
-   Condition: ≥ 1
    
-   Period: 1 minute
    
-   Missing data: **Treat as Breaching**
    
-   Purpose: Trigger alert when instance crashes, becomes unreachable, or is stopped
    

----------

----------

#  **6. Configure SNS for Email Notifications**

Created an SNS Topic:

-   Topic name: `EC2-Alerts-Topic`
    
-   Protocol: **Email**
    
-   Added subscription: my email ID
    
-   Confirmed subscription from inbox
    

Connected both alarms to this SNS topic.

Result:  
✔ Received email notifications when alarms triggered.

----------

----------

#  **7. Testing the Alerts**

### **Test A: High CPU Spike**

-   Ran the Python script
    
-   CPU alarm triggered
    
-   Received an SNS email alert
    

### **Test B: Stop the EC2 Instance**

Stopped the instance:

`EC2 → Instances → Stop` 

CloudWatch detected failure → StatusCheckFailed alarm triggered  
SNS email received.

----------

----------

#  **8. Restart EC2**

After testing:

-   Restarted the EC2 instance
    
-   Application became accessible again
    
-   CloudWatch alarm returned to **OK** state
