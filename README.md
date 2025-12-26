# nd2-learning-journey-2025

## 📖 Introduction 

This repository serves as a comprehensive learning journal and technical review document, consolidating research data on architecting full-stack automation systems. As the industrial landscape shifts toward Industry 4.0, this document explores the convergence of Information Technology (IT) and Operational Technology (OT), providing a blueprint for implementing software engineering best practices within the physical constraints of the factory floor.


## 📌 Contents Overview

1.  [DevOps Concepts for Automation Systems](#devops-concepts-for-automation-systems)
    
2.  [Front-End and Back-End Development](#front-end-and-back-end-development)
    
3.  [Data Processing and AI Pipelines](#data-processing-and-ai-pipelines)
    
4.  [Deployment and Automation Workflow](#deployment-and-automation-workflow)
    
5.  [Future Works and Emerging Trends](#future-works-and-emerging-trends)
    

## DevOps Concepts for Automation Systems 


### Technical Review & Best Practices

The transition to "Industrial DevOps" addresses code fragmentation and reliability issues in OT.

-   **The "Build Once" Philosophy:** Ensuring that the exact binary artifact tested in the lab is what runs on the production motor. Artifacts are versioned and stored in registries (e.g., JFrog or Docker Registry) rather than being recompiled on-site.
    
-   **Security-First (Shift Left):** Integrating **Static Application Security Testing (SAST)** and dependency scanning into the pipeline to catch vulnerabilities in Python scripts or C++ firmware before they reach the factory.
    
-   **Progressive Delivery:** Implementing **Blue/Green** or **Canary Deployments** for Edge Gateways. This allows for automated rollbacks if system metrics (memory, error rates) deviate during a new version rollout.
    
-   **HIL (Hardware-in-the-Loop) Testing Stages:**
    
    1.  **Linter:** Checking adherence to MISRA C standards.
        
    2.  **Cross-Compilation:** Building for ARM/Target architecture on x86 agents.
        
    3.  **Flash:** Automated flashing via J-Link/ST-Link.
        
    4.  **Assertion:** Comparing physical outputs (PWM, Voltage) against expected profiles.
        
<img width="640" height="493" alt="image" src="https://github.com/user-attachments/assets/06bf4c70-2ee8-45ad-81e3-9b716ae19fe8" />

>  - **Figure (a):** A diagram illustrating the **closed-loop feedback** between an embedded computer and an HIL simulator to mimic real-world interactions.
 > -   **Figure (b):** A detailed view of an HIL simulator's **internal architecture**, showing how physics-based models interface with physical hardware through I/O modules.

## Front-End and Back-End Development 

### Modern Tech Stacks for Industry

Industrial dashboards must handle high-frequency data (100+ updates/sec) without freezing the UI.

-   **Front-End Deep Dive:**
    
    -   **React:** Uses a Virtual DOM. Excellent for large enterprise apps but requires optimization (e.g., `React.memo`) to prevent excessive re-renders during high-speed data streams.
        
    -   **Vue.js:** Favored for embedded HMIs due to its **Proxy-based reactivity**. It tracks dependencies precisely, updating only the specific text node affected by a sensor change, leading to better out-of-the-box performance for low-power devices.
        
-   **Back-End Performance:**
    
    -   **Go (Golang):** Utilizing **Goroutines** to poll thousands of Modbus/TCP registers concurrently with minimal memory overhead.
        
    -   **Node.js:** Best for managing massive WebSocket connection pools for real-time client updates.
        
-   **Protocol Implementation:**
    
    -   **Modbus TCP:** Implementing robust "forever loops" for TCP reconnection.
        
    -   **OPC UA:** Prioritizing **Subscriptions** over Polling to reduce network bandwidth, allowing the server to push updates only on value changes.
        
<img width="457" height="339" alt="image" src="https://github.com/user-attachments/assets/f593241d-a232-4c1e-a4e2-756f512932d7" />

> **Figure:** A system architecture diagram showing the **data flow from a Linked Data Endpoint** through a PHP backend to an interactive frontend with various visualization widgets.

## Data Processing and AI Pipelines 


### From Raw Data to Actionable Intelligence

Moving away from "Spaghetti Architecture" toward a Unified Namespace (UNS).

-   **UNS Architecture:** A hierarchical "Single Source of Truth" following **ISA-95** (Enterprise/Site/Area/Line/Cell). Every device publishes to a central MQTT Broker (HiveMQ or EMQX).
    
-   **MQTT Sparkplug B:**
    
    -   **Protobuf Encoding:** Drastically reduces payload size compared to JSON.
        
    -   **Session Management:** Uses "Birth" and "Death" certificates to ensure subscribers always know the real-time connectivity status of an edge device.
        
-   **AI for Predictive Maintenance:**
    
    -   **LSTM Autoencoders:** Trained on "Normal" data. By calculating the **Mean Absolute Error (MAE)** between input and reconstruction, the system identifies anomalies without needing prior failure data.
        
-   **Medallion Architecture:** Data flows from **Bronze** (Raw) → **Silver** (Cleaned/Enriched) → **Gold** (Aggregated KPIs like OEE).

<img width="640" height="216" alt="image" src="https://github.com/user-attachments/assets/5be6bc88-e976-4283-b92e-7b59c63c4f0c" />

> **Figure:** A comprehensive workflow diagram for **anomaly detection**, illustrating the process from image preprocessing and model training to **anomaly score calculation** and final **defect detection** based on a selected threshold.

## Deployment and Automation Workflow

### Orchestrating the Edge

Managing software across hundreds of devices requires cloud-native methodologies suited for the edge.

-   **Docker in OT:** Provides isolation for modern AI services on legacy Windows IPCs, preventing "Dependency Hell."
    
-   **K3s vs. MicroK8s:**
    
    -   **K3s:** Lightweight (<100MB), stripped of cloud plugins, ideal for ARM/Raspberry Pi gateways.
        
    -   **MicroK8s:** Zero-ops Kubernetes, better for high-availability clusters with easy-to-use add-ons.
        
-   **GitOps (Pull-Based Model):** Using **FluxCD**. Unlike traditional push models, FluxCD agents inside the factory pull updates from Git. This bypasses firewall issues and ensures the device resumes its state automatically after a network outage.
    
![Architecture](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fuhj2krev2wsbpzste9b6.png)

> **Figure:** A diagram of a **DevSecOps pipeline** showing the automated flow from GitHub code commits through SonarQube scanning, Docker image building, and Trivy vulnerability scanning before deployment to AWS ECS.

## Future Works and Emerging Trends


### The Road to Industry 5.0

Exploring the hyper-connected future of manufacturing.

-   **Industrial Metaverse:** Utilizing **NVIDIA Omniverse** and **OpenUSD** to create physics-accurate simulations. This allows for testing production line changes virtually before any physical implementation.
    
-   **5G URLLC:** Ultra-Reliable Low-Latency Communication (<1ms) enabling wireless closed-loop control, removing the need for physical cables in mobile robot logic.
    
-   **Edge MLOps:** Creating a continuous learning loop where "hard samples" (uncertain AI predictions) are sent back to the cloud for labeling and retraining, then pushed back to the edge.


