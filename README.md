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
<img src="https://github.com/Bomays/holbertonschool-system_engineering-devops/blob/993362b3d0484759f3625f2de43ece3a6e0b1203/Images%20tasks/Simple%20web%20Stack.png" alt="Simple web structure" width="500"/>
</p>

$~$

#### Infrastructure

1. Server: A physical or virtual machine that hosts all the components of the web application.

2. Domain Name: foobar.com is the human-readable address for the website. The www record is a CNAME (Canonical Name) DNS record type that points to the server's IP address.

3. Web Server (Nginx): Receives HTTP requests and serves static content. It also acts as a reverse proxy, forwarding dynamic content requests to the application server.

4. Application Server: Executes the application code and generates dynamic content.

5. Application Files: The codebase of the website.

6. Database (MySQL): Stores and manages the website's data.

7. Communication Protocol: The server uses HTTP/HTTPS protocols to communicate with the user's computer.

$~$

#### Issues with this Infrastructure

1. Single Point of Failure (SPOF): If the server goes down, the entire website becomes unavailable.

2. Downtime during Maintenance: Any maintenance, such as deploying new code or restarting the web server, will result in website downtime.

3. Scalability Limitations: This setup cannot handle large amounts of traffic as it's limited to the capacity of a single server.

$~$

### Task 1: Distributed web infrastructure 








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

- QPF stands for:

 **Quartus Prime Project File** is a file format used in the Quartus Prime software, which is a development tool for programmable logic devices.

 The .qpf file contains basic information about:

- The current version of the Quartus Prime software
- The date of project creation
- A list of all revisions created for the project
