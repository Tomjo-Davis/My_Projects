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

Additionally, CAI is used to:
- **Push messages to JMS Queues**  
- **Iterate file lists and trigger taskflows dynamically**

---

## 🧩 List of POCs Demonstrated

### ✔ Dynamic Taskflow Trigger (file-driven) using **CDI**
### ✔ Dynamic Taskflow Trigger (file-driven) using **CAI**
### ✔ JMS Queue Integration – push messages using **CAI**

---

# 1. Dynamic Taskflow Trigger Using CDI (Cloud Data Integration)

This approach uses a **File Watcher** object to continuously monitor a file path and automatically trigger a Taskflow when files arrive.

---

## 🔁 Mechanism Overview

1. **File Watcher monitors a directory**  
2. When a file **arrives**, a **Found Event** is generated  
3. The FileWatcher event **triggers the Taskflow**  
4. If no file is present, the watcher simply waits

---

## ⚙ CDI Components & Configuration

### **File Watcher**  
**Name:** `FL_CLIENT_POC_2024` (genericized)

**Workflow:**
- Execute File Watcher
- Drop a file into the monitored path
- Taskflow fires automatically

**Schedule:**
- Frequency: **Daily**
- Polling: **Every 15 seconds**

**Listener Rules:**
- Event: **File Arrival**
- Pattern type: **Wildcard**
- File pattern: `*.TXT`

---

### **Taskflow**
**Name:** `tf_client_to_XML` (genericized)

**Key Configuration:**
- Trigger type: **Event-Based**
- Event Source: `fileListenerTask:FL_CLIENT_POC_2024`

---

# 2. Dynamic Taskflow Trigger Using CAI (Cloud Application Integration)

This approach relies on a CAI process that **reads a list of files**, processes them sequentially, and dynamically triggers the Taskflow for each file.

Ideal for:
- Multiple file processing  
- Iterative or loop-based orchestration  
- Complex file-handling logic

---

## 🔁 Mechanism Overview

1. A **file list** is created (from a directory or file)  
2. The list is passed to a **CAI process**  
3. CAI calls the Taskflow for each file  
4. A **loop** continues until the list is fully processed  

---

## ⚙ CAI Components

### **🔌 Service Connector**  
`SC_RESTAPI`

Used to call Informatica REST API and run the Taskflow dynamically.

---

### **🔗 App Connection**
`App-RESTAPI`

Holds API endpoints and authentication.

---

### **🟦 CAI Process**
`ClientName-list-Process` (genericized)

This CAI process performs the primary orchestration.

### **Process Logic**

- Load file content from a path  
  *(example path genericized from original: `/infdata/Targetfiles/.../STAGING/MPL.lsf`)*  
- Split file contents into a **list** (e.g., via newline delimiter)
- Use decision step:  
  - **IF** list is NOT empty → process next file  
  - **ELSE** terminate
- For each file in list:
  - Extract head value  
  - Trigger Taskflow via **TaskflowRunID** REST action  
  - Append the file to processed list  
  - Remove item from temp list  
  - Loop continues

---

### **🟩 Taskflow Triggered by CAI**
`Taskflow_ClientName_CAI` (genericized)

**Inputs:**
- `src_ID` (file name or file content sliced from list)

**Contains:**
- A Data Task  
- Any transformation rules required

---

### **🔌 REST API Action in Service Connector**
Action Name: `TaskflowRunID`  
HTTP Method: **GET**

Used to trigger Taskflow dynamically via endpoint.

---



