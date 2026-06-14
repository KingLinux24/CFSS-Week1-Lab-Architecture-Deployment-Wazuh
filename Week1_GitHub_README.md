# 🛡️ CFSS Global Internship 2026 — SOC Operations & Threat Detection Mastery

**Intern:** Israel Mbiyavanga David  
**Domain:** SOC Analyst & Blue Teaming  
**Duration:** 4 Weeks (Project-Based)  
**Company:** CFSS (Cyber Security & Forensics Services)

---

## Week 1 — Lab Architecture & Deployment

### 🎯 Objective
Build a functional Security Operations Center (SOC) environment using open-source tools, simulating a real-world monitoring infrastructure.

---

### 🖥️ Lab Environment

| Component | Details |
|-----------|---------|
| **Hypervisor** | Oracle VirtualBox |
| **VM 1 — SIEM Server** | Ubuntu 22.04 LTS (Wazuh Manager + Indexer + Dashboard) |
| **VM 2 — Endpoint** | Windows 10 Home Single Language (Wazuh Agent) |
| **Network** | Host-Only Adapter — `192.168.134.x` subnet |
| **Server IP** | 192.168.134.133 |
| **Wazuh Version** | v4.14.5 |

---

### 🔧 Setup Steps

#### Step 1 — Install VirtualBox & Create Virtual Machines
Two VMs were created with a Host-Only Network Adapter:
- **VM1 (Ubuntu 22.04):** 4 GB RAM · 2 vCPU · 50 GB storage — hosts the Wazuh SIEM stack
- **VM2 (Windows 10):** 4 GB RAM · 2 vCPU · 60 GB storage — monitored endpoint

#### Step 2 — Deploy Wazuh Stack on Ubuntu VM

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash ./wazuh-install.sh -a

# Verify server IP
ip a
# Result: 192.168.134.133/24 on eth0
```

**Screenshot 1 — Server IP confirmed:**

![Server IP](screenshots/01_server_ip.png)

#### Step 3 — Validate Services & Dashboard

```bash
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-dashboard

sudo systemctl status wazuh-dashboard   # Active: running
sudo systemctl status wazuh-indexer     # Active: running
```

**Screenshot 2 — Services running:**

![Services Status](screenshots/04_services_status.png)

**Screenshot 3 — API Connection Online:**

![API Online](screenshots/05_api_online.png)

#### Step 4 — Enroll Wazuh Agent on Windows 10 VM

Accessed **Wazuh Dashboard → Agents Management → Deploy new agent**, selected Windows, set Manager IP to `192.168.134.133`, then ran on the Windows 10 VM:

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.5-1.msi `
  -OutFile $env:tmp\wazuh-agent;
msiexec.exe /i $env:tmp\wazuh-agent /q WAZUH_MANAGER='192.168.134.133'

NET START WazuhSvc
```

**Screenshot 4 — Agent deployment wizard:**

![Agent Deploy](screenshots/06_agent_deploy.png)

---

### ✅ Week 1 Deliverable — 1 Agent Active

**Screenshot 5 — FINAL RESULT: Wazuh Dashboard showing 1 Active Agent:**

![1 Agent Active Dashboard](screenshots/03_agent_active_dashboard.png)

| Field | Value |
|-------|-------|
| **Agent ID** | 001 |
| **Agent Name** | DESKTOP-HKEMGDF |
| **IP Address** | 192.168.134.1 |
| **OS** | Microsoft Windows 10 Home Single Language 10.0.28200.8457 |
| **Group** | default |
| **Cluster Node** | node01 |
| **Version** | v4.14.5 |
| **Status** | ✅ **Active** |

---

### 🧠 Key Learnings — Week 1

- **Wazuh Architecture:** Manager (rule engine), Indexer (OpenSearch data store), and Dashboard (Kibana UI) as three independent services.
- **Service Troubleshooting:** Reading `systemctl status` to diagnose startup issues and restarting in order: Indexer → Manager → Dashboard.
- **VirtualBox Networking:** Host-Only adapter creates a private subnet for VM-to-VM communication without external exposure.
- **Agent Enrollment Flow:** Dashboard generates deploy command → MSI installed on Windows → agent registers to Manager on port 1514 → Status: Active.
- **Real-World Simulation:** Two-VM setup mirrors actual enterprise SOC deployments.

---

### 📸 Screenshots Folder Structure

```
screenshots/
├── 01_server_ip.png            ← Ubuntu VM: ip a showing 192.168.134.133
├── 04_services_status.png      ← systemctl: wazuh-dashboard & wazuh-indexer active
├── 05_api_online.png           ← API Connections: Online, v4.14.5
├── 06_agent_deploy.png         ← Agent deployment wizard in dashboard
└── 03_agent_active_dashboard.png ← FINAL: 1 Agent Active ✅
```

---

### 🔗 References

- [Wazuh Official Documentation](https://documentation.wazuh.com)
- [Wazuh Installation Assistant](https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/installation-assistant.html)
- [Wazuh Agent Enrollment](https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/index.html)

---

> **Intern:** Israel Mbiyavanga David  
> **Program:** CFSS Global Internship 2026  
> **LinkedIn:** Tag **CFSS Agency** with `#CFSSIntern2026`
