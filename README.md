<p align="center">
  <img width="900" height="500" alt="image" src="https://github.com/user-attachments/assets/2c068a6c-d82b-4a43-ad32-d9fe559e1837" />
</p>

# okta-AD-integration





### 1) Install Active Directory Agent

1. In **Okta** open the **Directory** tab then go to **Directory Integrations**
2. Click **Add Directory** then **Add Active Directory**
3. Select **Set Up Active Directory**
4. Select **Download Agent** then copy the **Link Address**

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

10. In the **COnnecte an Organizational Unit to Okta** page make sure only **_users** and **_groups** are selected then press **Next**

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

5. Once complete the agents will appear
6. Check the box next to all three agents then **Confirm Assignments**

<p align="center">
  <img width="978" height="694" alt="image" src="https://github.com/user-attachments/assets/c8084b53-5d9e-4d7a-bcb5-522c0a1d0b21" />
</p>

7. Check **Auto-Activate Users After COnfirmation** then **Confirm**

<p align="center">
  <img width="587" height="379" alt="image" src="https://github.com/user-attachments/assets/d04bca68-e42b-4475-9290-88599b5d7371" />
</p>

---

### 3) Confirm Import was Successful

1. Open the **Directory** tab then go to **People**
2. Click **John Smith**
3. It should say **Profile Sourced by Active Directory**

<p align="center">
  <img width="737" height="430" alt="capture1 drawio" src="https://github.com/user-attachments/assets/872838b3-9f1a-4a75-ad80-905c20f6fba2" />
</p>

---

### 4) Incremental Import

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

### 5) Enable Delegated Authentication

1. Go to the **Directory** tab then **Directory Integrations**
2. Choose your **Directory**
3. Open the **Provisioning** tab then click **Integration**
4. Find the **Delegated Authentication** section then toggle **Enable Delegated Authentication to Active Directory** to on then **Save**

<p align="center">
  <img width="741" height="243" alt="image" src="https://github.com/user-attachments/assets/fe458857-9a37-4ed7-920a-617231ad189c" />
</p>

5. Click **Test Delegatged Authentication**
6. Enter the credentials for **John Smith** then select **Authenticate**

<p align="center">
  <img width="441" height="352" alt="image" src="https://github.com/user-attachments/assets/9cd212c7-3f8c-40b6-bc04-96ca2969226c" />
</p>

7. Once complete it will say **Active Directory Authentication Successful**

<p align="center">
  <img width="439" height="184" alt="image" src="https://github.com/user-attachments/assets/f8526c2c-7a31-431e-93b9-98790c7ec85b" />
</p>

---

### Just-In-Time (JIT) Provisioning

