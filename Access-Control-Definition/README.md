# Access Control Models

## 1. Discretionary Access Control (DAC)
- Every object has an owner and the owner can grant or deny access to any other subjects.
- **Example:** The New Technology File System (NTFS), used on Microsoft Windows operating systems, uses the DAC model.

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
- Grants access based on a specific user’s identity rather than their role.
- **Example:** A CEO might have unique permissions that no other employees have.

## 8. Policy-Based Access Control (PBAC)
- Uses a set of policies to define who can do what.
- **Example:** Cloud providers like AWS use PBAC to let organizations define custom rules for access.

---

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
