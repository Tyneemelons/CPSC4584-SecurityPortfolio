# Week 01 — Maplewood SOC Onboarding Lab

## Overview
This lab introduced basic terminal navigation and file inspection skills used in a Security Operations Center (SOC). I practiced identifying my working directory, listing files with permissions, and reading file contents to understand how analysts gather information during an investigation.

---

## Terminal Entry 1 — Print Working Directory

### **Command You Typed**
`pwd`

### **Terminal Output**
`/home/student`

### **Analyst Observation**
Knowing the current working directory helps an analyst understand where they are in the system and what files or logs are accessible. It also ensures commands are run in the correct location, which is important when investigating suspicious activity.

---

## Terminal Entry 2 — List Directory Contents

### **Command You Typed**
`ls -la`

### **Terminal Output**
drwxr-xr-x 4 student student 4096 Sep 3 14:20 ..
-rw-r--r-- 1 student student   23 Sep 3 14:22 notes.txt
-rw-r--r-- 1 student student  120 Sep 3 14:21 config.log


### **Analyst Observation**
The directory listing reveals important details for an investigation. File permissions (such as `rw-r--r--`) show who can read or modify each file, helping analysts identify potential misconfigurations or unauthorized access. Timestamps indicate when files were last changed, which can help correlate file activity with the timeline of a security incident.

---

## Terminal Entry 3 — Read a File

### **Command You Typed**
`cat notes.txt`

### **Terminal Output**
System check complete.
No errors found.


### **Analyst Observation**
I chose to read `notes.txt` because it was a readable text file that could contain system information or recent activity. The contents showed a simple system check message, which helps confirm nothing unusual was reported at the time. Reviewing file contents matters because attackers often hide clues, scripts, or altered data inside ordinary text files. Even small files can reveal when a system was checked, what processes ran, or whether anything suspicious was logged.

---

## Summary
This onboarding lab helped reinforce foundational SOC skills: navigating directories, inspecting file metadata, and reviewing file contents. These actions form the basis of incident investigation and allow analysts to build a clear picture of system activity.

