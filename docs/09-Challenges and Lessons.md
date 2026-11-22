
# Challenges and Lessons Learned

## 1. Introduction

The development and deployment of the Area47 SDA Church website has involved challenges across application development, database management, Linux server administration, background processing, deployment automation, and infrastructure configuration.

The project has provided practical experience in building and maintaining a production Django application while independently managing much of the infrastructure required to operate it.

This document highlights some of the technical challenges encountered, the approaches used to address them, and the lessons learned throughout the project.

The challenges are grouped into application development, background processing, server administration, deployment, and infrastructure management.

## 2. Application Architecture and Development

### 2.1 Building a Maintainable Django Application

**Challenge**

As the application expanded, it became necessary to manage multiple functional areas, including content management, church bulletins, music, notifications, events, and administrative operations.

Implementing these features without a consistent structure would make the application increasingly difficult to maintain.

**Approach**

The application was developed using Django's modular application architecture.

Functional areas were separated into Django applications, allowing individual modules to maintain their own models, views, forms, URLs, and related functionality.

A custom administrative dashboard was also introduced to provide a central interface for managing application modules.

Reusable class-based views and shared components were developed to reduce repeated implementation across administrative functionality.

**Lesson Learned**

Application architecture becomes increasingly important as a project grows.

Separating functionality into logical modules and building reusable components makes it easier to maintain, extend, and troubleshoot an application.

### 2.2 Implementing a Custom User Model

**Challenge**

The application required an authentication system that used email addresses as the primary login identifier rather than relying on Django's default username-based authentication.

**Approach**

A custom user model was implemented by extending Django's `AbstractUser`.

The model includes user information such as first name, last name, username, phone number, and a unique email address.

The authentication identifier was configured through `USERNAME_FIELD`, with a custom user manager supporting user creation.

**Lesson Learned**

Authentication requirements should be considered early in a Django project, as Django's default authentication system is username-based.

Introducing a custom user model after other application components have already been developed can create additional migration and integration complexity.

### 2.3 Developing Reusable Administrative Interfaces

**Challenge**

Multiple administrative modules required similar operations, including listing records, creating entries, updating existing records, and managing permissions.

Building each interface independently would result in duplicated code and inconsistent user experiences.

**Approach**

A reusable class-based view structure was introduced through a shared dashboard foundation.

Common functionality was organised into reusable base views for listing, creating, and updating records.

Permission-aware navigation was also implemented so that administrative users are presented with modules according to their assigned permissions.

**Lesson Learned**

Reusable architecture is valuable beyond reducing code duplication. It also helps maintain consistent behaviour and simplifies future application development.


## 3. Background Processing and Application Automation

### 3.1 Integrating Celery and Redis

**Challenge**

Some application operations required processing outside the normal HTTP request-response cycle.

Executing time-consuming operations directly during web requests could affect responsiveness and create unnecessary delays.

**Approach**

Celery was introduced for background task execution, with Redis serving as the message broker.

Celery workers execute background tasks independently of the Django web application, while Celery Beat supports scheduled task execution.

The system supports background operations such as:

- Push notification delivery.
- Facebook access-token refresh for video and live content integration.
- Event status management.
- Scheduled bulletin publishing.
- Bulletin archiving.
- Cleanup of generated Tips and Quotes content.

**Lesson Learned**

Background processing helps separate user-facing operations from tasks that can execute independently. It also introduces additional infrastructure responsibilities, including worker management, scheduling, task monitoring, and failure handling.

### 3.2 Managing Web Push Notifications

**Challenge**

Implementing push notifications required more than simply creating notifications within Django. The application needed to manage device subscriptions, send notifications through the push infrastructure, and maintain records of delivery attempts.

Notification delivery also had to account for subscriptions that were no longer valid or devices that were unavailable.

**Approach**

A PWA-based notification system was implemented using service workers and browser push subscriptions.

The application includes models for:

- `PushSubscription`
- `AdminNotification`
- `NotificationLog`

Celery handles notification delivery through the background task: `notifications.tasks.send_push_notification`

Delivery results are recorded to support monitoring and troubleshooting. The implementation also includes subscription activity and device-related information to help manage registered endpoints.

**Lesson Learned**

Push notification systems require lifecycle management.

A subscription can become invalid, a device can stop receiving notifications, and delivery can fail even when the application successfully creates a notification.

Maintaining delivery logs and subscription status is important for diagnosing these issues.

### 3.3 Managing Generated Content

**Challenge**

