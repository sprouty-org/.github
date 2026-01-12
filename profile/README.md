# Sprouty — Smart Plant Companion

Tech Stack & Tools

## Sprouty is a specialized AI-driven microservices platform engineered to bridge the gap between IoT environmental telemetry and botanical care. This project was developed as a solo endeavor for the Software Development Processes (PRPO) 2025/2026 course at the University of Ljubljana, Faculty of Computer and Information Science (FRI).

---

## Table of Contents
#### 1. Project Vision & MVP Scope

#### 2. System Architecture

#### 3. Hardware & Telemetry

#### 4. Cloud Infrastructure & DevOps

#### 5. API Documentation

#### 6. Future Roadmap

#### 7. Local Development

---

## Project Vision & MVP Scope

#### The original project design outlined an interconnected plant ecosystem. To maintain a high standard of reliability within the course timeline, the MVP focuses on a robust "Sense-Identify-Notify" architectural loop.

### Core Functionalities

#### Automated Identification Pipeline:
High-accuracy species recognition via multi-stage API processing pipeline using Pl@ntNet for species recognition and OpenAI to generate custom plant-species care details. This pipeline also serves as a dynamic botanical library, expanding the system's global database whenever a previously undocumented species is added to a user’s garden.

#### Environmental Telemetry:
Real-time data ingestion of soil and atmospheric conditions.

#### Intelligent Alerting:
Event-driven triggers based on species-specific thresholds to maximize plant longevity and health.

#### Identity & Access Management (IAM):
Implements a Token-Exchange Architecture. The Android client performs primary authentication via the Firebase SDK (OIDC IdP) to obtain an idToken. This is exchanged at the User Service for a locally-signed, stateless JWT at login/registering. All subsequent requests are authorized via an Authorization header, which the Gateway validates and maps to the user’s unique Firebase UID for secure Firestore queries.

---

## System Architecture

#### The platform utilizes a decoupled Microservices Architecture, enabling independent scalability, fault isolation, and specialized data handling.

### Service Definitions

#### 1. Gateway:
Acts as the single entry point. It handles JWT Validation and dynamic routing to downstream services using GKE's LoadBalancer.

#### 2. User Service:
Manages user identity and profile metadata, integrated with Firebase Authentication for secure authorization flows.

#### 3. Plant Service:
Processes botanical logic. It chains the Pl@ntNet API (visual identification) with OpenAI GPT-3.5 Turbo to generate custom plant information data.

#### 4. Sensor Service:
The telemetry ingestion hub. It evaluates live ESP32-S3-CAM metrics against thresholds stored in the botanical database.

#### 5. Notification Service:
It utilizes Firebase Cloud Messaging (FCM) to push alerts to the Android client when environmental anomalies are detected.

#### Data persistence:
Uses a NoSQL approach with Google Cloud Firestore. Interaction is handled via the Firebase SDK acting as an Object Document Mapper (ODM) for seamless Java object serialization.

---

## Hardware & Telemetry

#### The "Sense" layer of the project is powered by custom IoT hardware:

#### Microcontroller:
ESP32-S3-CAM.

#### Atmospheric Sensors:
DHT11 for ambient air temperature and humidity.

#### Soil Sensors:
Analog soil moisture probes.

#### Connectivity:
Devices utilize a secure Wi-Fi handshake to transmit telemetry and image payloads to the backend /sensors ingestion endpoint.

---

## Cloud Infrastructure & DevOps

#### The infrastructure is optimized for high availability, low latency, and operational cost-efficiency.

### Deployment & Orchestration

#### Platform:
GKE Autopilot. This allows for managed pod orchestration, ensuring the cluster scales dynamically based on CPU and memory demand.

#### Regionality:
All resources—including GKE, Cloud Firestore and Firebase Storage—are localized in the europe-west4 (Netherlands) region to minimize latency for European users and reduce cross-region egress fees.

#### Health:
Leverages Spring Boot Actuator integrated with Kubernetes Liveness and Readiness probes to ensure automated pod recovery and traffic routing only to healthy instances.

### CI/CD Pipeline

#### Source Control:
GitHub repository with branch protection.

#### Automation:
GitHub Actions workflows triggered on master branch commits.

#### Build:
Automated Maven lifecycle management and unit testing.

#### Containerization:
Docker images are built, tagged, and pushed to Google Artifact Registry.

#### Continuous Deployment:
Kubernetes manifests are updated via kubectl, initiating a Zero-Downtime Rolling Update.

---
 
## API Documentation

The Microservice Ecosystem is fully documented using the OpenAPI 3.0 specification.

#### SWAGGER UI:
http://sprouty.duckdns.org/swagger-ui.html

---

## Future Roadmap

#### The system's modular design is prepared for the following service expansions:

#### Lifecycle Service:
Generating time-lapse growth videos from sensor-captured imagery for scientific logging and user engagement.

#### Shop Service:
A marketplace providing plant-specific products (vases, fertilizers, soil and other plant care accessories) based on the user's current garden inventory.

#### Community Service:
A social layer for sharing growth milestones and care tips once the user base reaches a high enough number.

---

## Local Development

#### To initialize a local development environment:

#### Clone the repository:
git clone https://github.com/sprouty-org/sprouty-main

#### Build the artifacts:
mvn clean install

#### Kubernetes Deployment:
make sure you go to the project's root directory and then

kubectl apply -f kubernetes/infrastructure.yaml

kubectl apply -f kubernetes/scaling.yaml

Note: Deployment requires pre-configured Kubernetes Secrets (app-secrets, firebase-key) for API connectivity and database access.

Author: David Muhič
