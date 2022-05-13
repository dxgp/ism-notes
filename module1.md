## Introduction
Identity and access management (IAM or IdAM) is a way to tell who a user is and what they're allowed to do. Technically, it's a means of managing a given set of users' digital identities and privelages associated with each identity. 

**Identity** - A certain set of properties that can be conveniently measured and recorded digitally.

A few digital identity artefacts are:

* User-ID, username, password
* DOB
* Phone No.
* Purchasing/Medical History
* Government issued identity
* Electronic transaction records

**Authentication** - A process where the user proves his identity to gain access to a resource such as application, system, device and so on. It's usually performed by checking the resource access request against a set of authorization policies typically stored in the backend.

Single factor:

| Authenticators | Implementation | Usability | Risk | Risk Mitigation |
|---|---|---|---|---|
| 1. Single challenge<br>2. One authenticator. | 1. Inexpensive<br>2. Simple | Easy to use | 1. Easier to break.<br>2. Password Leaks | 1. Password Complexity<br>2. Periodic password change.<br>3. Different passwords per application. |

Multi factor:

| Authenticators | Implementation | Usability | Risk | Risk Mitigation |
|---|---|---|---|---|
| 1. Multiple authenticators<br>2. E.g. p/w + OTP | 1. Expensive.<br>2. Complex wrt SFA. | Biometrics can get slightly complex. | 1. Difficult to break.<br>2. Probability of compromising all authenticators is low.  | 1. Password Complexity<br>2. Keeping authentication factors secure.<br>3. Different passwords per application. |

## Principles of Authorization

1. **SOD** - Seggregation of Duties: Primarily separates the responsibilities associated with an action of process to decrease the opportunity for misbehaviour or policy violations. Concept of separate developer and tester.

2. **Need to Know Basis**: Access to systems should only be granted to entities with a legitimate need to know the information contained within those systems. This principle prevents over-exposure of sensetive information which then becomes likely to be misused.

3. **Principle of Least Privelage**: This principle states that individuals or systems should only have the necessary access to perform specific activities required of their job or role. When entities or individuals are not performing work on these types of activities, they should use the privileged access accounts. It ensures that what can be done with the information is limited to the least amount of access necessary to perform those actions.

4. **Unsuccessful logons**: Limiting the number of unsuccessful logons by a user during a specific period can reduce the potential for guessing credentials.

5. **Session concurrency/Last login**: Notifying the end user of the last login can provide information to the end user in order to determine if unauthorized access was obtained using their account. Concurrent sessions occur when two or more sessions exist at the same time. This may be prohibited because most users would only create a single session for system access. Also, additional sessions using the same credentials may indicate malicious activity.

6. **Notification of System Usage**: Banners and login displays confirm the last usage of the account with the user to make sure that no third party has accessed their account.

## Access Control

Access control is referred to as a security method used to control who or what can access resources in the digital environment. Key concerns of an access control scheme are:

1. Prevent access: No Privelage => No Access
2. Determine access: Action with an object is taken after the access decision.
3. Grant access: Access provision
4. Revoke access: Removal of access.
5. Audit access: Determine access trails.

There are different types of access controls. They are listed below:

| Framework | Properties |
|---|---|
| **Mandatory Access Control (MAC)** | 1. MAC is a way of assignment of access rights based on policies /restrictions enforced by a designated central authority.<br>2. Restricts the power of individual resource owners to objects in a system.<br>3. Usually employed in military or government domain.<br>4. Typically uses assignment of classification label to each file system object.<br>5. Each system user is allotted a classification level.<br>6. Individual resource owners are not allowed to make their own assignments of access permissions to other users and entities. |
| **Discretionary Access Control (DAC)** | 1. A way to allocate access rights on the basis of rules specified by information owners.<br>2. The fundamental concept is that the owners can govern access to files and objects. |
| **Role Based Access Control (RBAC)** | 1. Works on the basis of a "role" of a user within an enterprise.<br>2. RBAC is a strong security control because it enables users to access only information relevant to their "role".<br>3. User permissions are mapped to specific enterprise roles and whenever an employee changes the role, the corresponding access permissions also change.  |
| **Rule Based Access Control (RuBAC)** | 1. Strategy to manage user access on multiple systems based on dynamically triggered rules.<br>2. RuBAC is also referred to as "Automated Provisioning".<br>3. With RuBAC, once a request for accessing a resource is sent, some security control would verify the properties of the request against a set of pre-defined rules. |



