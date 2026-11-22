
# Security and Server Maintenance

## 1. Overview

The Area47 SDA Church website is hosted on an Ubuntu 24.04 VPS. Server security and maintenance are important aspects of operating the application and its supporting infrastructure.

The server hosts several interconnected services, including:

- Django application.
- PostgreSQL database.
- Nginx web server.
- Gunicorn application server.
- Redis.
- Celery workers and scheduler.
- Postfix mail transfer agent.
- Dovecot mail delivery and access services.

Security is implemented at multiple levels, including Linux server access, network traffic control, web server configuration, application environment separation, mail authentication, and encrypted communication.

This document describes the security measures and operational practices used to maintain the infrastructure.

## 2. Operating System and Server Access Security

### Operating system

The production infrastructure runs on Ubuntu Server 24.04 LTS. The server is administered remotely through SSH, allowing authorised administration without requiring direct physical access.

### SSH hardening

SSH access has been hardened to reduce exposure to automated login attempts and unauthorised access.

Implemented measures include:

- Moving SSH access away from the default port.
- Using SSH key-based authentication for administrative access.
- Restricting server administration to authorised accounts.
- Avoiding unnecessary exposure of administrative services.

The non-default SSH port reduces exposure to some automated scans, but it is not a replacement for strong authentication and access controls. Sensitive SSH configuration, private keys, usernames and connection details are intentionally excluded from this documentation.

## 3. Firewall and Network Security

The server uses UFW (Uncomplicated Firewall) to manage incoming network traffic. Firewall rules are used to control which services are accessible externally. Only the network ports required for the server's intended functions should be exposed publicly.

These include the relevant services for:

- HTTP and HTTPS website traffic.
- SSH administration.
- SMTP mail transport.
- Secure IMAP mail access.

Internal application services such as PostgreSQL and Redis are not intended to be exposed directly to the public internet. The firewall provides an additional layer of protection by limiting unnecessary network access to the server.

## 4. Intrusion Prevention with Fail2ban

Fail2ban is installed to help protect exposed services against repeated unauthorised login attempts. It monitors configured log files and applies temporary bans to IP addresses that exceed defined failure thresholds. This helps reduce repeated automated authentication attempts against services such as SSH.

Fail2ban complements the firewall by dynamically responding to suspicious authentication activity.

## 5. Web Server Security

### Nginx

Nginx is used as the public-facing web server and reverse proxy. It receives incoming HTTP and HTTPS requests and forwards application requests to Gunicorn, which serves the Django application. Nginx also handles the delivery of static assets and provides a central point for managing web traffic.

### HTTPS and TLS certificates

Let's Encrypt is used to provision SSL/TLS certificates for the hosted services. Certificates are maintained separately for the relevant service hostnames rather than relying on one shared certificate for every service.

HTTPS provides encrypted communication between website visitors and the web server. Certificate renewal and validity should be monitored to prevent service interruptions caused by expired certificates.

## 6. Django Application Security

The Django application uses separate configuration settings for development, staging and production.

Environment-specific configuration helps isolate application behaviour and sensitive settings between deployment environments.

Sensitive configuration values are stored in environment files rather than committed to the public documentation repository.

Application security practices include:

- Keeping production configuration separate from development settings.
- Managing secrets outside source control.
- Using Django's production security settings.
- Applying database migrations through Django's migration framework.
- Using authentication and permission controls for administrative functionality.
- Separating staging and production deployments.
- Keeping application dependencies defined in requirements files.

The application also uses a custom user model and permission-aware dashboard functionality to control access to administrative modules.

## 7. Database and Internal Service Security

### PostgreSQL

PostgreSQL is used as the application's relational database. It runs as an infrastructure service on the VPS and is accessed by the Django application through its configured database connection. Database credentials are maintained in environment-specific configuration and are not stored in the public documentation repository.

Database access should remain restricted to authorised application and administrative connections.

### Redis

Redis is used as the message broker and supporting service for Celery background processing. It communicates with the application's background workers and scheduler. Redis is an internal infrastructure dependency and should not be exposed directly to untrusted external networks.

### Service isolation

