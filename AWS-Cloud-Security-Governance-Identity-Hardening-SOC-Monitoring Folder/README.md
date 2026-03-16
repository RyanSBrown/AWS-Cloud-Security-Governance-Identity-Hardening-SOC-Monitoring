# AWS Security Engineering: IAM Hardening & SOC Monitoring

## 📌 Project Overview
This project demonstrates the implementation of robust **Identity and Access Management (IAM)** security controls within an AWS environment. The primary objective was to simulate a security operations team's workflow in hardening cloud identity infrastructure and establishing real-time monitoring to detect potential privilege abuse.

## 🛡️ Threat Scenario
* **Vector:** A malicious actor gains access to AWS credentials and attempts a Root account login.
* **Risk:** Root account access is extremely dangerous because it provides unrestricted administrative control over the entire AWS environment.
* **Objective:**
    1. Prevent root account misuse.
    2. Enforce identity verification.
    3. Detect privileged login attempts in real time.

---

## 🚀 Implementation Phases

### Phase 1: Identity Hardening & Governance
I established a secure baseline by enforcing strict account-wide policies and securing the most critical access points.

* **Custom Password Policy:** Configured a 12-character minimum length requirement.
* **Complexity Requirements:** Enforced uppercase, lowercase, numbers, and non-alphanumeric character requirements.
* **Credential Lifecycle:** Enabled 90-day password expiration and prevented the reuse of the last 12 passwords.
* **Root Protection:** Successfully assigned a **Virtual MFA device** to the Root account to mitigate credential theft risks.

### Phase 2: Role-Based Access Control (RBAC)
To prevent "permission creep," I implemented a scalable group-based model for access management.

* **Administrative Group:** Created the `Admins` IAM group and attached the `AdministratorAccess` policy.
* **Identity Segregation:** Created a dedicated `Cloud.Admin` IAM user for daily administrative tasks, ensuring the Root account is strictly reserved for emergency use.
* **Least Privilege:** By assigning permissions at the group level, I ensured that administrative access is governed and audited consistently.

### Phase 3: SOC Monitoring & Threat Detection
Utilizing my **CySA+** and **Security+** background, I engineered a detection pipeline to provide immediate visibility into high-risk authentication events.

* **CloudTrail Auditing:** Enabled account-wide logging to capture a forensic audit trail of all identity-related activity and API calls.
* **Detection Logic:** Developed a **CloudWatch Log Metric Filter** to isolate Root console logins.
    * **Filter Pattern:** `{ ($.eventName = "ConsoleLogin") && ($.userIdentity.type = "Root") }`
* **Automated Alerting:** Integrated **Amazon SNS** to trigger immediate email notifications when a Root login occurs.

---

## 📊 Security Operations Analysis
From a SOC analyst perspective, this monitoring pipeline enables the immediate detection of privileged identity misuse. If a root login occurs, the security team can quickly investigate the login source IP, verify whether the login is authorized, and initiate incident response protocols if necessary. This architecture significantly reduces the **Time-to-Detection (TTD)** for high-risk authentication events.

**Key Security Outcomes:**
* Implemented a **Least Privilege** identity model.
* Secured the **Root account with MFA** and hardened the identity lifecycle.
* Established **Group-based access control** to streamline governance and prevent permission creep.
* Built a functional, real-time monitoring pipeline integrating **CloudTrail, CloudWatch, and SNS**.
