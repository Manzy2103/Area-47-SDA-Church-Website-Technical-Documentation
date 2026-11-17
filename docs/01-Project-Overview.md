
# Area47 SDA Website – Project Overview

## 1. Introduction

The Area47 Seventh-day Adventist Church website is a Django-based web platform developed to improve communication, information sharing and digital operations within the church.

The project initially focused on establishing a public-facing online presence where church members and visitors could access important church information, announcements, events and programmes.

As development progressed, the project expanded beyond its original scope to include administrative functionality, application management and a self-managed hosting infrastructure.

This documentation presents the project's background, objectives, evolution, technical responsibilities and the technologies used throughout its development.

## 2. Project Background

The website was developed as part of an initiative to improve communication between the church and its members, as well as with the wider community.

The primary motivation was to establish a central digital platform through which important church information could be published and accessed more conveniently.

Previously, communication and information sharing relied on different channels, including social media platforms (still being used). The website was intended to complement these channels by providing a centralised and accessible source of church information.

Particular attention was given to improving the communication of announcements and upcoming events while incorporating multimedia content, including Facebook videos and live streaming to make church-related information and media more accessible through the website.

Beyond communication, the platform was also intended to support the church's broader digital operations through features for managing content, activities and administrative functions.

## 3. Project Objectives

The main objectives of the project were to:

- Establish a dedicated online presence for Area47 Seventh-day Adventist Church.
- Improve communication between the church and its members.
- Provide a central platform for publishing announcements and important updates.
- Make church events, activities and programmes accessible online.
- Integrate multimedia content, including Facebook videos.
- Provide tools for managing website content and church-related information.
- Introduce administrative functionality to support selected church operations.
- Establish a reliable and maintainable hosting environment for the application.

## 4. Project Evolution

The project initially started with a relatively straightforward objective: developing a public-facing website for Area47 Seventh-day Adventist Church.

The original hosting plan was to use shared hosting, which was expected to be sufficient for the website's requirements.

However, during preparation for deployment, a hosting limitation was encountered. The shared hosting providers considered did not offer the required Python and Django hosting environment.

This led to a change in the deployment strategy. Instead of proceeding with shared hosting, the decision was made to use a Virtual Private Server (VPS).

### 4.1 Transition to VPS Hosting

Moving to a VPS introduced a new level of responsibility, flexibility and control.

Unlike shared hosting, a VPS provided greater control over the server environment, allowing the application and its supporting services to be configured according to the project's requirements.

The server environment was configured from scratch, beginning with the initial Ubuntu Linux setup and progressing through the installation, configuration and management of the services required to host the application.

This transition introduced practical responsibilities in:

- Linux server administration.
- SSH access and server security.
- Web server configuration.
- Application server deployment.
- Database installation and management.
- Domain and DNS configuration.
- SSL certificate management.
- Application deployment and maintenance.

### 4.2 Expansion of the Project

As development and infrastructure management progressed, the project gradually expanded beyond its original public-facing website requirements.

The VPS environment provided the opportunity to establish and manage additional services supporting the wider digital operations of the church.

These developments included:

- Establishing a dedicated mail server.
- Configuring Nginx and Gunicorn for application hosting.
- Installing and managing PostgreSQL.
- Introducing background task processing using Celery and Redis.
- Developing administrative dashboards and management functionality.
- Implementing Progressive Web App capabilities.
- Integrating web push notifications.
- Establishing separate staging and production environments.
- Introducing Docker containerisation to improve deployment consistency.

### 4.3 From Website Development to Infrastructure Management

What began as a website development project gradually evolved into a broader application hosting and infrastructure management undertaking.

The transition to VPS hosting created an opportunity to gain practical experience beyond application development, particularly in Linux systems administration, networking, web hosting, mail infrastructure, deployment and service management.

The project consequently became an ongoing development and learning environment, combining backend software engineering with practical IT infrastructure management.

## 5. Technical Architecture and Technology Stack

The project uses a combination of technologies across backend development, frontend tooling, database management, infrastructure and deployment.

| Area | Technologies |
|---|---|
| Programming language | Python |
| Backend framework | Django |
| Database | PostgreSQL |
| Frontend tooling | Vite, Tailwind CSS |
| Frontend interactivity | Alpine.js |
| Background processing | Celery |
| Message broker | Redis |
| Web server | Nginx |
| Application server | Gunicorn |
| Operating system | Ubuntu Linux |
| Version control | Git |
| Containerisation | Docker |
| Mail transfer agent | Postfix |
| Mail access | Dovecot |
| Email authentication | OpenDKIM, SPF, DKIM and DMARC |
| Frontend calendar | FullCalendar |
| Rich text editing | TinyMCE |
| Application delivery | Service Workers and Web Push Notifications |

*The technologies listed represent the project's technology stack and ongoing infrastructure work. Individual implementation details and completion status are covered in the relevant technical sections.*

## 6. My Role and Technical Contributions

I was responsible for the development of the Area47 SDA website and the setup, configuration and management of its hosting infrastructure.

My involvement extended beyond application development into the administration of the underlying server environment.

### 6.1 VPS Provisioning and Linux Server Administration