The Tips and Quotes feature allows users to generate visually styled quote images using different themes.

The feature introduced additional requirements involving server-side image rendering, generated file storage, and the management of temporary content.

**Approach**

Puppeteer.js was introduced for server-side image rendering.

Generated content is stored by the application, with a planned retention lifecycle that includes removing generated content after 60 days through background processing.

**Lesson Learned**

Features that generate files introduce additional infrastructure considerations.

Beyond implementing the user-facing functionality, developers must consider rendering dependencies, storage requirements, cleanup operations, and resource management.

## 4. Linux Server Administration

### 4.1 Hosting Multiple Application Environments

**Challenge**

Operating both staging and production environments on the same VPS required careful separation of application files, configuration, dependencies, and running services.

Without proper separation, a staging deployment could interfere with the live application.

**Approach**

Separate application directories were established:

- Production: `/var/www/Area47Website`
- Staging: `/var/www/Area47Website_staging`

The environments use separate configuration files, Python virtual environments, and systemd services. The staging and production applications also use separate PostgreSQL databases.

**Lesson Learned**

Multiple environments can share physical infrastructure while remaining logically isolated.

Environment separation must cover more than application source code. It should also include database connections, secrets, dependencies, background workers, and service management.

### 4.2 Managing Multiple Linux Services

**Challenge**

The application depends on several services that must operate together, including Nginx, Gunicorn, PostgreSQL, Redis, Celery workers, and Celery Beat. Managing these processes manually would make server operations difficult and unreliable.

**Approach**

Systemd was used to manage long-running application services. This allows application servers and background workers to run independently of interactive terminal sessions.

Nginx acts as the reverse proxy, while Gunicorn runs the Django application.

PostgreSQL provides persistent application storage, and Redis supports background processing.

**Lesson Learned**

Deploying an application to a VPS involves managing an ecosystem of interconnected services.

Understanding service dependencies, process management, logs, and restart behaviour is essential for maintaining production applications.

### 4.3 Establishing a Self-Hosted Mail Infrastructure

**Challenge**

Providing domain-based email required more than creating email addresses. The infrastructure needed to support mail transfer, authentication, mailbox access, virtual users, and domain-level email authentication.

**Approach**

A self-hosted mail stack was established using:

- Postfix for mail transfer.
- Dovecot for authentication and mailbox access.
- PostgreSQL for virtual mail user information.
- OpenDKIM for email signing.

The infrastructure also includes SPF and DMARC DNS records.

Postfix uses Dovecot for SMTP authentication and LMTP for local message delivery.

**Lesson Learned**

Operating a mail server involves several interconnected protocols and services.

Successful mail delivery depends on correct server configuration, authentication, DNS records, encryption, and coordination between the mail transfer and mailbox services.

## 5. Deployment and DevOps

### 5.1 Automating Environment-Specific Deployments

**Challenge**

Staging and production deployments required several repetitive operations, including updating application code, installing dependencies, applying database migrations, compiling frontend assets, collecting static files, and restarting services.

Performing these operations manually for every deployment would increase the possibility of human error.

**Approach**

A custom Bash deployment script was developed to automate the deployment workflow.

The script identifies the deployment environment based on the currently selected Git branch.

- `staging` targets the staging environment.
- `main` targets the production environment.

It then performs the required deployment operations and restarts the corresponding systemd services.

**Lesson Learned**

Deployment automation improves consistency and reduces repetitive administrative work.

Separating staging and production deployment logic also helps reduce the risk of accidentally applying changes to the wrong environment.

### 5.2 Managing Frontend Build Dependencies

**Challenge**

The Django application uses a modern frontend build process involving Vite and npm.

Deploying updated frontend code requires compiling assets and ensuring Django can locate the generated files through the Vite manifest. Outdated build artifacts can cause problems when the application is updated.

**Approach**

The deployment script includes frontend dependency installation, removal of previous Vite build artifacts, and execution of the production build.

Django's `collectstatic` command is then used to prepare static assets for deployment.

**Lesson Learned**

Modern Django applications can have deployment requirements beyond Python package installation. A successful deployment must account for both backend dependencies and frontend build processes.

### 5.3 Introducing Docker

**Challenge**

Traditional VPS deployment relies on directly installed system services and environment-specific configuration.

This can make development and deployment environments harder to reproduce consistently.

**Approach**

Docker and Docker Compose were introduced to explore containerised application deployment.

The staging configuration includes containers for:

- Django web application.
- Celery worker.
- Celery Beat.
- Redis.

