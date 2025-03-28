# Understading terms used in Access Control Models

## Comparing Permissions, Rights, and Privileges
- Permissions: the access granted for an object and determine what you can do with it
- Rights: ability to take an action on an object
- Privileges: combination of rights and permissions

## Understanding Authorization Mechanism
- Implicit Deny: access to an object is denied unless access has been explicitly granted to a subject.
- Access Control Matrix: a table that includes subjects, objects, and assigned privileges. An access control matrix can include a group of files as the objects and a group of users as the subjects. This covers much more than a single access control list (ACL). Each file listed within the matrix has a separate ACL that lists the authorized users and their assigned permissions.
- Capability Tables: different from ACLs in that a capability table is focused on subjects (like users, groups, or roles). It helps identify privileges assigned to subjects.
- Constrained Interface (or restricted interface): Applications use constrained interfaces to restrict what users can do or see based on their privileges. The Clark-Wilson model discusses the technical details of how it implements a constrained interface.
- Content-Dependent Control: restrict access to data based on the content within an object. A database view (like in SQL query) is a content-dependent control.
vi.Context-Dependent Control: require specific activity before granting users access (like when you purchasing on the website that leads to different distinct pages). It’s also possible to use date and time controls as context-dependent controls (like denying access when the user access outside the allowed time).
- Need to Know: subjects are granted minimum accesses
- Least Privilege: subjects are granted minimum privileges.
- Separation of Duties and Responsibilities: helps prevent fraud and errors by creating a system of checks and balances.

## Defining Requirements with a Security Policy:
Security policy defines security requirements for an organization. Some organizations create multiple security policies focusing on a separate area. It may state the need to implement and enforce separation of duties and least privilege principles but not state how to do so.

# Comparing Access Control Models

## 1. Discretionary Access Control (DAC)
- Every object has an owner (**data custodian**) and the owner can grant or deny access to any other subjects.
- **Example:** The New Technology File System (NTFS), used on Microsoft Windows operating systems, uses the DAC model. Data owners can also delegate day-to-day tasks for handling data to data custodians, giving data custodians the ability to modify permissions.
- A DAC model is implemeneted using ACLs on objects. Each ACL defines the types of access granted or denied to subjects. It doesn’t offer a centrally controlled management system because owners can alter the ACLs on their objects at will.

