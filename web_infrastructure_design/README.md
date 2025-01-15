# holbertonschool-system_engineering-devops

## General learning objectives

    - You must be able to draw a diagram covering the web stack
      you built with the sysadmin/devops track projects
    - You must be able to explain what each component is doing
    - You must be able to explain system redundancy
    - Know all the mentioned acronyms: LAMP, SPOF, QPS


## Project Tasks:

### Task 0: Simple web stack 

A user wants to access the website www.foobar.com. Here's how the one server web infrastructure works:

<p align="left">
<img src="https://github.com/Bomays/holbertonschool-system_engineering-devops/blob/d8a109c52ef7ed7bc54f20eb12fc07f658da15c6/web_infrastructure_design/Images%20tasks/Simple%20web%20Stack.png" alt="Simple web stack" width="300"/>
</p>

$~$

#### Infrastructure

1. **Server:** A physical or virtual machine that hosts all the components of the web application.

2. **Domain Name:** foobar.com is the human-readable address for the website. The www record is a CNAME (Canonical Name) DNS record type that points to the server's IP address.

3. **Web Server (Nginx):** Receives HTTP requests and serves static content. It also acts as a reverse proxy, forwarding dynamic content requests to the application server.

4. **Application Server:** Executes the application code and generates dynamic content.

5. **Application Files:** The codebase of the website.

6. **Database (MySQL):** Stores and manages the website's data.

7. **Communication Protocol:** The server uses HTTP/HTTPS protocols to communicate with the user's computer.

$~$

#### Issues with this Infrastructure

1. **Single Point of Failure (SPOF):** 

Nginx Web Server:
If it fails, all incoming requests will be blocked.

Application Server:
If it fails, the application logic cannot process requests.

MySQL Database:
If the MySQL Database goes down, all data access operations will be blocked, affecting the entire application.

2. **Downtime during Maintenance:** Any maintenance, such as deploying new code or restarting the web server, will result in website downtime.

3. **Scalability Limitations:** This setup cannot handle large amounts of traffic as it's limited to the capacity of a single server.

$~$

### Task 1: Distributed web infrastructure 

$~$

<p align="left">
<img src="https://github.com/Bomays/holbertonschool-system_engineering-devops/blob/d8a109c52ef7ed7bc54f20eb12fc07f658da15c6/web_infrastructure_design/Images%20tasks/Web%20structure.png" alt="Web structure" width="300"/>
</p>

$~$


#### What has been added:

1. **Application Servers**
Each hosts application files and handles logic processing.
This ensures horizontal scaling and resilience.

2. **Web Server (Nginx)**
Increases reliability and load handling. If one server fails,
the load balancer directs traffic to the other.

3. **Load balancer (HAPoxy)**
To distribute incoming requests across multiple web servers to
improve fault tolerance and scalability.
Configured with a Round Robin distribution algorithm, which cycles through servers sequentially to balance load evenly.

4. **Setup:** An Active-Active setup is used, meaning all servers handle traffic simultaneously. This setup ensures high availability and better resource utilization compared to an Active-Passive setup, where only one server is active, and the other remains idle until a failure occurs.

5. **Primary and Replica Database (MySQL)**
Implements a Primary-Replica setup for better performance. The Primary database handles all write operations, while the Replica processes read operations to reduce the load on the Primary.

Differences:
The Primary Node is the main source of truth and handles data writes, while the Replica Node
is used only for read operations. This separation improves performance and reduces contention.


$~$

#### Issues with Infrastructure:

1. **Additionnal Single Point of Failure (SPOF)**

The load balancer:
If the load balancer fails, all traffic will be disrupted, and the entire system may go down until the issue is fixed.  There is no mechanism to distribute requests to the web servers, effectively stopping all incoming traffic to the application.

MySQL Primary Database (DBP):
It handles write operations and synchronizes data with its replica. If DBP becomes unavailable, the application will not be able to write or modify data, which could result in errors or complete downtime.

Nginx Web Servers:
If both of these servers go down, users cannot access the application. Redundancy for web servers is essential.

Application Servers:
If one of these servers fails, it could lead to service disruption unless there is a way to redirect traffic to the other server or ensure load balancing across a larger pool of application servers.


2. **Security Issues**
No firewall: Systems are exposed to direct attacks.
No HTTPS: Communication between clients and servers is insecure, risking data interception.

3. **No Monitoring**
There is no visibility into server health, traffic, or database performance, making it harder to detect and respond to failures or performance degradation.

$~$

### Task 2: Secured and monitored web infrastructure 


$~$

<p align="left">
<img src="https://github.com/Bomays/holbertonschool-system_engineering-devops/blob/d8a109c52ef7ed7bc54f20eb12fc07f658da15c6/web_infrastructure_design/Images%20tasks/Secured%20and%20monitored%20web%20infrastructure.png" alt="Secured and monitored web infrastructure" width="300"/>
</p>

$~$



#### What has been added:

1. **Firewalls:**
Firewalls added in the flow before the HAProxy Load Balancer.
They are placed to ensure only legitimate traffic reaches the load balancer and backend systems. They inspect and filter out malicious or unauthorized requests, protecting infrastructure from attacks and unauthorized access.

