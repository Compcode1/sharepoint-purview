# Microsoft Purview File-Level Encryption for SharePoint Online

## Description
Deploys file-level cryptographic protection for PII within SharePoint Online using Microsoft Purview. It ensures sensitive files remain encrypted and accessible only to authorized users, shifting security from the site container boundary directly into the file metadata itself.(Sensitivity Labels)

---

## Core Configurations

1. **Tenant Baseline & Processing Integration**
   * Enabled Microsoft Purview's ability to process content in Office Online encrypted files. This baseline switch allows SharePoint Online and OneDrive to open, index, and co-author protected documents directly within a web browser, eliminating rendering failures for users.

2. **Sensitivity Label Architecture (`Restricted-PII`)**
   * **Scope:** Configured strictly at the item level (files and emails) rather than the site container level, ensuring data protection moves with the file regardless of its hosting location.
   * **Cryptographic Access Control:** Enabled encryption using the Azure Rights Management engine. Identities (such as User-Alpha) are explicitly mapped to the file's metadata access control list (ACL) with **Viewer** permissions, restricting them to read-only capabilities while preventing modification, printing, or copying.
   * **Identity Constraint Handling:** Addressed the technical constraint where standard, non-mail-enabled Entra ID security groups are filtered out by the Purview encryption wizard, resolving it through direct user-to-metadata identity mapping.

3. **Label Policy Deployment (`Restricted-PII-Publish-Policy`)**
   * Deployed a dedicated Sensitivity Label Policy scoped to the full directory. This publishes and advertises the label to client applications, making it instantly selectable for data owners inside SharePoint document libraries.

---

## System Mechanics: Container vs. Data Security

This project implements a critical shift from traditional perimeter-based security to data-centric security. 

* **The Limitation of Container Boundaries:** Standard network controls—such as traditional Conditional Access or basic SharePoint site permissions—operate on an "all-or-nothing" entry gate. Once a user crosses the site perimeter, those network gates drop. If an insider or external actor accidentally or maliciously downloads a file, perimeter controls can no longer protect that data.
* **The Power of File-Level Cryptography:** By embedding the cryptographic policy directly into the file's XML structure, the data becomes self-protecting. If a file tagged with `Restricted-PII` is downloaded, sent via email, or moved to a personal thumb drive, it remains completely encrypted. Every time a user attempts to open it, the client application must authenticate against Microsoft Purview to verify if that specific user has the decryption key.

---

## Global Importance & Enterprise Enterprise Use Cases

In modern security operations, file-layer protection is a core requirement for achieving a comprehensive Zero Trust architecture. It is widely implemented across enterprise environments in the following scenarios:

* **Insider Threat Mitigation:** Prevents over-privileged internal users (such as site owners, global administrators, or IT personnel) from viewing highly confidential data. An IT administrator can manage the SharePoint infrastructure, but cannot read the contents of the files unless explicitly granted decryption rights.
* **Strict Regulatory Compliance (GDPR, HIPAA, CCPA):** Simplifies compliance audits by ensuring that Personally Identifiable Information (PII) and protected health information remain unreadable to unauthorized entities, effectively mitigating liability in the event of a broad data breach.
* **Secure Cross-Functional Collaboration:** Allows organizations to utilize unified, open SharePoint sites for standard team collaboration while seamlessly isolating high-value corporate assets—such as payroll spreadsheets, executive performance reviews, or intellectual property—down to specific individuals within that same shared space.
