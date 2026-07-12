<p align="center">
  <img width="900" height="500" alt="image" src="https://github.com/user-attachments/assets/2c068a6c-d82b-4a43-ad32-d9fe559e1837" />
</p>

# Okta Active Directory Integration

This project demonstrates the integration of an on-premises Active Directory environment with Okta as the cloud Identity Provider, simulating the directory synchronization responsibilities of an IAM Analyst or Identity Engineer in a hybrid identity environment. Building on the Okta tenant configured in the previous lab, this lab walks through installing and configuring the Okta AD Agent on a Windows Server domain controller, running Full and Incremental imports to sync user identities and security groups from Active Directory into Okta, enabling Delegated Authentication so users authenticate with their Active Directory credentials, configuring Just-In-Time provisioning for automatic account creation on first login, validating user profile attribute mapping between Active Directory and Okta, and verifying group sync from Active Directory. All configurations are hands-on in a live Okta trial org connected to a Windows Server 2022 domain controller hosted in Microsoft Azure and reflect real-world IAM practices around hybrid identity management and directory integration.

---

## Prerequisites

This is the second lab of the [Okta IAM Lab Series](https://github.com/RyanKennon/Okta-Lab-Series/tree/main).
Complete all previous labs before starting this one.

The following should be in place before starting:

- Active Okta trial org with users, groups, and policies configured from Lab 1
- Windows Server 2022 VM running in Microsoft Azure
- Active Directory domain configured with the following:
  - Organizational Units created (`_Users` and `_Groups`)
  - At least three AD users created with department attributes populated
  - At least four AD security groups created with users assigned

---

## Environments and Technologies Used

- Okta Identity Cloud (Trial Org)
- Okta Admin Console
- Okta Universal Directory
- Okta AD Agent
- Microsoft Azure
- Windows Server 2022
- Active Directory Domain Services (AD DS)
- Active Directory Users and Computers (ADUC)
- Remote Desktop Protocol (RDP)

---

## Table of Contents

- [1) Install Active Directory Agent](#1-install-active-directory-agent)
- [2) Import from Active Directory](#2-import-from-active-directory)
- [3) Run an Incremental Import](#3-run-an-incremental-import)
- [4) Enable Delegated Authentication](#4-enable-delegated-authentication)
- [5) Enable Just-In-Time (JIT) Provisioning](#5-enable-just-in-time-jit-provisioning)
- [6) User Profile Mapping in Active Directory](#6-user-profile-mapping-in-active-directory)
- [7) Verify Group Sync from Active Directory](#7-verify-group-sync-from-active-directory)

---

### 1) Install Active Directory Agent

The Okta Active Directory Agent is a lightweight service installed on the 
domain controller that acts as the bridge between on-premises Active Directory 
and the Okta Identity Cloud. Installing and configuring the agent establishes 
the connection that enables user and group sync, delegated authentication, and 
Just-In-Time provisioning.

1. In **Okta** open the **Directory** tab then go to **Directory Integrations**
2. Click **Add Directory** then **Add Active Directory**
3. Select **Set Up Active Directory**
4. Select **Download Agent** then **Copy Link Address**

<p align="center">
  <img width="892" height="213" alt="image" src="https://github.com/user-attachments/assets/c6c21c3b-9ad5-4997-9031-fcfa24ee147c" />
</p>

5. Inside of the **Domain Controller** open a **browser** and paste the **Link Address**
6. Run the installation wizard and follow the steps
7. When prompted to sign in to **Okta** enter the **Okta Org Credentials**

<p align="center">
  <img width="397" height="329" alt="Device Activated-Capture1 drawio" src="https://github.com/user-attachments/assets/2c525fbf-8d3f-4313-bf47-ad920d9f76ed" />
</p>

8. In **Okta** open the **Directory** tab then select **Directory Integrations**
9. Select the **Connected Directory**

<p align="center">
  <img width="469" height="312" alt="image" src="https://github.com/user-attachments/assets/19e7a27d-2232-4504-b522-e87e1b69a1dc" />
</p>

10. In the **Connect an Organizational Unit to Okta** page make sure only **_users** and **_groups** are selected then press **Next**

<p align="center">
  <img width="747" height="767" alt="image" src="https://github.com/user-attachments/assets/d2d7274a-c082-474b-bb10-ba3d29edef73" />
</p>

11. On the **Select Attributes to Build your Okta User Profile** page click **Next**
12. Then on the **Agent Setup Complete** screen select **Done**

<p align="center">
  <img width="1039" height="335" alt="image" src="https://github.com/user-attachments/assets/5abfc4bf-039b-49ea-8e22-f31bd4a46d3a" />
</p>

---

### 2) Import from Active Directory

A Full Import pulls all users and groups from Active Directory into Okta based 
on the configured OU scope. Confirming the import assignments links the AD 
accounts to their corresponding Okta user profiles, making Active Directory 
the authoritative source for those identities.

1. In **Okta** open the **Directory** tab then select **Directory Integrations**
2. Choose your **Directory**
3. Open the **Import** tab then **Import Now**

<p align="center">
  <img width="1024" height="544" alt="image" src="https://github.com/user-attachments/assets/29bf6950-e7ce-4582-b588-4408f8be9f5c" />
</p>

4. Select **Full Import** then **Import**

<p align="center">
  <img width="625" height="600" alt="image" src="https://github.com/user-attachments/assets/20a7e196-9d6d-4af8-a307-0d95d82181ac" />
</p>

5. Once complete the users will appear
6. Check the box next to all three agents then **Confirm Assignments**

<p align="center">
  <img width="978" height="694" alt="image" src="https://github.com/user-attachments/assets/c8084b53-5d9e-4d7a-bcb5-522c0a1d0b21" />
</p>

7. Check **Auto-Activate Users After Confirmation** then **Confirm**

<p align="center">
  <img width="587" height="379" alt="image" src="https://github.com/user-attachments/assets/d04bca68-e42b-4475-9290-88599b5d7371" />
</p>

8. Open the **Directory** tab then go to **People**
9. Click **John Smith**
10. Confirm the profile says **Profile Sourced by Active Directory**

<p align="center">
  <img width="737" height="430" alt="capture1 drawio" src="https://github.com/user-attachments/assets/872838b3-9f1a-4a75-ad80-905c20f6fba2" />
</p>

---

### 3) Run an Incremental Import

An Incremental Import picks up only the changes made in Active Directory since 
the last import rather than re-importing everything from scratch. Running an 
Incremental Import with no AD changes confirms the import mechanism is working 
correctly and demonstrates the difference between Full and Incremental imports.

1. Open **Directory** then **Directory Integrations**
2. Choose your **Directory**
3. Open the **Import** tab then **Import Now**
4. Select **Incremental Import** then **Import**

<p align="center">
  <img width="629" height="602" alt="image" src="https://github.com/user-attachments/assets/68c22287-a53a-43ff-bac3-7a6040f5696d" />
</p>

5. Since there were no changes made it will say **0 Users Scanned** and **0 Groups Scanned**

<p align="center">
  <img width="305" height="168" alt="image" src="https://github.com/user-attachments/assets/23285dea-7d48-45ca-bbf9-1cbdaa5192b6" />
</p>

---

### 4) Enable Delegated Authentication

Delegated Authentication allows Okta to pass password validation back to Active 
Directory, meaning users authenticate with their existing Windows credentials 
instead of maintaining a separate Okta password. This ensures a single 
credential experience across both on-premises and cloud resources.

1. Go to the **Directory** tab then **Directory Integrations**
2. Choose your **Directory**
3. Open the **Provisioning** tab then click **Integration**
4. Find the **Delegated Authentication** section then toggle **Enable Delegated Authentication to Active Directory** to on then **Save**

<p align="center">
  <img width="741" height="243" alt="image" src="https://github.com/user-attachments/assets/fe458857-9a37-4ed7-920a-617231ad189c" />
</p>

5. Click **Test Delegated Authentication**
6. Enter the credentials for **John Smith** then select **Authenticate**

<p align="center">
  <img width="441" height="352" alt="image" src="https://github.com/user-attachments/assets/9cd212c7-3f8c-40b6-bc04-96ca2969226c" />
</p>

7. Once complete it will say **Active Directory Authentication Successful**

<p align="center">
  <img width="439" height="184" alt="image" src="https://github.com/user-attachments/assets/f8526c2c-7a31-431e-93b9-98790c7ec85b" />
</p>

---

### 5) Enable Just-In-Time (JIT) Provisioning

Just-In-Time provisioning automatically creates an Okta user account the first 
time a user authenticates via delegated authentication, even if they have not 
been manually imported or synced yet. This eliminates the dependency on import 
schedules and ensures users can always access Okta as long as they exist in 
Active Directory.

1. Open the **Directory** tab then go to **Directory Integrations**
2. Choose your **Directory** then open the **Provisioning** tab then open **To Okta**
3. Under the **General** section select **Edit**
4. Check the box labeled **Create and Update Users on Login** next to **JIT Provisioning**

<p align="center">
  <img width="715" height="592" alt="image" src="https://github.com/user-attachments/assets/fcd7df0d-f354-47cb-928b-c146789d37f6" />
</p>

5. **Save**
6. In **Active Directory Users and Computers** create a new **User** in the **_Users** folder with the following information:
   - **First Name:** Sarah
   - **Last Name:** Connor
   - **User Logon Name:** sarah.connor
  
<p align="center">
  <img width="436" height="374" alt="image" src="https://github.com/user-attachments/assets/8a93c49f-5610-4e90-bce8-8273f6893bed" />
</p>

7. Open an **Incognito Browser** and sign in to your **Okta Org** as **Sarah Connor**
8. Go back to your **Okta Admin Account** then navigate to **Directory** then **People** and confirm that the profile for **Sarah Connor** has been created

<p align="center">
  <img width="1018" height="627" alt="image" src="https://github.com/user-attachments/assets/a659b554-2faa-41fd-a642-4e5cf5763fc1" />
</p>

---

### 6) User Profile Mapping in Active Directory

User profile mapping defines how Active Directory attributes flow into Okta 
user profile fields. Updating an attribute in Active Directory and running an 
Incremental Import validates that the mapping is working correctly and that 
profile changes in AD are automatically reflected in Okta.

1. In **Active Directory Users and Computers** find the **John Smith** profile and select **Properties**
2. Go to the **Organization** tab then for the **Job Title** enter **Financial Analyst** then press **Ok**

<p align="center">
  <img width="409" height="535" alt="image" src="https://github.com/user-attachments/assets/98db4208-3315-41a9-9947-37575fc23751" />
</p>

3. In **Okta** go to **Directory** then **Directory Integrations**
4. Choose your **Directory** then select the **Import** tab
5. Select **Import Now** then **Incremental Import** then select **Import**

<p align="center">
  <img width="301" height="285" alt="image" src="https://github.com/user-attachments/assets/a46abe63-07fb-41ce-bbb7-7f24490ac7b8" />
</p>

6. Go to the **Directory** tab then **People** then **John Smith** then the **Profile** tab
7. Confirm his **Title** shows as **Financial Analyst**

<p align="center">
  <img width="520" height="431" alt="image" src="https://github.com/user-attachments/assets/7508aff4-69e3-4a8e-bd31-2ff88c03094f" />
</p>

---

### 7) Verify Group Sync from Active Directory

Group sync ensures that Active Directory security groups and their memberships 
are reflected in Okta as directory-linked groups. Verifying group sync confirms 
that access assignments driven by AD group membership are correctly inherited 
in Okta, and demonstrates how to identify and resolve duplicate groups created 
from multiple sources.

1. Go to the **Directory** then **Groups**
2. Confirm the following groups appeared with an **AD** icon next to them
   - **Finance**
   - **IT**
   - **Human Resources**
   - **Kennon Technologies Employees**

<p align="center">
  <img width="830" height="253" alt="image" src="https://github.com/user-attachments/assets/110eafa9-6e09-4976-8f72-6fd602cccecd" />
</p>

3. Open the **AD Synced Finance Group** and verify **John Smith** appears as a member

<p align="center">
  <img width="1040" height="650" alt="image" src="https://github.com/user-attachments/assets/da518ec4-05df-4524-8fa7-e5c0e643cae6" />
</p>

4. Go **Back to Groups** and open the **Empty Native Finance Group (With the Okta Logo)** and open it up
5. Hit the **Actions** dropdown then press **Delete** then **Delete Group**

<p align="center">
  <img width="1015" height="529" alt="image" src="https://github.com/user-attachments/assets/6ecf9570-6252-4d6f-bf1d-4b74fc9de485" />
</p>

6. Delete the **Native IT, Human Resources, and Kennon Technologies Employees Groups**
7. Go back to **Active Directory Users and Computers** and open the **Properties** for the **Sarah Connor** profile
8. Open the **Member Of** tab then select **Add**
9. Add **Sarah Connor** to the **Finance** and **Kennon Technologies Employees** groups

<p align="center">
  <img width="408" height="535" alt="image" src="https://github.com/user-attachments/assets/a46cc65d-cf8a-41ec-a72f-9e501bbaf200" />
</p>

10. In **Okta** go to **Directory** then **Directory Integrations** then open your **Directory**
11. Open the **Import** tab then select **Import Now**
12. Select **Incremental Import** then run an **Import**

<p align="center">
  <img width="299" height="403" alt="image" src="https://github.com/user-attachments/assets/9c450eaf-eff8-46d0-a47d-4cc9b1386ddf" />
</p>

13. Now go to **Directory** then **Groups** then open the **Finance** group
14. Confirm that **Sarah Connor** appears in the **People** tab

<p align="center">
  <img width="1016" height="381" alt="image" src="https://github.com/user-attachments/assets/1f40da89-ad5e-4b24-88f1-93137a95e29e" />
</p>

---

> **Note:** This lab is intentionally left open. The Okta org configured 
> here serves as the foundation for all subsequent Okta labs in the 
> [Okta IAM Lab Series](https://github.com/RyanKennon/Okta-Lab-Series/tree/main).

---

<p align="left">
  <a href="https://github.com/RyanKennon/Okta-Tenant-Setup">⬅ Lab 1 — Okta Tenant Setup & Configuration</a>
</p>

<p align="right">
  <a href="https://github.com/RyanKennon/Okta-RBAC/tree/main">Lab 3 — RBAC Design & Implementation ➡</a>
</p>
