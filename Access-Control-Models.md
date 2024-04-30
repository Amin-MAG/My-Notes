# Access Control Models

Authenticating a user is not Authorization. It happens a lot that users can login to their account, but they do not have similar access to all features of applications or all pages of a website. Authorization is the concept that let a user read or write something. There several Access Control Models that can help us in Authorizing users.


1. **Discretionary Access Control (DAC)**
	- Resource owners (users or subjects) have the discretionary authority to grant or revoke access rights to other users or subjects.
	- The owner of a resource (e.g., a file, directory, or system object) can decide who should have access to that resource and what type of access (read, write, execute) they should have.
	- Users can pass their access rights or permissions to other users if they have the authority to do so.
	- Authorization decisions are based on the identity of the subject and the access rights granted by the resource owner.
	- This model is commonly used in commercial and personal computing environments, where users have control over their own resources.
2. **Non-Discretionary Access Control (NDAC)**
	- Access rights are determined and enforced by a central authority or system administrator.
	- Users and subjects (processes, programs, etc.) are assigned specific access permissions based on predefined rules and policies.
	- Users and subjects have no control over the access rights granted to them or the ability to transfer their permissions to others.
	- Authorization decisions are made based on the subject's identity, the object's classification level, and the access rules defined by the central authority.
	- This model is commonly used in military and government environments where strict control over access to sensitive information is required.
3. **Dynamic Access Control (DAC)**
	- It is more flexible and adaptive approach that combines aspects of NDAC and DAC models.
	- Access decisions are made based on a wide range of contextual attributes, such as the user's identity, role, location, time of access, environmental conditions, and risk factors.
	- Access rules and policies can be dynamically adjusted based on changing circumstances or risk levels.
	- Authorization decisions are made by evaluating various attributes and policies in real-time, rather than relying solely on predefined rules or owner-granted permissions.
	- This model is gaining popularity in cloud computing, IoT, and other dynamic environments where access requirements can change rapidly based on various factors.

There are a couple of differences in these models. The main differences in authorization among these models are:

- In NDAC, authorization is centrally controlled and enforced based on predefined rules and policies set by an authority.
- In DAC, authorization is based on the discretion of resource owners, who can grant or revoke access rights to other users or subjects.
- In dynamic access control, authorization is based on a combination of attributes, contextual factors, and policies that can be dynamically adjusted in real-time to adapt to changing circumstances or risk levels.

## Discretionary Access Control 

In this model, resource owners (users or subjects) have the discretion to grant or revoke access rights to other users or subjects. The owner of a resource can decide who should have access and what type of access (read, write, execute) they should have. Authorization is based on the identity of the subject and the access rights granted by the resource owner.

One of the Discretionary Access Control models is ACL.

### ACL

ACL stands for Access Control List. It is a method in which users are mapped to actions and actions to resources.

Here is a simple ACL Model in [Go-Casbin](Golang/Go-Casbin.md) Fromat:

```conf
[request_definition]  
r = sub, act, obj  
  
[policy_definition]  
p = sub, act, obj  
  
[policy_effect]  
e = some(where (p.eft == allow))  
  
[matchers]  
m = r.sub == p.sub && r.obj == p.obj && r.act == p.act
```
