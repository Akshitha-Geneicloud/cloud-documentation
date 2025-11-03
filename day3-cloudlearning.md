1. What is Nginx?<br>

Nginx (pronounced “engine-x”) is a web server that can also act as:<br>

a reverse proxy (forwards requests to backend servers like Tomcat),<br>

a load balancer (distributes traffic across multiple servers),<br>

and a static file server (serves HTML, CSS, JS, images).<br>

Example use:<br>

Nginx receives user requests on port 80 (HTTP) or 443 (HTTPS).<br>

It forwards them to a backend like Tomcat, Node.js, or Flask.<br>

It then returns the response to the user.<br><br>

2. What is Apache Tomcat?<br>

Apache Tomcat is an application server used mainly to run Java web applications (JSP, Servlets, Spring Boot WAR files).<br>
It’s not the same as Apache HTTP Server — Tomcat specifically runs Java-based code.<br>

Example flow:<br>

Nginx (web server) → sends dynamic requests to Tomcat (application server)<br>

Tomcat → runs the Java code → sends result back through Nginx → user’s browser<br><br>

3. What is Middleware?<br>

Middleware is software that sits between the web server (like Nginx/IIS) and application server (like Tomcat).<br>
It helps in:<br>

Managing communication between front-end and back-end.<br>

Handling authentication, logging, and data transfer.<br>

Acting as a bridge connecting different systems.<br>

Think of it as a translator that makes sure everything works smoothly between the web server and the app.<br><br>

###<Deployment of instance in linux server and hosting it through ngnix><br>
I have deployed the application on an AWS EC2 instance (a virtual Linux or Windows machine in the cloud).<br>
Then, i configured servers like Nginx or Tomcat to host my application and make it publicly accessible through the instance’s public IP or DNS.<br>
#<commands used:><br>
*sudo apt update -y {for updation of system packages}<br>
*sudo apt install nginx -y {for installing nginx in the system}<br>
*sudo systemctl enable nginx {Enable nginx server to start on boot}<br>
*sudo systemctl start nginx {start nginx}<br>
*sudo ufw allow "nginx http" {Alow http traffic through firewalls}<br>
*sudo systemctl ststus nginx {show nginx status}<br>
**stored these commands in the name of "nginx-setup.sh" in the git bash.**<br><br>

###<creation of EC2 instance and policies(for the iam user) that are restricted to only mumbai region><br>
*launch an instance in the root user<br>
*create a iam user account<br>
*create a custom policy for accessing the ec2 instance restricted by mumbai region only<br>
        { <br>
            "Version": "2012-10-17",<br>
             "Statement": <br>
            [  { "Sid": "Statement1",<br>
              "Effect": "Allow",<br>
               "Action": "ec2:*",<br>
                "Resource": "*",<br>
                 "Condition":<br>
                  { "StringEquals":<br>
                   { "ec2:Region": "ap-south-1" } <br>
                  } <br>
                } <br>
            ] <br>
          }<br>
*Now sign in as iam user with the credentials<br>
*Try to access to ec2 instance from different regions.<br>
*Only the instance can be accessed from mumbai region but not from other regions.<br>


