# Platform Services Overview

This document provides an overview of the platform's core services and their responsibilities.

---

## Template Service
Manages content templates within the Content Hub, including creation, editing, duplication, and versioning.  
Provides structured formats for messages like emails, SMS, or push notifications used in campaigns.

---

## Comment Service
Enables users to add, view, and manage comments within a specific template.  
Useful for collaboration and feedback during content creation and review processes.

---

## Cache Service
Provides high-performance caching for various services to reduce response time and backend load.  
Commonly used for frequently accessed data such as user metadata, configurations, or rendered templates.

---

## Presentation Layer
Acts as the backend for the platform’s UI, serving data and rendering endpoints for the web interface.  
Powers user interaction, template editing, campaign visualization, and more.

---

## Agent Service
Acts as the bridge to AI large language models (LLMs), enabling capabilities like content generation, personalization, summarization, and intelligent content suggestions.

---

## Feed Management Service
Handles scheduling and orchestration of outbound push notifications.  
Coordinates which messages to send, to whom, and when, often based on triggers or workflows.

---

## Web Fetch Service
Crawls external websites to collect content for AI-based search or recommendation systems.  
Feeds structured/unstructured data to LLMs or data stores for downstream processing.

---

## User Consumer Service
Consumes user-related messages from message queues, enabling real-time updates such as user creation, updates, and triggering events related to user activity.

---

## Consumer Service
Consumes third-party callback events (e.g., from SendGrid) from queues.  
Handles tasks like email delivery status updates, unsubscribe events, and bounce processing.

---

## Conductor Consumer Service
Listens to workflow execution events from queues.  
Processes tasks or steps in asynchronous workflows managed by Conductor, ensuring reliable orchestration.

---

## Ingest API Service
Provides endpoints for clients to send data into the platform (e.g., events, user traits, external signals).  
Supports data enrichment and downstream processing.

---

## Webhook Ingest API Service
Specifically designed to handle webhook callbacks (e.g., from SendGrid).  
Accepts and processes real-time feedback events like opens, clicks, bounces, etc.

---

## Data Process Layer Service
Runs background jobs to transform and analyze data.  
Generates metrics, aggregates insights, and prepares data for reporting, visualization, or AI training.

---

## Frontend Canvas
Hosts the frontend UI for the platform.  
Provides a visual workspace (canvas) for designing campaigns, workflows, and interacting with templates and analytics.

---

## Conductor Server
The core orchestration engine that schedules and manages the execution of workflows across services.  
Ensures each task is triggered and processed correctly.

---

## Conductor Email Worker
Executes email-sending tasks within workflows.  
Integrates with email delivery providers like SendGrid to send transactional or marketing emails.

---

## Conductor SMS Worker
Sends SMS messages as part of workflow executions.  
Works with SMS gateways or platforms to ensure timely delivery to end users.

---

## Conductor Tag Worker
Applies tags to users based on workflow triggers.  
Useful for segmentation, personalization, and downstream targeting.

---

## Conductor Statsig Worker
Integrates with third-party A/B testing and experimentation platform Statsig.  
Applies experiment variants and tracks user exposure.

---

## Conductor Manage Experiment Worker
Handles custom experiment logic and variant assignment within the platform.  
Enables self-hosted or custom logic A/B testing and multivariate experiments.

---

## Conductor Smart Email Worker
Uses AI to generate and send personalized emails automatically based on user data, behavior, and template structure.

---

## Conductor Smart Push Worker
AI-powered push notification sender.  
Dynamically composes and delivers push messages tailored to individual user behavior and context.

---

## Conductor Render Worker
Renders final message content (SMS or push) before sending.  
Resolves templates, variables, and personalization logic.

---

## Conductor UI
Provides a visual interface for workflow management.  
Allows users to view, debug, and monitor workflow executions and their task status.
