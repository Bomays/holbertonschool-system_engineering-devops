# Holberton School - System Engineering & DevOps

Overview of holbertonschool-system_engineering-devops repo:

This repository showcases projects and learning objectives related to system engineering and DevOps, emphasizing the architecture and management of modern web infrastructures. From simple single-server designs to complex, secure, and scalable infrastructures, the content provides a deep dive into key concepts and techniques.

The most important concepts so our experience typing www.google.com is seamless !
> See my article on Medium > [Holberton School System Engineering & DevOps](https://github.com/Bomays/holbertonschool-system_engineering-devops/tree/main)


# Web Infrastructure Design

You can see directly the web infrastucture design readme here >
[Project detailed readme](web_infrastructure_design/README.md)

Or only Flowcharts here > 
[Flowcharts](web_infrastructure_design/Images tasks)


## Simple Web Stack

    A basic single-server infrastructure with:
        Nginx Web Server for static content and reverse proxying.
        Application Server to execute logic.
        MySQL Database to store and manage data.
        
    Issues:
        Single Point of Failure (SPOF).
        Downtime during maintenance.
        Limited scalability.

## Distributed Web Infrastructure

    Adds multiple application servers and a load balancer (HAProxy) for fault tolerance and scalability.
    Implements a Primary-Replica MySQL setup for improved database performance.
    
    Challenges:
        Load balancer SPOF.
        Security vulnerabilities (no firewall or HTTPS).
        Lack of monitoring.

## Secured and Monitored Infrastructure

    Introduces firewalls, SSL certificates, and monitoring clients:
        Firewalls filter unauthorized traffic.
        SSL ensures encrypted communication via HTTPS.
        Monitoring tools track system health and performance.

    Issues:
        Internal traffic encryption gaps.
        Single write-capable MySQL server.
        Resource contention across shared servers.

## Scale-Up

    Deploys redundant load balancers and additional web servers.
    Separates components (web servers, application servers, databases) for better resource management and security.
    Enhancements:
        Load balancing with multiple HAProxy instances.
        Improved scalability and fault tolerance.
        Monitoring cluster to ensure system visibility.

