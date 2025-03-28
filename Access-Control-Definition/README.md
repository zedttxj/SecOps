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
- The use of labels is applied to both subjects and objects. When documented in a table, the MAC model sometimes resembles a lattice (such as the one used for a climbing rosebush), so it is referred to as a lattice-based model.
- **Example:** If a user has a label of top secret, the user can be granted access to a top-secret document (matching labels).

## 3. Role-Based Access Control (RBAC)
- instead of assigning prmissions directly to users, user account are placed in roles and administrators assign privileges to the roles. These roles are typically identified by job functions.
- **Example:** Microsoft Windows operating systems implement this model with the use of groups.

## 4. Attribute-Based Access Control (ABAC)
- Its use of rules that can include multiple attributes, allowing it to be much more flexible than a rule-based access control model that applies the rules to all subjects equally.
- **Example:** Many SDNs use the ABAC model. Additionally, ABAC allows administrators to create rules within a policy using plain language statements such as “Allow Managers to access the WAN using a mobile device.”

## 5. Rule-Based Access Control
- It applies global rules to all subjects. Rules within the rule-based access control model are sometimes referred to as restrictions or filters.
- **Example:**  A firewall uses rules that allow or block traffic to all users equally

## 6. Risk-Based Access Control (RBAC, but different from Role-Based)
- Grants access after evaluating risk. It evaluates the environment and the situation and makes risk-based decisions using policies embedded within software code. It uses machine learning to make predictive conclusions about current activity based on past activity.
- **Example:**
  - If a user logs in from an unusual location, require extra authentication.
  - If a device is outdated or unpatched, limit its access.
- Used in zero-trust security and fraud prevention.

## 7. Identity-Based Access Control (IBAC)
- A subset of Discretionary Access Control. Systems identify users based on their identity and assign resource ownership to identities
- **Example:** A CEO might have unique permissions that no other employees have.

## 8. Policy-Based Access Control (PBAC)
- Uses a set of policies to define who can do what.
- **Example:** Cloud providers like AWS use PBAC to let organizations define custom rules for access.

## 9. Non-discretionary Access Control
- Administrators centrally administer nondiscretionary access controls and can make changes that affect the entire environment. Access doesn’t focus on user identity. Instead, a static set of rules governing the whole environment manage access. Non-DAC systems are centrally controlled and easier to manage (although less flexible). In general, any model that isn’t a discretionary model is a nondiscretionary model.

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
