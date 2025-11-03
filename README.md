# 🛡️ STIGs & POA&M Simulation (DoD/RMF)

> **Summary:** On **2025-10-21**, I attended a session on creating **POA&Ms** and working with **DISA STIGs**. I then continuously after ran my own simulations: downloading STIGs from the DoD Cyber Exchange, reviewing findings (prioritizing **CAT I**), and drafting **OSCAL-aware POA&M entries** the way an ISSO would for A&A/ATO workflows.

---

## 🎯 Objectives
- Understand **what STIGs are** and why they matter for DoD/RMF.
- Practice **triaging findings** (CAT I/II/III) and drafting **POA&M** items.
- Align artifacts with **A&A (Assessment & Authorization)** expectations.
- Demonstrate the **ISSO role**: document risks, owners, milestones, and evidence.

---

## 🧠 Key Concepts (from my notes)
- **STIGs (Security Technical Implementation Guides):** hardening baselines published by DISA; cover OS, apps, network devices, cloud/containers; distributed as **XML/XCCDF, checklists, and PDFs**.  
- **Why STIGs matter:** standardize security, reduce attack surface, align with **NIST 800-53**, required for **ATO**, save time during audits.  
- **Best practices:** stay on latest STIG, apply early in system build, **automate** (SCC/SCAP/Ansible/PowerShell), track exceptions with **POA&M** or **Risk Acceptance**, document clearly.  
- **ISSO role in testing:** coordinate, record evidence (eMASS style), create **POA&Ms** for non-compliance, communicate to SCA, advise owners on priorities.

---

## 🛠️ Environment & Sources
- **Source:** DoD Cyber Exchange → **STIG Document Library** (downloaded STIGs).
- **Standards context:** **NIST 800-53**, **OSCAL POA&M** data model (fields & structure).
- **Artifacts I handled:** STIG checklists (XML/CSV/PDF), simulated system inventory, POA&M worksheet.

---

## 🚀 Workflow

### 1) Acquire & Scope
- Pulled latest STIG package for target technology from DoD Cyber Exchange.
- Noted **release date**, **benchmark version**, and **applicability**.
- Defined simulation scope (system components, owners, environment).

### 2) Triage Findings (focus CAT I first)
- Parsed STIG rules → grouped by **Severity/CAT** (CAT I/II/III).
- Flagged **CAT I** that impact confidentiality/integrity/availability.
- Mapped each rule to the **affected component** and **control family** (when known).

### 3) Draft POA&M Entries (ISSO view)
For each non-compliant item I captured:
- **Rule/Check ID** (from STIG), **Title**, **Severity (CAT)**  
- **Finding** (what is non-compliant)  
- **Recommendation/Remediation** (from STIG guidance)  
- **Responsible Owner** (system owner/tech lead/ISSO)  
- **Milestones** (steps to fix) & **Due Date**  
- **Status** (Open/In Progress/Mitigated/Accepted)  
- **Evidence/Links** (screenshots, tickets, config snippets)  
- **OSCAL fields** (kept lightweight so this can map to an OSCAL POA&M later)

### 4) Example POA&M Table 

| 🧾 **POA&M ID** | ⚙️ **STIG Rule ID** | 🧩 **Control / Title**                           | 🚨 **Severity (CAT)** | 💡 **Finding / Risk Description**                                                                        | 🛠️ **Recommended Remediation**                                                                                      | 👤 **Responsible Owner** | 🗓️ **Planned Milestone / Due Date**                           | 📈 **Status**  | 🧬 **OSCAL / Control Mapping** | 📁 **Evidence Reference**                                          |
| :-------------: | :------------------ | :----------------------------------------------- | :-------------------: | :------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------- | :----------------------- | :------------------------------------------------------------- | :------------- | :----------------------------- | :----------------------------------------------------------------- |
|  **POAM-0001**  | RHEL-07-010010      | *Ensure password complexity is enforced*         |    **CAT I (High)**   | System allows weak passwords; PAM configuration does not enforce complexity.                             | Update `/etc/security/pwquality.conf` and enforce policy through PAM; verify via STIG Viewer.                        | ISSO / Linux Admin       | Milestone – Apply PAM update<br>Due Date – **2025-11-15**      | 🟡 In Progress | IA-5 (1), AC-2                 | Change Ticket CHG-10234 • Screenshot `/evidence/pam_pwquality.png` |
|  **POAM-0002**  | APP-00-000100       | *Disable directory listing on web server*        |  **CAT II (Medium)**  | Apache configuration allows directory browsing under `/var/www/html`, revealing internal file structure. | Modify virtual host configuration (`Options -Indexes`) and redeploy hardened build; add config check to CI pipeline. | Web Platform Owner       | Milestone – Apply hardened config<br>Due Date – **2025-11-30** | 🔵 Open        | SC-7, CM-6                     | OpenVAS report #22 • Screenshot `/evidence/dirlist-off.png`        |
|  **POAM-0003**  | WIN-10-000015       | *Ensure screen lock is enabled after 15 minutes* |  **CAT II (Medium)**  | Default Group Policy does not enforce inactivity lockout; risk of unauthorized access.                   | Enforce screen lock timer via Group Policy `Computer Config → Security Options → Interactive logon`.                 | System Administrator     | Milestone – Update GPO<br>Due Date – **2025-12-05**            | 🟢 Planned     | AC-11 (a), AC-2                | GPO screenshot • Audit log record `/evidence/gpo-lock.png`         |
|  **POAM-0004**  | NET-FW-001200       | *Restrict inbound administrative traffic*        |    **CAT I (High)**   | Firewall ACL permits inbound management traffic from any IP; potential remote exploit vector.            | Implement ACL to restrict to management subnet; validate rule via compliance scan.                                   | Network Engineer         | Milestone – Deploy new ACL<br>Due Date – **2025-11-25**        | 🟠 In Progress | SC-7 (3), CM-6                 | FortiGate config export • Scan screenshot `/evidence/fw-acl.png`   |


### 6) Evidence & Documentation
Saved screenshots/exports (STIG viewer, configs, scan diffs) to /evidence/.
Wrote a brief rationale for any risk acceptance (RA) and linked to approvals.
Kept a change log with dates/owners for audit traceability.

--

## ✅ Outcome
- Produced clear, auditable POA&M entries aligned to STIG findings and RMF language.
- Practiced the ISSO workflow end-to-end: identify → document → assign → plan → evidence.
- Ensured the structure can map to OSCAL POA&M if needed later.

--

## 🧩 Skills Demonstrated
- DISA STIG interpretation & severity triage (CAT I/II/III)
- POA&M creation/tracking with milestones, owners, dates, evidence
- RMF/A&A understanding (ATO expectations, documentation discipline)
- Cross-walk awareness to NIST 800-53 controls & OSCAL artifacts

--

## 🗒️ Portfolio Note
I simulated a DoD-style hardening and remediation workflow. I downloaded STIGs, triaged CAT I first, and authored OSCAL-aware POA&M entries with owners, milestones, and evidence — practicing the ISSO role within an A&A/ATO context.
