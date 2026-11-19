
# Application Features and Modules

## 1. Introduction

The Area47 SDA Church website is more than a public-facing website. It is a church management and communication platform that provides administrative tools for managing church content, programmes, ministry activities, member interactions and notifications.

This document describes the major application modules, their purpose, key functionality and how they contribute to the overall system.

## 2. User Accounts and Access Management

### 2.1 Overview

The accounts module provides the foundation for user authentication, account management and access control across the application.

A custom Django user model is implemented to accommodate the application's authentication and user management requirements.

### 2.2 Custom User Model

The application uses a custom user model extending Django's `AbstractUser`.

The model includes fields such as:

- Username
- First name
- Last name
- Email address
- Phone number

Email is configured as the primary authentication identifier through Django's `USERNAME_FIELD` configuration.

A custom user manager supports the creation and management of user accounts.

### 2.3 Authentication and Access Control

Django's authentication and permission framework provides the foundation for controlling access to protected application functionality.

Access control is important because the administrative dashboard contains functionality intended for authorised staff and administrators.

The application uses permissions to determine which users can access specific administrative features.

## 3. Staff Administration Dashboard

### 3.1 Overview

The administration dashboard provides a central interface through which authorised church staff can manage different areas of the website.

Rather than requiring staff to interact directly with Django's default administration interface, the project provides a customised dashboard designed around the application's operational requirements.

The dashboard aims to make common administrative activities accessible to non-technical users.

### 3.2 Dashboard Architecture

The dashboard is organised around reusable class-based views.

A shared dashboard view structure provides common functionality for:

- Dashboard pages
- List views
- Create operations
- Update operations

The architecture promotes consistency across administrative modules and reduces duplication.

### 3.3 Permission-Aware Navigation

The dashboard navigation is designed to respond to the permissions assigned to users.

This allows the application to present relevant management sections according to a user's authorised access.

The approach helps maintain a cleaner interface while supporting role-based administrative responsibilities.

### 3.4 CRUD Operations

The dashboard supports common data management operations:

- Create new records
- View existing records
- Update existing records
- Delete records where authorised

Reusable views and components provide a consistent foundation for implementing these operations across different application modules.

### 3.5 Design Considerations

The dashboard focuses on:

- A clean and organised interface
- Consistent administrative workflows
- Reusable application components
- Permission-aware navigation
- Accessibility for non-technical staff

## 4. Blog and Content Management

### 4.1 Overview

The blog module provides functionality for managing and publishing church-related content.

It allows authorised users to manage articles and other content intended for the public-facing website.

### 4.2 Content Management

The module forms part of the church's communication platform, helping administrators maintain an online presence and share relevant information with website visitors.

Content management is integrated into the broader administrative dashboard.

### 4.3 Purpose

The blog module supports the church's digital communication activities by providing a central place for managing website articles and related content.

## 5. Church Bulletin Management

### 5.1 Overview

The bulletin module supports the management of church programmes and related bulletin information.

It provides administrative functionality for organising information that is presented to the congregation through the church bulletin.

### 5.2 Programme Management

The module includes functionality for managing programme items.

Administrators can organise the information associated with church programmes through the administrative interface.

### 5.3 Duties and Assignments

The bulletin functionality also supports the management of duties and assignments associated with church activities.

This helps administrators organise responsibilities and maintain structured programme information.

### 5.4 Dynamic Form Handling

The module uses Django formsets, including:

- `ProgramItemFormSet`
- `DutyFormSet`

Formsets allow multiple related records to be managed within a single form submission.

This is particularly useful when administrators need to add, edit or remove several programme items or duty assignments together.

### 5.5 Sorting and Record Management

The bulletin interface supports dynamic management of form rows, including sorting and deletion.

This provides a more flexible administrative experience when arranging programme information.

### 5.6 Purpose

The bulletin module simplifies the organisation and maintenance of church programme information while reducing the administrative effort involved in managing multiple related records.

## 6. Offering Plan Management

### 6.1 Overview

The Offering Plan module provides members with a simple interface for allocating their intended offerings across different giving categories.

It helps members organise their giving by assigning amounts to individual offering slots and generating a structured summary of their allocations.

The module focuses on offering planning and communication rather than processing financial transactions.

### 6.2 Offering Categories

The offering plan organises allocations into three main areas:

- **Local Church:** Offerings allocated towards the local church.
- **Union/Conference:** Offerings allocated towards the broader church structure.
- **Tithe:** A separate allocation for tithe.

These categories allow members to distribute their intended giving according to the available offering structure.

### 6.3 Allocation Process

Members interact with a form where they assign monetary amounts to the available offering slots.

The form provides a straightforward way to distribute intended contributions across the different categories.

### 6.4 Summary Generation

After completing the allocation form, the application generates a structured text summary showing how the member has distributed their intended offerings.

The summary makes it easier to review the allocations before sharing them.

### 6.5 WhatsApp Sharing

The generated summary can be shared through WhatsApp as text.

This provides a convenient communication mechanism for members to share their offering plans without requiring a separate payment or financial transaction system.

### 6.6 Purpose

The module simplifies offering planning by providing members with a convenient way to organise, review and communicate their intended giving allocations.

## 7. Music Ministry Management

### 7.1 Overview

The music module provides functionality for managing information related to the church's music ministry.

It supports the organisation of music groups and their associated information.

