# Fault-Tolerant Cloud P2P Image Sharing System

A robust, decentralized environment for secure image sharing. This project focuses on transparency, load balancing, fault tolerance, and P2P services within a cloud-based infrastructure.

## Live Demo
[Watch the P2P Sharing System Demo](https://drive.google.com/file/d/1Hzn6aJQsCtR7z_clN1RYOOVJwEcmDz03/view?usp=drivesdk)

## Key Features

* **Raft-Based Consensus:** Implements a custom **Raft algorithm** to manage a 3-node server cluster, handling leader elections, heartbeats, and cluster state synchronization.
* **Metric-Aware Load Balancing:** Dynamically routes encryption workloads by calculating server "health scores" based on real-time **CPU load, active connections, and latency telemetry**.
* **Secure Steganography:** Utilizes a **Least Significant Bit (LSB)** engine to embed **Bincode-serialized** permission metadata and view quotas directly into image bitstreams.
* **Replicated Directory Service:** A persistent discovery service for user registration and peer reachability, featuring **JSON-based disk persistence** and state-sync recovery.
* **Asynchronous P2P Protocol:** Optimized for large file transfers with **TCP socket buffer tuning** (SO_SNDBUF/RCVBUF) and asynchronous I/O via the **Tokio runtime**.
* **Fault Tolerance Simulation:** Periodic node failures are simulated to test the cluster's ability to recover and remain consistent upon revival.



## System Architecture

### 1. The Cloud (Server Cluster)
The cloud consists of three server peers that utilize a distributed election algorithm to manage incoming workloads. It provides two primary services::
* **Encryption Service:** Uses steganography to embed user permissions and view quotas into images.
* **Load Balancing & Fault Tolerance:** Servers elect a "worker" for incoming tasks using a distributed election algorithm based on current system parameters.



### 2. Directory Service (Discovery)
Users register with this service when online to discover peers and reach them directly. It supports:
* **Consistency:** The peer table is kept consistent across the cloud servers.
* **Offline Support:** A best-effort policy manages permission updates for offline owners or viewers.

### 3. P2P Client & Permissions
* **Discovery Service:** Users can inquire with the discovery service for online peers and Directly request low-resolution thumbnails or full images from peers.
* **Controlled Sharing:** Users can only view their own images or images where their username is hidden in the metadata.
* **Quota Enforcement:** Each view decrements a quota stored *inside* the image. Access is denied (replaced by a default image) once the quota is consumed.
* **Owner Control:** Owners can dynamically add/remove users or change viewing quotas.

## Tech Stack

* **Backend:** Rust (Tokio for async networking, Serde/Bincode for serialization).
* **Frontend:** React and Tauri (Cross-platform desktop integration).
* **Networking:** Custom TCP-based P2P protocol, Raft Consensus, Multicast-ready client middleware.
* **Security:** LSB Steganography for decentralized access control.

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