Following the decision to move from shared hosting to VPS hosting, I undertook the server setup and configuration from the beginning.

My responsibilities included:

- Initial VPS setup and Ubuntu Linux server configuration.
- SSH access configuration and security hardening.
- Linux user and permission management.
- Firewall configuration using UFW.
- Intrusion prevention using Fail2ban.
- Web server installation and configuration using Nginx.
- Application server deployment using Gunicorn.
- SSL certificate configuration and HTTPS.
- DNS and domain configuration.
- PostgreSQL installation and database administration.
- Linux service management and troubleshooting.

### 6.2 Backend Development

I developed and maintained the website using Python and Django.

My backend responsibilities included:

- Developing Django applications and reusable components.
- Designing and managing database models.
- Implementing authentication and user management.
- Developing administrative dashboards and management functionality.
- Implementing content management features.
- Integrating background processing using Celery and Redis.
- Developing and maintaining application functionality across different modules.

### 6.3 Mail Server Infrastructure

The infrastructure expanded to include a dedicated mail server to support the church's email requirements.

This involved:

- Configuring Postfix for email delivery.
- Configuring Dovecot for mailbox access.
- Integrating PostgreSQL for virtual mail account management.
- Configuring OpenDKIM for email authentication.
- Implementing SPF, DKIM and DMARC.
- Managing mail services and troubleshooting delivery issues.

### 6.4 Deployment and Application Operations

I also took responsibility for deploying and maintaining the application across staging and production environments.

This included:

- Managing application code through Git.
- Maintaining separate staging and production environments.
- Performing application updates and database migrations.
- Building frontend assets using Vite.
- Managing static files and production deployment.
- Managing application services.
- Troubleshooting deployment and runtime issues.

### 6.5 Application and Infrastructure Improvements

As the project evolved, additional application and infrastructure improvements were introduced.

These included:

- Implementing Progressive Web App functionality.
- Integrating web push notifications.
- Using Celery and Redis for asynchronous processing.
- Introducing Docker containerisation to improve deployment consistency.
- Working towards more repeatable and automated infrastructure deployment.

## 7. Project Scope

The project covers several areas of software development and infrastructure management.

### 7.1 Public-Facing Website

The public-facing side of the platform focuses on providing church-related information and improving communication with members and visitors.

### 7.2 Content and Administrative Management

The application includes administrative functionality designed to support the management of website content and selected church operations.

### 7.3 Application Hosting Infrastructure

The hosting environment includes the Linux operating system, web server, application server, database and supporting services required to operate the website.

### 7.4 Mail Infrastructure

A dedicated mail server provides email services and incorporates authentication mechanisms to improve email security and deliverability.

### 7.5 Deployment and Environment Management

The project incorporates version control, separate staging and production environments, and ongoing work towards containerised deployment and infrastructure automation.

## 8. Challenges and Technical Decisions

One of the first significant challenges encountered was the lack of suitable Django hosting support from the shared hosting providers initially considered.

Rather than changing the application technology to accommodate the hosting limitations, the deployment approach was changed by moving to VPS hosting.

This decision provided greater control over the hosting environment but also introduced additional responsibilities for server configuration, security, maintenance and service management.

The project demonstrates how infrastructure decisions can influence the development and long-term operation of an application.

Additional technical challenges and their respective solutions will be documented in a dedicated section of this repository.

## 9. Professional Experience and Skills Developed

Working on the Area47 project provided practical experience across multiple areas of software engineering and IT infrastructure.

The major areas include:

- Backend application development.
- Python and Django development.
- Linux systems administration.
- Web server and application hosting.
- PostgreSQL database administration.
- Email server configuration and management.
- Networking, DNS and server security.
- Application deployment and maintenance.
- Background task processing.
- Progressive Web App development.
- Containerisation and DevOps practices.
- Technical troubleshooting and problem-solving.

The project also provided an opportunity to work across the application and infrastructure layers, developing an understanding of how software applications are deployed, hosted, secured and maintained in a production environment.

## 10. Documentation Structure

The technical documentation is organised into the following sections:

| No. | Document | Description |
|---|---|---|
| 01 | Project Overview | Background, objectives, evolution and technical contributions |
| 02 | System Architecture | Application architecture and component relationships |
| 03 | Backend Development | Django applications, models, authentication and business logic |
| 04 | Database Management | PostgreSQL database design and management |
| 05 | Linux Server Administration | Ubuntu server configuration and administration |
| 06 | Deployment Process | Staging, production and release workflows |
| 07 | Docker Containerisation | Docker configuration and container deployment |
| 08 | Mail Server Infrastructure | Mail services, authentication and email security |
| 09 | Challenges and Solutions | Technical problems, troubleshooting and lessons learned |

## 11. Documentation Status

**Status:** In Progress

This repository is maintained separately from the application's private source-code repository.

It serves as a technical case study documenting the development, deployment and infrastructure management of the Area47 SDA website.

Sensitive configuration, credentials, private user information and internal security details are intentionally excluded.

---

*Technical documentation maintained as part of a professional portfolio demonstrating practical experience in backend development, Linux systems administration and IT infrastructure management.*

