# ros2-os-analysis

# ROS 2 Architecture: An Operating Systems Perspective

This repository contains a high-level architectural analysis of **ROS 2 (Robot Operating System)** evaluated through core operating systems principles. It explores how ROS 2 builds abstractions on top of standard OS kernels to manage processes, hardware interfaces, communication, and scheduling.

## 📺 Project Presentation Video (10 Min)
Click the image below to watch the video defense where I explain these concepts in detail:

[![Watch the video]([[https://img.youtube.com/vi/YOUR_VIDEO_ID/0.jpg](https://uses0-my.sharepoint.com/personal/iregonqui_alum_us_es/_layouts/15/stream.aspx?id=%2Fpersonal%2Firegonqui%5Falum%5Fus%5Fes%2FDocuments%2FIRENE%20GONZALEZ%20QUIROS%20%2D%20ROS%20OS%20PROJECT%2Emp4&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0RpcmVjdCJ9fQ&referrer=StreamWebApp%2EWeb&referrerScenario=AddressBarCopied%2Eview%2E308e1a93%2Dcbb5%2D4d9d%2D9e89%2Df81e38ab01c6))](https://uses0-my.sharepoint.com/personal/iregonqui_alum_us_es/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Firegonqui%5Falum%5Fus%5Fes%2FDocuments%2FCaptura%20de%20pantalla%202026%2D07%2D13%20203306%2Epng&parent=%2Fpersonal%2Firegonqui%5Falum%5Fus%5Fes%2FDocuments&nav=eyJkZWZhdWx0TmF2UGFuZWwiOnsicGx1Z2luTmFtZSI6Ik1lZGlhU2V0dGluZ3NMYXllciJ9LCJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJVcGxvYWREaWFsb2dPcGVuQnV0dG9uIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D)]([https://www.youtube.com/watch?v=YOUR_VIDEO_ID](https://uses0-my.sharepoint.com/personal/iregonqui_alum_us_es/_layouts/15/stream.aspx?id=%2Fpersonal%2Firegonqui%5Falum%5Fus%5Fes%2FDocuments%2FIRENE%20GONZALEZ%20QUIROS%20%2D%20ROS%20OS%20PROJECT%2Emp4&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0RpcmVjdCJ9fQ&referrer=StreamWebApp%2EWeb&referrerScenario=AddressBarCopied%2Eview%2E308e1a93%2Dcbb5%2D4d9d%2D9e89%2Df81e38ab01c6))

---

## 📄 Project Document
The original presentation slide deck is available for review:
👉 **[Read the Project Presentation PDF](./Presentación%20Proyecto%20ROS%20-%20IRENE%20GONZALEZ%20QUIROS.pdf)**

---

## 🔍 Core Topics Covered

This project bridges theoretical OS concepts learned in class with the practical design of ROS 2, focusing on three main areas:

### 1. System Structure & Modularity (Microkernel vs. Monolithic)
* **The Concept:** Monolithic kernels (like Linux) offer high execution efficiency but lack flexibility. ROS 2 acts as user-space middleware, mimicking a **Microkernel** approach over a monolithic architecture.
* **Isolation:** Individual hardware drivers and components run isolated in User Mode. If a driver crashes, it throws an isolated software exception without crashing the underlying OS kernel.
* **The VFS Analogy:** Just as a Virtual File System (VFS) abstracts different storage media behind uniform system calls (`read`/`write`), the **ROS Client Library (RCL)** abstracts diverse robotic hardware behind a standardized interface (`publish`/`subscribe`).

### 2. Inter-Process Communication (DDS vs. Classical IPC)
* **Anonymous & Scalable:** While classical OS pipes are restricted to 1-to-1 FIFO byte streams, ROS 2 utilizes DDS (Data Distribution Service) for data-centric, many-to-many communication.
* **Transparent Architecture:** The underlying mechanisms change adaptively based on deployment without modifying application code:
  * **Same Machine:** Utilizes **Shared Memory** segments to bypass kernel-space copy overheads (crucial for heavy data like LiDAR or video frames).
  * **Remote Machines:** Automatically switches to network **Sockets (UDP/IP)** through the standard kernel network stack.
* **Communication Paradigms:** Analyzes standard non-blocking *Topics*, alongside synchronous/blocking *Services* and asynchronous *Actions*.

### 3. Two-Level Scheduling (OS Scheduler vs. ROS 2 Executors)
* **The Problem:** The Linux kernel scheduler (e.g., CFS) views a ROS 2 node simply as a standard process or thread. It allocates CPU time slices based on overall system priorities but remains blind to what tasks are pending inside that node.
* **The Solution:** ROS 2 introduces **Executors**, which act as a user-space scheduler running on top of the OS scheduler. 
* Once the CPU assigns execution time to the node process, the Executor steps in to coordinate and prioritize internal callbacks (timers, incoming messages, or service requests) to ensure deterministic robotic behavior.

---

## 🛠️ Context
* **Course:** Operating Systems
* **Author:** Irene González Quirós