### 7.2 Music Groups

The application includes a `MusicGroup` model for organising music groups within the ministry.

This provides a structured way to represent the different groups and their information.

### 7.3 Group Members and Directors

The module supports the management of information associated with music group members and directors.

This helps maintain organised records of the people involved in the music ministry.

### 7.4 Music Videos

The module also supports the management of music-related video content.

This contributes to the church's wider digital media and communication activities.

### 7.5 Purpose

The music module provides a centralised way to organise music ministry information and related media content.

## 8. Notification Management

### 8.1 Overview

The notifications module provides the foundation for managing and delivering push notifications to supported devices.

It forms part of the application's communication capabilities, particularly for users accessing the website through an installed Progressive Web Application.

### 8.2 Push Subscriptions

The application maintains push subscription information through a `PushSubscription` model.

This allows the system to keep track of browser or device subscriptions used for receiving push notifications.

### 8.3 Administrative Notifications

The `AdminNotification` model supports the management of notifications initiated through the administrative side of the application.

This provides a structured way to manage notification content.

### 8.4 Notification Logs

The `NotificationLog` model supports the recording of notification delivery activity.

Notification logs are useful for monitoring delivery attempts and investigating failures.

### 8.5 Notification Delivery

The application integrates push notification delivery with Celery background processing.

The notification task is implemented in:

`notifications.tasks.send_push_notification`

Using background processing allows notification delivery work to be handled separately from the main web request.

Detailed background processing and task execution are documented in:

`04-background-processing.md`

### 8.6 Purpose

The notifications module extends the church's communication capabilities by allowing supported devices to receive notifications from the application.

## 9. Progressive Web Application (PWA)

### 9.1 Overview

The website includes Progressive Web Application functionality, allowing supported devices to install and access the application through an app-like experience.

The implementation is intended to improve accessibility for members who primarily access the platform through mobile devices.

### 9.2 Web App Manifest

The application provides a web app manifest containing installation-related information.

The manifest includes:

- Application identity
- Application display configuration
- Application icons
- Installation-related metadata

The application uses standalone display mode to provide an app-like experience when installed.

### 9.3 Service Worker

The application uses a service worker located at:

`/service-worker.js`

The service worker supports caching and offline-related functionality.

It also manages specific caching exclusions for areas such as:

- Django administration
- TinyMCE

These exclusions help avoid inappropriate caching of dynamic or administrative resources.

### 9.4 Offline Support

The application includes an offline page:

`/offline/`

This provides a fallback experience when a user attempts to access the application without an available network connection.

### 9.5 Mobile Installation

The PWA implementation supports installation on compatible mobile devices, including Android and iOS.

The application provides the necessary manifest and icon assets for supported installation experiences.

### 9.6 Integration with Push Notifications

The PWA functionality works alongside the notification module.

Installed or supported browser environments can register push subscriptions, allowing the application to deliver notifications to subscribed devices.

### 9.7 Purpose

The PWA implementation improves the accessibility of the church website by providing an installable, mobile-friendly experience with caching and offline-related capabilities.

## 10. Module Relationships and Integration

Although the application is divided into separate Django modules, these modules contribute to a shared platform.

Some of their relationships include:

- The accounts module provides authentication and user identity across the application.
- The administration dashboard provides a central management interface for authorised staff.
- The blog and bulletin modules support church information and communication.
- The offering plan module helps members organise and share their intended giving allocations.
- The music module supports the organisation of music ministry information.
- The notification module provides push notification functionality.
- The PWA implementation extends application accessibility and supports mobile notification capabilities.
- Celery and Redis support selected background processing requirements.

The modular approach makes it easier to maintain individual areas of functionality while allowing them to work together as part of one application.

## 11. Application Design Considerations

The application has been developed with several practical considerations in mind.

### 11.1 Maintainability

The use of separate Django applications helps organise functionality according to responsibility.

This makes individual modules easier to maintain and extend.

### 11.2 Reusability

Reusable dashboard views and components reduce duplication and provide consistent administrative workflows.

### 11.3 Access Control

Django's authentication and permission mechanisms provide a foundation for controlling access to administrative functionality.

### 11.4 User Experience

The public-facing website and administrative dashboard serve different audiences.

The administrative interface is designed to simplify common tasks for church staff, while the PWA improves accessibility for members using mobile devices.

### 11.5 Background Processing

Selected operations, particularly push notification delivery, are integrated with Celery to support asynchronous execution.

This helps separate background work from normal web request handling.

## 12. Summary

The Area47 SDA Church website combines public-facing content management, administrative functionality, ministry management, member-oriented tools and communication capabilities.

Its modular Django architecture provides a foundation for maintaining and extending these capabilities.

The application also incorporates supporting technologies such as Celery, Redis and Progressive Web Application functionality to improve the overall platform.

The following documentation explores the technical implementation of background processing, infrastructure, deployment, security and operational maintenance in greater detail.

## 13. Related Documentation

- [Project Overview](01-project-overview.md)
- [System Architecture](02-system-architecture.md)
- [Background Processing](04-background-processing.md)
- [Server Infrastructure](05-server-infrastructure.md)
- [Deployment and DevOps](06-deployment-and-devops.md)
- [Security and Maintenance](07-security-and-maintenance.md)
- [Challenges and Lessons Learned](08-challenges-and-lessons.md)
