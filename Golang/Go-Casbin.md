# Casbin

Casbin is a powerful and efficient open-source access control library for GoLang. It provides support for access control models like ACL, RBAC, ABAC, and more. With Casbin, you can easily define permissions for your application and enforce access control policies in a flexible and scalable way.

The `model.conf` and `policy.csv` files are used by Casbin to define the access control model and policies, respectively. It's important to note that the `model.conf` and `policy.csv` files can be modified to suit your specific access control requirements. Casbin supports various access control models, including RBAC, ABAC (Attribute-Based Access Control), and custom models.
## Model Configuration 

The `model.conf` file defines the access control model used by Casbin. It specifies the types of entities (e.g., users, roles, objects) and the relationships between them. Here's an example of a basic RBAC (Role-Based Access Control) model:
