# TF Trigger POC – Dynamic Task Flow Triggering & JMS Messaging in Informatica Cloud

This repository contains the implementation and documentation for a Proof of Concept (POC) demonstrating:

- **Automatic Taskflow triggering based on file availability**, using both **Informatica Cloud Data Integration (CDI)** and **Cloud Application Integration (CAI)**.
- **Pushing messages to a JMS Queue** using CAI.
- A complete end-to-end integration pattern leveraging **file events**, **REST API triggers**, **list processing**, and **dynamic taskflow execution**.

The objective of this POC is to show how Informatica Cloud can orchestrate dynamic workflows based on incoming data files and integrate with enterprise systems via JMS.

---

## 📌 Overview

The solution proactively monitors file locations (CDI FileWatcher / CAI file list reader) and automatically triggers an **Informatica Taskflow** whenever a new file arrives.

Data flows from:
**Source Files ➜ Informatica Cloud (CDI / CAI) ➜ Taskflow ➜ Output System**

Two separate approaches are demonstrated:

1. **Dynamic Taskflow Triggering using CDI**
2. **Dynamic Taskflow Triggering using CAI**
---

## 🧩 List of POCs Demonstrated

###  Dynamic Taskflow Trigger (file-driven) using **CDI**
###  Dynamic Taskflow Trigger (file-driven) using **CAI**




