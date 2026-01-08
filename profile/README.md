Sprouty — Intelligent Plant Companion

Sprouty is an AI-driven microservices platform designed to bridge the gap between IoT environmental data and botanical care. Developed as a solo project for the Software Development Processes (PRPO) 2025/2026 course at the University of Ljubljana, Faculty of Computer and Information Science (FRI).

Table of Contents

Vision & MVP Scope

System Architecture

Cloud Infrastructure & DevOps

API Documentation

Future Roadmap

🌟 Vision & MVP Scope
The initial design envisioned an interconnected plant ecosystem. To ensure a high-quality delivery within the course timeline, the MVP/current state of the project focuses on the critical "Sense-Identify-Notify" loop.

Current MVP Features:
AI Plant Identification: High-accuracy species recognition and care instruction generation.

Live plant environment data: Real-time monitoring of soil and atmosphere using ESP32-S3 microcontroller with a DHT11 sensor for air humidity and temperature and an analog soil mositure sensor.

Plant care alerts: Threshold-based notifications to improve plant's survivability and wellbeing.

Secure accounts: Isolated user gardens and encrypted authorization using .

🏗 System Architecture
The project has a microservicee architecture - each service is decoupled, allowing for independent scaling and maintenance.

The Connected Service Mesh:
Gateway Service: It handles JWT Validation and routes traffic. It prevents unauthorized access to sensitive plant data.

User Service: Integrated with Firebase Auth to manage user identities and profiles.

Plant Service: Combines Pl@ntNet API (visual recognition) with OpenAI GPT-3.5 turbo to generate human-readable care tips.

Sensor Service: Ingests data from IoT devices, comparing live metrics against the thresholds defined in the Plant Service.

Notification Service: Dispatches push notifications via FCM when the Sensor Service detects anomalies.

☁️ Cloud Infrastructure & DevOps
The project is architected for speed and cost-efficiency:

Deployment: Hosted on GKE Autopilot. This removes the need for manual node management while ensuring the app scales automatically based on CPU/RAM load.

Regional Optimization: All resources (GKE, Firestore, Firebase Storage) are pinned to europe-west4 (Netherlands). This minimizes latency for users in Slovenia and reduces inter-region data egress costs.

CI/CD Pipeline: 1. GitHub Actions triggers on every push to main. 2. Automatic Maven builds and unit testing. 3. Docker images are built and pushed to Google Artifact Registry. 4. Kubernetes manifests are applied, performing a zero-downtime rolling update.

📖 API Documentation
Full technical specifications of the internal API are available via Swagger/OpenAPI.

Gateway (Aggregated): http://<external-ip>/swagger-ui.html

Plant Service Specs: http://plant-service:8082/v3/api-docs

User Service Specs: http://user-service:8081/v3/api-docs

🚀 Future Roadmap
The architecture is already "future-proofed" for the remaining features of the original proposal:

Lifecycle Service: Building time-lapse videos from sensor-sent photos to track growth patterns scientifically.

Shop Service: An intelligent marketplace recommending vases, soil, and fertilizers specific to the plants the user actually owns.

Community Service: A social platform for sharing time-lapses and care success stories once the user base scales.

🛠 Local Setup
To run a local instance of the Sprouty backend:

Clone the organization repository: git clone https://github.com/sprouty-org/sprouty-main

Install dependencies: mvn clean install

Configure your firebase-key.json in the root of each service.

Apply Kubernetes configurations: kubectl apply -f k8s/

Author: David Muhič

Organization: Team Sprouty