2. **SSL Certificate:**
SSL Certificate is placed to ensure all traffic is encrypted using HTTPS, meaning the load balancer decrypts incoming HTTPS traffic and forwards it as HTTP internally.
It ensures the traffic is encrypted between the browser and the server, protecting sensitive data such as passwords, cookies, and other personal information from eavesdropping and tampering.

3. **Monitoring Clients:**
They are added after the Load Balancer to collect metrics, logs, and system health data.
Monitoring helps track system health, performance, and usage, allowing for proactive troubleshooting and optimization. If something goes wrong, we can be alerted.

4. **What Happens If You Want to Monitor QPS (Queries Per Second):**
To monitor QPS on the web server, we have to track the HTTP Request rates using monitoring clients that can integrate with log analysis tools like **Sumo Logic** to gather such data.
We have to configure logging in the web servers (Nginx) to log every incoming request and use a tool like **Prometheus or Sumo Logic** to aggregate and visualize these logs.


$~$

#### Issues in the Infrastructure:

1. **Terminating SSL at the Load Balancer:**
While terminating SSL at the load balancer provides scalability and simplifies management, it can lead to a security gap if internal traffic isn't secured properly (between the load balancer and web servers). This requires proper internal encryption (SSL/TLS) to ensure end-to-end encryption.

2. **Only One MySQL Server Capable of Accepting Writes:**
Having only one MySQL Primary Database for writes creates a single point of failure. If this server goes down, the entire database becomes unavailable for writes, causing downtime or service interruptions.
The solution would be implementing MySQL replication (which present with a replica DB) helps, but read/write splitting and clustering should be considered for better high availability and scaling.

3. **Having all the same components on Servers:**
Servers that share identical roles (Nginx Web Server, Application Server, and Database Server) might have resource contention. For instance, if the application server uses heavy CPU, it might impact the database performance or web serving capabilities.
The solution would be to consider separating concerns across different servers and instances for scalability. For example, one set of servers for web serving, another for application logic, and a separate cluster for databases.

4. **SSL Certificate Layer:**
If the SSL termination point fails, HTTPS traffic cannot be decrypted, disrupting secure communication.
To avoid this we must terminate SSL at multiple endpoints or deploy SSL offloading at a redundant load balancer. Also automate SSL certificate management to avoid expiration issues.

5.**DNS Server**
DNS server level can have a SPOF, but modern DNS infrastructures are typically designed to avoid this. 

$~$

### Task 3: Scale Up

$~$

<p align="left">
<img src="https://github.com/Bomays/holbertonschool-system_engineering-devops/blob/d8a109c52ef7ed7bc54f20eb12fc07f658da15c6/web_infrastructure_design/Images%20tasks/Scale%20up.png" alt="Scale Up" width="300"/>
</p>

$~$


#### What has been added:

1. **HAProxy Load Balancer Cluster:**
Two HAProxy instances are used for load balancing. This ensures high availability and fault tolerance. If one load balancer fails, the other will take over the task of distributing traffic.
This is a critical addition to ensure the infrastructure is resilient and can handle increased traffic without downtime.

2. **New Server (Additional Nginx Web Server):**
The additional web server splits the load between two Nginx servers. This helps to distribute traffic better, providing higher scalability and reliability.

3. **Component Separation:**
Web servers are separated from application servers, ensuring the web-facing components can be managed independently from the application logic.
Database (DBP and DBR) and application components are isolated, improving security and scalability.

4. **Monitoring Cluster:**
Monitoring requests from both load balancers help ensure that system health and performance are continuously monitored, providing real-time insights into the infrastructure's operation.

I added SPOF on Monitors even if monitoring itself is not a critical SPOF, but if it fails, visibility into system health is lost.
To prevent from this use redundant monitoring servers or multiple clients.
and ensure monitoring clients send alerts to multiple destinations.


**Acronyms:**

- LAMP stands for:

**Linux:** The operating system that runs all components,
it serves as the foundation, providing the operating system environment

**Apache:** The web server software that delivers web pages,
it handles incoming web requests and serves static content directly

**MySQL:** The relational database management system that stores and manages the website's data

**PHP, Perl, or Python:** The programming language for developing web applications, it processes dynamic content requests, interacts with the database, and generates HTML to be sent back to the user's browser

LAMP is a popular open-source technology stack used primarily for web development, it follows a client-server architecture, where client requests are processed through these layers to deliver the final web content

$~$

- SPOF stands for:

 **Single Point of Failure** is a critical component or part of a system that, if it fails, causes the entire system to stop functioning
 This vulnerability exists because the component lacks a redundant counterpart that can take over its function in case of failure.

$~$

- QPS stands for:

 **Queries Per Second** 
 QPS (Queries Per Second) is a metric that measures the number of requests or queries a system, application, or service processes within one second. It is commonly used to evaluate the performance and scalability of systems such as web servers, APIs, databases, or DNS servers.

```
    Improving QPS:

      Caching:
          Use caching (CDN, in-memory stores like **Redis**) to reduce the load on origin
          servers or databases.

      Load Balancing:
          Distribute queries across multiple servers to avoid bottlenecks.

      Database Optimization:
          Optimize queries, indexes, and schemas to improve database QPS.

      Horizontal Scaling:
          Add more servers or instances to handle increased traffic.

      Rate Limiting:
          Implement rate limits to protect systems from being overwhelmed by excessive requests.
```
