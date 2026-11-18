# Enterprise Group Policy Management & Security Baselines

## 🎯 Objective
Design a structured enterprise Organizational Unit (OU) hierarchy and deploy granular Group Policy Objects (GPOs) to enforce security baselines, automate resource provisioning, and manage user/device behavior across the domain.

---

## 🛠️ Environment & Architecture
- **Domain Controller:** Windows Server 2022 (`ApexTech.com` at Static IP `192.168.122.22`)
- **Management Console:** Group Policy Management Console (`gpmc.msc`)
- **Client Target:** Windows 11 Pro (`WIN11-Client01` joined to the domain and placed in the Workstations OU)

---

## 🏗️ 1. Organizational Unit (OU) Architecture
To adhere to enterprise best practices and avoid the limitations of default Active Directory containers, a clean hierarchical OU structure was established in **Active Directory Users and Computers (`dsa.msc`)**:

- **Root OU:** `DFW`
  - 📂 `Workstations` *(Houses enterprise client machines for device-level computer GPOs)*
  - 📂 `Servers` *(Houses domain member servers, isolated from client policies)*
  - 📂 `Users` *(Houses domain user accounts for identity-targeted user GPOs)*

---

## 📋 2. Implemented Group Policy Objects (GPOs)

### GPO 1: Workstation Screen Timeout / Inactivity Lock (`SEC-Workstation-ScreenLock`)
- **Target Scope:** Linked to the `Workstations` OU (Computer Configuration).
- **Objective:** Enforce physical security by ensuring unattended endpoints lock automatically after a period of inactivity.
- **Configuration:** Configured interactive logon user inactivity limits via GPO security settings to enforce automated screen locking.

### GPO 2: Automated Shared Drive Mapping (`USER-Map-SharedDrive`)
- **Target Scope:** Linked to the `Users` OU (User Configuration > Preferences > Windows Settings > Drive Maps).
- **Objective:** Eliminate manual drive mapping by dynamically provisioning network file shares upon user login.
- **Configuration:** Set action to *Update*, specified the file server share path, assigned a persistent drive letter, and targeted user group membership.

### GPO 3: Account Lockout & Password Complexity 
- **Target Scope:** Domain-wide / Default Domain Policy.
- **Objective:** Mitigate brute-force attacks and enforce robust credential standards.
- **Configuration:** Configured account lockout threshold (locking accounts after repeated failed attempts) and minimum password length/complexity requirements.

---

## ✅ 3. Verification & Testing

1. **Forcing Policy Updates on the Client:**
   - Logged into the domain-joined client VM (`WIN11-Client01`) using domain credentials.
   - Opened Command Prompt and executed the policy refresh command:
     ```cmd
     gpupdate /force
     ```
   - Confirmed successful policy download and computer/user setting application without errors.

2. **Validating Enforcement:**
   - Verified that screen timeout configurations and automated drive mappings applied correctly to the targeted user/workstation context.

---

## 📸 Lab Evidence
### Group Policy Management Console & Linked GPOs
![Group Policy Management](<GPOs.jfif>)

### Client Policy Refresh & Verification
![Group Policy Update](<gpupdate.png>)

### Domain Password Policy Configuration
![Password Policy](<Password policy.png>)

### Account Lockout Policy Configuration
![Account Lockout Policy](<Account lockout.png>)