## 2. Mandatory Access Control (MAC)
- The use of labels is applied to both subjects and objects. When documented in a table, the MAC model sometimes resembles a lattice (such as the one used for a climbing rosebush), so it is referred to as a lattice-based model. For the example below, within the Confidential section (Lentil, Foil, Crimson, and Matterhorn), users also require the additional label to access data within these compartments. To access Lentil data, users need to have both the Confidential label and the Lentil label. Notice that Sensitive data doesn’t have any additional labels. Users with the Sensitive label can be granted access to any data with the Sensitive label.  
  ![image](https://github.com/user-attachments/assets/7bc8959d-c625-4159-b0e4-1b9ee2d69b1a)
- **Example:** If a user has a label of top secret, the user can be granted access to a top-secret document (matching labels).
- Using compartmentalization with the MAC model enforces the *need to know* principle. The MAC model is more security than the DAC model, but it isn’t as flexible or scalable. Classifications within a MAC model use one of the following three types of environment:
  - Hierarchical Environment: relates various classification labels in an ordered structure from low security to medium security to high security (like Confidential, Secret, and Top Secret). Clearance in one level grants the subject access to objects in that level as well as to all objects in lower levels.
  - Compartmentalized Environment: there’s no relationship between one security domain and another. To gain access to an object, the subject must have specific clearance for the object’s security domain.
  - Hybrid Environment: Combining both hierarchical and compartmentalized concepts so that each hierarchical level may contain numberous subdivisions that are isolated from the rest of the security domain. It provides granular control over access but becomes increasingly difficult to manage as it grows.

## 3. Role-Based Access Control (RBAC)
- Instead of assigning prmissions directly to users, user account are placed in roles and administrators assign privileges to the roles. These roles are typically identified by job functions.
- **Example:** Microsoft Windows operating systems implement this model with the use of groups.
- Administrators often implement RBAC using groups.
- Another **example**: A bank may have loan officers, tellers, and managers. Administrators can create a group named Loan Officers, place the user accounts of teach loan officer into this group, and then assign appropriate privileges to the group. If the organization hires a new loan officer, administrators simply add the new loan officer’s account into the Loan Officers group, and the new employee automatically has all the same permissions as other loan officers in this group. Administrators would take similar steps for tellers and managers.
  ![{D490EC1E-FC1D-461B-B304-5526AC26B3D3}](https://github.com/user-attachments/assets/c58f951d-da4f-4d75-917d-ddda1d4f038f)

## 4. Attribute-Based Access Control (ABAC)
- Its use of rules that can include multiple attributes, allowing it to be much more flexible than a rule-based access control model that applies the rules to all subjects equally. It’s an advanced implementation of a rule-based access control.
- **Example:** Many SDNs use the ABAC model. Additionally, ABAC allows administrators to create rules within a policy using plain language statements such as “Allow Managers to access the WAN using a mobile device.” MDM systems can use attributes to identify mobile devices, allowing the user to log on when the attributes match and verified.

## 5. Rule-Based Access Control
- It applies global rules to all subjects. Rules within the rule-based access control model are sometimes referred to as restrictions or filters.
- **Example:**  A firewall uses rules that allow or block traffic to all users equally

## 6. Risk-Based Access Control (RBAC, but different from Role-Based)
- Grants access after evaluating risk. It evaluates the environment and the situation and makes risk-based decisions using policies embedded within software code. It uses machine learning to make predictive conclusions about current activity based on past activity. Or, it can sometimes use binary rules to control access.
- **Example:**
  - If a user logs in from an unusual location, require extra authentication.
  - If a device is outdated or unpatched, limit its access.
- Used in zero-trust security and fraud prevention.
- Consider this scenario: an information system containing patient information and used by medical professionals. Doctors, nurses, and others working in the emergency room (ER) of a hospital need access to this data for any patient who shows up in the ER. The model attempts to evaluate risk by considering several different elements, such as:
  - The environment: is the ER in this example. Within cybersecurity, the environment can include items such as the location using the IP address. Some low-risk IP addresses may internal IP addresses and internet-based IP addresses of users who have previously signed in. High-risk IP addresses could be from foreign coutnries, anonymized IP addresses, users signed in from two or more IPs in different countries, and users signed in from unfamiliar locations.
  - The situation: is the medical emergency in this example. Moreover, the situation may include what a device is doing. Most IoT devices have predictable behavior. If an IoT device suddenly starts flooding a network with malicious traffic, the risk-based model could determine the device is now a high risk and block its access to a network.
  - Security policies: software code that makes risk-based decisions based on available data. An organization would modify the choices within the software to support their needs. In this example, it consider this a low risk and grant full access to patient data to doctors and nurses.
  - Two other things can be checked or required before the policy grants access:
    - Multifactor Authentication
    - Compliant Mobile Devices: the policy may require that smartphones and tablets meet specific security requirements.

## 7. Identity-Based Access Control (IBAC)
- A subset of Discretionary Access Control. Systems identify users based on their identity and assign resource ownership to identities
- **Example:** A CEO might have unique permissions that no other employees have.

## 8. Policy-Based Access Control (PBAC)
- Uses a set of policies to define who can do what.
- **Example:** Cloud providers like AWS use PBAC to let organizations define custom rules for access.

## 9. Non-discretionary Access Control
- Administrators centrally administer nondiscretionary access controls and can make changes that affect the entire environment. Access doesn’t focus on user identity. Instead, a static set of rules governing the whole environment manage access. Non-DAC systems are centrally controlled and easier to manage (although less flexible). In general, any model that isn’t a discretionary model is a nondiscretionary model.

## 10. Task-based Access Control (TBAC)
- Another method related to RBAC is task-based access control. Instead of being assigned to one or more roles, each user is assigned an array of tasks. The focus is on controlling access by assigned tasks rather than by user identity.
- **Example:** Microsoft Project uses TBAC. Each project has multiple tasks. The project manager assigns tasks to project team personnel. Team personnel can address their own tasks (adding comments, indicating progress, and so on), but they cannot address other tasks. Microsoft Project handles the underlying details.

# Access Control Lists (ACL)

## 1. ACL in Networking (Router, Firewall)
- Used to control network traffic by defining rules for which packets can pass or be blocked.
- Commonly found in routers, firewalls, and switches.
- Rules are based on IP addresses, ports, and protocols.

### ✅ Example (Cisco Router ACL):
```plaintext
access-list 101 permit tcp any host 192.168.1.10 eq 22
access-list 101 deny ip any any
```
🔹 This allows SSH (port 22) traffic to 192.168.1.10 but blocks everything else.

## 2. ACL in File Systems / OS Security
- Used to restrict access to files and directories.
- Defines which users or groups can read, write, or execute files.
- Found in Windows NTFS, Linux, and Unix systems.

### ✅ Example (Linux ACL using `setfacl`):
```bash
setfacl -m u:john:r-- file.txt
```
🔹 This grants read-only access to John for `file.txt`.

### ✅ Example (Windows NTFS ACL using PowerShell):
```powershell
icacls C:\example.txt /grant John:(R)
```
🔹 This grants read-only access to John for `example.txt`.

## Key Differences

| Feature            | Network ACL (Router/Firewall) | File System ACL (OS Security) |
|--------------------|-----------------------------|------------------------------|
| **Purpose**       | Controls network traffic     | Controls file access        |
| **Applies To**    | IP addresses, ports, protocols | Users, groups, and file permissions |
| **Common Use**    | Firewalls, routers, switches | Linux, Windows, Unix        |
| **Example**       | `access-list 101 deny ip any any` | `icacls C:\file /grant John:(R)` |
