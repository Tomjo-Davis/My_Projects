
TF Trigger POC: Dynamic Data Integration and Application Integration
This repository contains materials related to a Proof of Concept (POC) demonstrating automatic task flow triggering based on file availability and the functionality of pushing messages to a JMS queue within Informatica Cloud.

The overall solution leverages Informatica Cloud, utilizing both Cloud Data Integration (CDI) and Cloud Application Integration (CAI) components. The primary objective is to dynamically call an Informatica Task Flow when files become available in a designated file path on the server. Data flows from source Files through the Informatica Cloud components (CDI/CAI) to a defined Output.

List of POCs Demonstrated
Dynamic Task Flow trigger based on file availability using CDI.
Dynamic Task Flow trigger based on file availability using CAI.
Pushing Messages to JMS Queue using CAI.
1. Dynamic Task Flow Trigger using CDI
This method uses a File Watcher object within CDI to monitor a file location and automatically trigger a Task Flow upon file arrival.

Mechanism Overview
The process begins with a File Watcher monitoring a specific Location.

If Files are detected, the system executes a Found-Trigger the event, which subsequently Triggers the Task flow.
If No Files are detected, the system will Wait.
CDI Artifacts and Configuration
The components used in this method include:

File Watcher: FL_CLIENT_POC_2024 (Genericized name).
Steps: Execute the File watcher, drop a file to the designated path, and the Taskflow is triggered automatically.
Schedule: The watcher is configured to run Daily, checking every 15 Seconds.
Listener Rules: It notifies when files Arrive, uses a Wildcard pattern type, and specifically looks for files with the extension .TXT.
Taskflow: tf_client_to_XML (Genericized name).
The Taskflow is designed to start based on an Event, specifically bound to the File Watcher event source, fileListenerTask:FL_CLIENT_POC_2024.
2. Dynamic Task Flow Trigger using CAI
This method utilizes Informatica Cloud Application Integration (CAI) processes to handle a list of files and dynamically trigger the Task Flow, allowing for processing multiple files.

Mechanism Overview
The CAI method involves listing the files, processing them sequentially, and looping until the list is exhausted.

A File List is created.
The list feeds into a CAI process.
The CAI process calls the Task flow.
The system implements a Loop till end of filelist.
CAI Artifacts
Key artifacts configured for this POC include:

Service Connector: SC_RESTAPI
App Connection: App-RESTAPI
Process: ClientName-list-Process (Genericized name, used for listing and iterating files)
Taskflow: Taskflow_ClientName_CAI (Genericized name)
CAI Process Details
The CAI process handles list management and task flow execution:

Starts with an assignment to load a temporary list (tmp_list). This assignment involves reading file content from a path (genericized path: /infdata/Targetfiles/NGBO/MPL/STAGING/MPL.lsf in the source) and splitting records by newline character to create the list of records (<record>).
A decision step checks if the temporary list is empty (is not empty / is empty).
If not empty, the process assigns the head of the list, triggers the task flow using TaskflowRunID, assigns results to a list (POC_List), and then updates the temporary list (temp_list). This structure facilitates iteration.
The Taskflow_ClientName_CAI Taskflow includes a Data Task and handles input fields like src_ID (which uses a field input file value).

The Service Connector (SC_RESTAPI) includes an action TaskflowRunID. This action uses a GET verb and a URL that points to the task flow, allowing the CAI process to trigger it dynamically.