The application uses separate systemd services for the web application and background processing components.

This provides independent process management for:

- Gunicorn.
- Celery workers.
- Celery Beat.

The staging and production environments also have separate application services.

## 8. Mail Server Security

The infrastructure includes a self-hosted mail server supporting the church's email services.

The mail stack consists of:

- Postfix.
- Dovecot.
- PostgreSQL.
- OpenDKIM.

### SMTP authentication

Postfix uses Dovecot for SMTP authentication. This allows authenticated mail clients to submit outgoing messages through the mail server.

### Mail delivery and access

Postfix handles mail transfer and uses LMTP to deliver locally hosted messages to Dovecot. Dovecot provides secure IMAP access to mailboxes.

Mail users are managed through a separate virtual mail user database rather than being directly tied to Django application accounts.

### Email authentication

Several email authentication mechanisms are configured to improve domain reputation and reduce email spoofing.

**SPF (Sender Policy Framework)**

SPF identifies the mail servers authorised to send email on behalf of the domain.

**DKIM (DomainKeys Identified Mail)**

OpenDKIM provides email signing so receiving mail servers can verify the authenticity and integrity of signed messages.

**DMARC (Domain-based Message Authentication, Reporting and Conformance)**

DMARC defines how receiving mail servers should handle messages that fail SPF and DKIM alignment checks.

The domain uses a quarantine policy for messages that fail the applicable DMARC checks.

### Encrypted mail connections

Let's Encrypt certificates are used for the relevant mail services. Dovecot provides secure IMAP access over IMAPS. Separate certificates are maintained for the relevant service hostnames.

## 9. Domain and DNS Security

The infrastructure uses Cloudflare for domain DNS management and as an additional layer in front of the public website. Cloudflare supports the management of DNS records and provides additional web traffic protection capabilities. DNS configuration is also important to the mail infrastructure.

The mail domain uses records for:

- MX: Identifies the mail server responsible for receiving domain email.
- SPF: Specifies authorised sending infrastructure.
- DKIM: Publishes the public key used to verify signed messages.
- DMARC: Publishes the domain's email authentication policy.

Correct DNS configuration is important for both website availability and reliable email delivery.

## 10. Monitoring and Maintenance

Operating a production server requires regular maintenance beyond the initial installation and deployment of services.

Maintenance activities include:

- Monitoring service availability.
- Checking systemd service status.
- Reviewing application and system logs.
- Monitoring disk and memory usage.
- Checking Nginx and Gunicorn operation.
- Monitoring Celery worker and scheduler processes.
- Reviewing mail delivery and authentication logs.
- Monitoring SSL/TLS certificate validity.
- Applying operating system and application dependency updates.
- Maintaining database backup procedures.

Logs are particularly useful when diagnosing failed deployments, application errors, background task failures and mail delivery problems.

## 11. Security Considerations and Future Improvements

The existing infrastructure incorporates several practical security controls. However, server security is an ongoing process that requires regular review.

Potential improvements include:

- Implementing automated security updates where appropriate.
- Establishing a documented database backup and restoration procedure.
- Testing disaster recovery procedures.
- Introducing centralised log monitoring.
- Configuring automated service health checks.
- Reviewing firewall rules periodically.
- Auditing SSH access and authorised accounts.
- Monitoring failed authentication attempts.
- Introducing vulnerability scanning and dependency auditing.
- Reviewing Django security settings and HTTP response headers.
- Establishing a formal incident response procedure.
- Improving certificate expiry monitoring.

These improvements would strengthen the reliability, security and maintainability of the infrastructure.

## 12. Summary

The Area47 SDA Church infrastructure uses multiple security layers to protect its application and supporting services.

These include Linux server access controls, SSH hardening, UFW firewall management, Fail2ban intrusion prevention, HTTPS encryption, environment-specific Django configuration, restricted internal services, and email authentication mechanisms.

The infrastructure also incorporates operational maintenance practices involving service monitoring, log inspection, dependency updates and certificate management.

Together, these measures demonstrate practical experience in Linux server administration, web application security, network security and self-hosted mail infrastructure.
