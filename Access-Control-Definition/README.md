# Access Control Models

## 1. Discretionary Access Control (DAC)
- The owner of a resource (file, database, etc.) decides who gets access.
- **Example:** In Windows, if you create a file, you can set permissions for other users.
- **Downside:** Not very secure—users can grant access freely.

## 2. Mandatory Access Control (MAC)
- Access is controlled by system-enforced policies (not the user).
- Common in military and government systems where security classifications matter.
- **Example:** Top Secret documents can only be accessed by people with Top Secret clearance.

## 3. Role-Based Access Control (RBAC)
- Users are assigned roles, and roles have permissions.
- **Example:**
  - Admins can access everything.
  - Managers can approve requests but not change system settings.
  - Employees can only view their own data.
- Common in corporate IT systems.

## 4. Attribute-Based Access Control (ABAC)
- Access is granted based on attributes (user, resource, environment).
- More flexible than RBAC because it doesn’t rely on fixed roles.
- **Example:**
  - "Doctors can view patient records only during work hours."
  - "Employees can access the system only from company devices."

## 5. Rule-Based Access Control
- Uses if-then rules to grant/deny access.
- **Example:**
  - "Block all employees from logging in after 10 PM."
  - "Allow only encrypted devices to access the network."
- Often used with other models like RBAC or ABAC.

## 6. Risk-Based Access Control (RBAC, but different from Role-Based)
- Access depends on a risk score calculated in real-time.
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

## Final Thoughts
- **If flexibility is needed →** ABAC is the best choice.
- **If strict rules are needed →** MAC is best.
- **If simplicity & structure are important →** RBAC works well.
- **If dynamic decisions are required →** Risk-Based Access Control helps.

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
