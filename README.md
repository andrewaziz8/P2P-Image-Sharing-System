# Fault-Tolerant Cloud P2P Image Sharing System

[cite_start]A robust, decentralized environment for secure image sharing, developed as part of the **CSCE 4411: Distributed Systems** course[cite: 3, 7]. [cite_start]This project focuses on transparency, load balancing, fault tolerance, and P2P services within a cloud-based infrastructure[cite: 12].

## Key Features

* [cite_start]**Raft-Based Consensus:** Implements a custom **Raft algorithm** to manage a 3-node server cluster, handling leader elections, heartbeats, and cluster state synchronization[cite: 34].
* [cite_start]**Metric-Aware Load Balancing:** Dynamically routes encryption workloads by calculating server "health scores" based on real-time **CPU load, active connections, and latency telemetry**[cite: 35].
* [cite_start]**Secure Steganography:** Utilizes a **Least Significant Bit (LSB)** engine to embed **Bincode-serialized** permission metadata and view quotas directly into image bitstreams[cite: 14, 21].
* [cite_start]**Replicated Directory Service:** A persistent discovery service for user registration and peer reachability, featuring **JSON-based disk persistence** and state-sync recovery[cite: 17, 39, 42].
* **Asynchronous P2P Protocol:** Optimized for large file transfers with **TCP socket buffer tuning** (SO_SNDBUF/RCVBUF) and asynchronous I/O via the **Tokio runtime**.
* [cite_start]**Fault Tolerance Simulation:** Periodic node failures are simulated to test the cluster's ability to recover and remain consistent upon revival[cite: 36, 38].



## System Architecture

### 1. The Cloud (Server Cluster)
[cite_start]The cloud provides two primary services and utilizes a P2P architecture for decision-making[cite: 32]:
* [cite_start]**Encryption Service:** Uses steganography to protect images and embed access controls[cite: 14].
* [cite_start]**Load Balancing:** Servers elect a "worker" for incoming tasks using a distributed election algorithm based on current system parameters[cite: 34, 35].



### 2. Directory Service (Discovery)
[cite_start]Users register with this service when online to discover peers and reach them directly[cite: 39, 40]. It supports:
* [cite_start]**Consistency:** The peer table is kept consistent across the cloud servers[cite: 42].
* [cite_start]**Offline Support:** A best-effort policy manages permission updates for offline owners or viewers[cite: 25, 26].

### 3. P2P Client & Permissions
* [cite_start]**Controlled Sharing:** Users can only view their own images or images where their username is hidden in the metadata[cite: 22].
* **Quota Enforcement:** Each view decrements a quota stored *inside* the image. [cite_start]Access is denied (replaced by a default image) once the quota is consumed[cite: 23].
* [cite_start]**Owner Control:** Owners can dynamically add/remove users or change viewing quotas[cite: 24].

## Tech Stack

* **Backend:** Rust (Tokio for async networking, Serde/Bincode for serialization).
* **Frontend:** React and Tauri (Cross-platform desktop integration).
* [cite_start]**Networking:** Custom TCP protocol, Raft Consensus, Multicast-ready client middleware[cite: 33].

## Getting Started

### Prerequisites
* Rust (latest stable)
* Node.js & npm (for the Tauri/React GUI)

### Installation
1. Clone the repository:
   ```bash
   git clone [https://github.com/andrewaziz8/P2P-Image-Sharing-System.git](https://github.com/andrewaziz8/P2P-Image-Sharing-System.git)
   cd P2P-Image-Sharing-System
2. Install frontend dependencies:
   ```bash
   npm install

### Running the System
1. Start Directory Servers:
   ```bash
   cargo run --bin directory_server <port> <server_id> [peer_addresses...]
2. Start Cloud Servers:
   ```bash
   cargo run --bin server <port> <server_id> [peer_addresses...]
3. Launch the P2P Client:
   ```bash
   cargo run --bin client -- start-peer --username <name> --port <p2p_port>

## Evaluation
* The project includes extensive documentation on design decisions, performance measurements, and stress testing to ensure the system's statistical viability under heavy load.
