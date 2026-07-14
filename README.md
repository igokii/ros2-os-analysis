# ros2-os-analysis

# ROS 2 Architecture: An Operating Systems Perspective

This repository contains a high-level architectural analysis of **ROS 2 (Robot Operating System)** evaluated through core operating systems principles. It explores how ROS 2 builds abstractions on top of standard OS kernels to manage processes, hardware interfaces, communication, and scheduling.

## Project Presentation Video (10 Min)
This is the video defense where I explain these concepts in detail:

> ### [Watch the Video Presentation on Clipchamp (Click Here) ↗](https://uses0-my.sharepoint.com/personal/iregonqui_alum_us_es/_layouts/15/stream.aspx?id=%2Fpersonal%2Firegonqui%5Falum%5Fus%5Fes%2FDocuments%2FIRENE%20GONZALEZ%20QUIROS%20%2D%20ROS%20OS%20PROJECT%2Emp4&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0RpcmVjdCJ9fQ&referrer=StreamWebApp%2EWeb&referrerScenario=AddressBarCopied%2Eview%2E308e1a93%2Dcbb5%2D4d9d%2D9e89%2Df81e38ab01c6)

---

## Project Document
The original presentation slide deck is available for review:
 **[Read the Project Presentation PDF](https://github.com/igokii/ros2-os-analysis/blob/3912558f84d5baadc1ec786d66aaf57cc20ae7d3/ROS2%20Project%20Presentation.pdf)**

---

## Core Concepts Summary

This analysis connects theoretical operating systems concepts with the practical design of ROS 2 through three main areas:

### 1. System Structure and Modularity (Microkernel vs. Monolithic)
ROS 2 runs entirely as user-space middleware, mimicking a microkernel-like philosophy on top of a monolithic system like Linux. This architecture ensures that if a hardware driver or node crashes, it triggers an isolated software exception rather than causing a kernel panic. This abstraction model is highly comparable to a Virtual File System (VFS), wrapping diverse robotic hardware under a single, unified interface.

### 2. Inter-Process Communication (DDS vs. Classical IPC)
Instead of relying on traditional OS pipes, which are too restrictive for complex robotic networks, ROS 2 adopts the DDS standard for many-to-many communication. The underlying transport layer adapts dynamically and remains transparent to the developer: it utilizes shared memory segments for local processes to avoid heavy data-copying overhead (crucial for LiDAR or video), and automatically transitions to standard network sockets (UDP/IP) when communicating between different physical robots.

### 3. Two-Level Scheduling (OS Scheduler vs. ROS 2 Executors)
A standard Linux scheduler is completely blind to the specific tasks waiting inside a ROS 2 node; it simply views the node as a generic process and allocates CPU time. To solve this and guarantee deterministic robotic behavior, ROS 2 introduces Executors. Operating in user space, Executors act as a second-level scheduler, coordinating and prioritizing the node's internal tasks once the host operating system hands over CPU execution.

---

## 🛠️ Context
* **Course:** Sistemas Operativos - Universidad de Sevilla
* **Author:** Irene González Quirós
