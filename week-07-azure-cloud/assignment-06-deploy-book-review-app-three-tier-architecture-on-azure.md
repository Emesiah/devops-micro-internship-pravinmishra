# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![alt text](screenshots/week-07-Assignment-06-screenshot1.png)

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

Written Architecture Assumptions and Selected Azure Services
The Book Review App will use a three-tier Azure architecture.
•	Presentation/Web Tier: Azure Application Gateway will be the public entry point for users.
•	Application Tier: An Azure Linux VM will host the Book Review backend application.
•	Database Tier: Azure Database for MySQL Flexible Server will store the application data.
•	Network: Azure Virtual Network (VNet) will separate the tiers into different subnets.
•	Security: Network Security Groups (NSGs) will control traffic between the tiers.
•	Database Security: MySQL Flexible Server will use private access with public access disabled.
•	Monitoring: Azure Monitor will be used for monitoring and logging.
•	Backup: MySQL Flexible Server backups will be enabled to support recovery.

Selected Azure Services

Component	         Selected Service
Public Entry Point	 Azure Application Gateway
Web/Application	     Azure Linux VM
Database	         Azure Database for MySQL Flexible Server
Network	             Azure Virtual Network
Network Security	 Network Security Groups
Monitoring	         Azure Monitor
Backup	             MySQL Flexible Server Backup
Traffic Flow
Internet → Application Gateway → Application VM → MySQL Flexible Server

Architecture Screenshot:
![alt text](screenshots/week-07-Assignment-06-screenshot2.png)



---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

![alt text](screenshots/week-07-Assignment-06-screenshot3.png)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

![alt text](screenshots/week-07-Assignment-06-screenshot4.png)

---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

![alt text](screenshots/week-07-Assignment-06-screenshot5.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

![alt text](screenshots/week-07-Assignment-06-screenshot6.png)

---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

![alt text](screenshots/week-07-Assignment-06-screenshot7.png)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

![alt text](screenshots/week-07-Assignment-06-screenshot8.png)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

![alt text](screenshots/week-07-Assignment-06-screenshot9.png)

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

![alt text](screenshots/week-07-Assignment-06-screenshot10.png)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

![alt text](screenshots/week-07-Assignment-06-screenshot11.png)

---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

![alt text](screenshots/week-07-Assignment-06-screenshot12.png)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

![alt text](screenshots/week-07-Assignment-06-screenshot13.png)

---

#### Screenshot 14 — Availability, backup, and retention configuration

![alt text](screenshots/week-07-Assignment-06-screenshot14.png)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

![alt text](screenshots/week-07-Assignment-06-screenshot15.png)

---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

![alt text](screenshots/week-07-Assignment-06-screenshot16.png)![alt text](screenshots/week-07-Assignment-06-screenshot16-1.png)![alt text](screenshots/week-07-Assignment-06-screenshot16-2.png)

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

![alt text](screenshots/week-07-Assignment-06-screenshot17.png)![alt text](screenshots/week-07-Assignment-06-screenshot17-1.png)![alt text](screenshots/week-07-Assignment-06-screenshot17-2.png)

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

![alt text](screenshots/week-07-Assignment-06-screenshot18.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

![alt text](screenshots/week-07-Assignment-06-screenshot19.png)![alt text](screenshots/week-07-Assignment-06-screenshot19-1.png)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

![alt text](screenshots/week-07-Assignment-06-screenshot20.png)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

![alt text](screenshots/week-07-Assignment-06-screenshot21.png)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

![alt text](screenshots/week-07-Assignment-06-screenshot22.png)![alt text](screenshots/week-07-Assignment-06-screenshot22-1.png)

---

#### Public Endpoint

http://4.248.7.18/

(http://4.248.7.18/)

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

The Book Review App was successfully deployed on Azure with Application Gateway as the public entry point and MySQL Flexible Server as the private database.
The main issues were the **502 error, incorrect backend IP/port, and stopped Node.js service**, which were fixed by starting the backend and configuring port **8080** correctly.
Security was improved by keeping the backend and database private, using SSL, and protecting credentials.
Application Gateway health checks, Azure Monitor, managed backups, and safe availability tests were used to improve reliability.


---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [ ] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [ ] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [ ] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [ ] Task 4: Presentation tier deployed (Screenshots 8–9)
- [ ] Task 5: Application tier deployed privately (Screenshots 10–12)
- [ ] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [ ] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [ ] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