A Python 3.12-based Docker image was created with the required Linux dependencies.

The containerised environment is being developed separately from the existing production deployment architecture.

**Lesson Learned**

Containerisation can improve environment consistency and portability, but it also introduces additional considerations involving networking, persistent storage, environment variables, service dependencies, and database connectivity.

The Docker work also highlighted the importance of understanding how containers communicate with services running outside their own network.

## 6. Database and Connectivity Challenges

### 6.1 Understanding Database Connectivity Across Environments

**Challenge**

During Docker-based staging development, the Django container initially attempted to connect to PostgreSQL through `localhost`.

However, `localhost` inside a container refers to the container itself rather than the host machine.

This resulted in PostgreSQL connection failures. After adjusting the database host configuration, network connectivity to the host PostgreSQL service was established, but database authentication still required further investigation.

**Approach**

The database connection was changed to use the Docker host gateway through `host.docker.internal`. Network connectivity was then tested independently of Django database authentication.

This helped distinguish between network-level connectivity and database credential or permission problems.

**Lesson Learned**

Database connection failures can occur at different layers. A successful TCP connection does not necessarily mean that database authentication will succeed.

Troubleshooting should distinguish between:

- Hostname resolution.
- Network connectivity.
- Database service availability.
- Authentication credentials.
- Database permissions.

Understanding these differences makes infrastructure troubleshooting more systematic.

## 7. Infrastructure Management and Security

### 7.1 Securing a Publicly Accessible VPS

**Challenge**

Hosting public-facing web and mail services exposes the server to automated scans, authentication attempts, and other internet-based threats. The infrastructure therefore requires security measures beyond application-level authentication.

**Approach**

Several server security controls were implemented:

- SSH key-based authentication.
- A non-default SSH port.
- UFW firewall management.
- Fail2ban intrusion prevention.
- Let's Encrypt SSL/TLS certificates.
- Cloudflare website proxying.
- SPF, DKIM, and DMARC email authentication.

Django production security settings are also configured separately from development settings.

**Lesson Learned**

Server security requires multiple complementary layers.

No single security tool protects every component of an infrastructure environment.

Security must be considered at the operating system, network, web server, application, and mail server levels.

### 7.2 Managing Self-Hosted Infrastructure

**Challenge**

Operating multiple services on one VPS introduces resource and maintenance responsibilities.

The application, database, background workers, mail services, and web server all compete for available server resources.

**Approach**

The infrastructure is managed through Linux services, environment-specific configurations, and application deployment procedures.

System and service logs are used to investigate operational problems.

Resource availability and service status are also considered during maintenance and troubleshooting.

**Lesson Learned**

A production server is not simply a place to host application files.

It is an interconnected environment that requires resource management, service monitoring, security maintenance, and operational planning.

## 8. General Lessons Learned

The development and operation of the Area47 SDA Church website has provided several important lessons.

### Application engineering

- Plan application architecture before adding large numbers of features.
- Use modular Django applications to organise functionality.
- Build reusable components to reduce duplication.
- Consider authentication requirements early in a project.
- Separate background processing from user-facing request handling.

### Database management

- Understand database ownership, roles, and permissions.
- Separate staging and production databases.
- Diagnose network and authentication problems independently.
- Establish reliable backup and recovery procedures.

### Linux administration

- Understand how Linux services interact.
- Use systemd to manage long-running processes.
- Learn to diagnose problems through logs.
- Restrict unnecessary network access.
- Treat server maintenance as an ongoing responsibility.

### Deployment and DevOps

- Automate repetitive deployment operations.
- Keep staging and production isolated.
- Manage backend and frontend deployment requirements together.
- Understand container networking and external service dependencies.
- Test deployment changes before promoting them to production.

### Infrastructure security

- Apply security controls at multiple infrastructure layers.
- Keep sensitive configuration outside source control.
- Maintain secure authentication mechanisms.
- Understand the relationship between DNS, TLS, and mail authentication.
- Plan for backups and disaster recovery before an incident occurs.

## 9. Conclusion

The Area47 SDA Church website has evolved beyond a conventional Django development project into a broader exercise in application engineering, Linux server administration, and infrastructure management.

Working on the project has provided practical exposure to the complete lifecycle of a production web application, from application architecture and feature development to deployment, service management, security, and troubleshooting.

The challenges encountered have reinforced the importance of understanding not only how individual technologies work, but also how they interact within a production environment.

The experience continues to provide a foundation for deeper work in backend development, Linux system administration, DevOps, and infrastructure engineering.
