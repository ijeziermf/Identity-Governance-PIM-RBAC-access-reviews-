# Lab 3 — Identity Governance, Access Control, and Conditional Access

## Overview
This lab demonstrates how I designed and implemented identity governance controls in Microsoft Entra ID and Azure. The goal was to enforce least privilege, protect privileged roles, and validate identity-based access across multiple Azure resources.

This lab builds on my existing Azure environment and introduces:
- Role-Based Access Control (RBAC)
- Conditional Access
- Privileged Identity Management (PIM)
- Access Reviews
- Identity Protection
- Audit & Sign-in log analysis

---

# 1. Environment Architecture

### Identity Governance Architecture Diagram
<img width="1536" height="1024" alt="Identity Governance Architecture" src="https://github.com/user-attachments/assets/3f01bbfb-cdba-4b9f-8af3-3be732a23f78" />

---

# 2. Identity Objects

## 2.1 Create Least-Privilege User
Created a standard user with no directory roles.

**Screenshot:**  
<img width="482" height="181" alt="Screenshot 2026-02-27 at 11 31 44 PM" src="https://github.com/user-attachments/assets/58513aac-dd82-46b5-b677-ddba1856e584" />


---

## 2.2 Create Security Group
Created a security group to manage RBAC assignments at scale.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 1 22 22 AM" src="https://github.com/user-attachments/assets/bf2b3ce8-30e5-480a-bbc7-ca94c95910e7" />

---

# 3. RBAC Assignments

## 3.1 Resource Group-Level RBAC (Contributor)
Assigned **Contributor** to the operator group at the RG scope.

**Screenshot:** 
<img width="1404" height="1092" alt="Screenshot 2026-02-27 at 11 34 17 PM" src="https://github.com/user-attachments/assets/afc9f9f3-01c0-48f0-ba9b-dd84a06ea128" />

---

## 3.3 Key Vault RBAC (Secrets User)
Assigned **Key Vault Secrets User** to LabUser1.

**Screenshot:**  
<img width="1404" height="1092" alt="Screenshot 2026-02-27 at 11 35 51 PM" src="https://github.com/user-attachments/assets/9d404126-8038-4c0b-906b-0d61c6eee7f6" />

---

## 3.4 Storage Account RBAC (Blob Data Reader)
Assigned **Storage Blob Data Reader** to the operator group.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 1 50 15 AM" src="https://github.com/user-attachments/assets/feda492f-0c68-4a8d-8ad7-257762b14f9a" />

---

## 3.5 VM RBAC (Virtual Machine User Login)
Assigned **Virtual Machine User Login** to LabUser1.

**Screenshot:**  
<img width="1404" height="1090" alt="Screenshot 2026-02-27 at 11 44 07 PM" src="https://github.com/user-attachments/assets/af0b4d2e-374b-4ea7-9cd1-9f8731aec4e6" />

---

# 4. Conditional Access Policies

## 4.1 Require MFA for Admins
Created a Conditional Access policy enforcing MFA for Global Administrators.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 2 01 10 AM" src="https://github.com/user-attachments/assets/3dbcbc34-4536-458b-b92c-9bc8ea184bec" />

---

## 4.2 Block Non-Trusted Locations
Created a policy blocking sign-ins from outside trusted IP ranges.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 2 25 03 AM" src="https://github.com/user-attachments/assets/f51a399e-539f-4f05-8912-5e30e6a99bab" />

---

# 5. Privileged Identity Management (PIM)

## 5.1 Eligible Assignment for Global Administrator
Configured my account as **Eligible** for Global Administrator with:
- MFA required  
- Justification required  
- 1-hour activation window  

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 3 13 13 AM" src="https://github.com/user-attachments/assets/653f13ac-e1d8-4fdb-9df1-6d88b1f9e5d4" />

---

# 6. Access Reviews

Reviewed access for Global Administrator assignments.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 3 33 38 AM" src="https://github.com/user-attachments/assets/60dcc0de-6346-4238-85b0-da3eab347a16" />

---

# 7. Identity Protection Policies

## 7.1 User Risk Policy
Configured user risk policy to require password reset.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 3 55 22 AM" src="https://github.com/user-attachments/assets/1653a722-61ae-4193-a48d-6757b2a16517" />

---

## 7.2 Sign-In Risk Policy
Configured sign-in risk policy to require MFA.

**Screenshot:**  
<img width="1458" height="1081" alt="Screenshot 2026-03-03 at 3 56 52 AM" src="https://github.com/user-attachments/assets/9e8a8aee-f904-4310-b032-d314cee1e6be" />

---

# 8. Logging

## 8.1 Audit Logs
Captured logs for:
- Role assignments  
- Conditional Access creation  
- PIM activation  
- Access review creation
- Successful sign-ins  
- Blocked sign-ins  
- Risky sign-ins  

https://github.com/user-attachments/assets/deefcd32-c69c-43d6-9928-ffbbd1812451



---

# 9. Reflection & Challenges

This lab required deep troubleshooting due to identity and tenant complexities. Key challenges included:

### **1. Wrong Tenant Creation During Entra Signup**
Microsoft automatically created a new Entra tenant when I activated the Governance/P2 trial using my Gmail address.  
This caused:
- My Azure subscription not appearing  
- PIM not showing  
- Licensing mismatch  

**Workaround:**  
I identified the correct tenant tied to my Azure subscription and activated the Governance trial *inside that tenant* using the `.onmicrosoft.com` identity instead of my Gmail MSA.

---

### **2. Subscription Not Visible in Entra**
Azure subscriptions only attach to one tenant.  
I initially tried to activate Entra Governance from the wrong tenant, causing:
- Empty subscription list  
- Missing resources  
- Missing RBAC assignments  

**Workaround:**  
Switched directories → located the correct tenant → reactivated licensing.

---

### **3. PIM Not Appearing**
PIM only appears when:
- The tenant has Entra Governance/P2  
- The user has a directory role  
- The correct tenant is selected  

**Workaround:**  
Assigned myself a directory role → activated Governance trial → refreshed portal.

---

### **4. Multiple Tenants with Similar Emails**
Using a Gmail MSA caused Microsoft to create multiple identities with the same email but different object IDs.

**Workaround:**  
Mapped each identity to its tenant → deleted accidental tenants → continued lab in the correct one.

---

### **Outcome**
These challenges strengthened my understanding of:
- Tenant boundaries  
- Licensing behavior  
- Identity lifecycle  
- Azure/Entra architecture  
- Real-world troubleshooting  

This reflection demonstrates not only technical skill but also resilience and problem-solving — critical traits for cloud security and GRC roles.

---

# End of Lab 3
