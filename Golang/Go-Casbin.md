# Casbin

Casbin is a powerful and efficient open-source access control library for GoLang. It provides support for access control models like ACL, RBAC, ABAC, and more. With Casbin, you can easily define permissions for your application and enforce access control policies in a flexible and scalable way.

The `model.conf` and `policy.csv` files are used by Casbin to define the access control model and policies, respectively. It's important to note that the `model.conf` and `policy.csv` files can be modified to suit your specific access control requirements. Casbin supports various access control models, including RBAC, ABAC (Attribute-Based Access Control), and custom models.
## Model Configuration 

The `model.conf` file defines the access control model used by Casbin. It specifies the types of entities (e.g., users, roles, objects) and the relationships between them. Here's an example of a basic RBAC (Role-Based Access Control) model:

```conf
# Request definition
[request_definition]
r = sub, obj, act

# Policy definition
[policy_definition]
p = sub, obj, act

# Role definition
[role_definition]
g = _, _

# Policy effect
[policy_effect]
e = some(where (p.eft == allow))

# Matchers
[matchers]
m = g(r.sub, p.sub) && r.obj == p.obj && r.act == p.act
```

- `[request_definition]` defines the structure of a request (e.g., `sub` for subject, `obj` for object, `act` for action).
- `[policy_definition]` defines the structure of a policy rule.
- `[role_definition]` defines the structure of roles and their hierarchy.
- `[policy_effect]` specifies how the final access decision is determined (e.g., if any policy allows access).
- `[matchers]` defines how the requested access is matched against the policies.

There are a couple of ways for loading the model configuration.

1. Load from a `.conf` file
2. Load from Code
3. Load from String

### Request

Defines the request parameters. A basic request is a tuple object, requiring at least a subject (accessed entity), object (accessed resource), and action (access method).

For instance, a request definition may look like this: `r={sub,obj,act}`

This definition specifies the parameter names and ordering required by the access control matching function.

### Policy

Defines the model for the access strategy. It specifies the name and order of the fields in the Policy rule document.

For instance: `p={sub, obj, act}` or `p={sub, obj, act, eft}`

Note: If eft (policy result) is not defined, the result field in the policy file will not be read, and the matching policy result will be allowed by default.

### Matcher

Defines the matching rules for Request and Policy.

For example: `m = r.sub == p.sub && r.act == p.act && r.obj == p.obj` This simple and common matching rule means that if the requested parameters (entities, resources, and methods) are equal to those found in the policy, then the policy result (`p.eft`) is returned. The result of the strategy will be saved in `p.eft`.

### Effect

Performs a logical combination judgment on the matching results of Matchers.

For example: `e = some(where(p.eft == allow))`

This statement means that if the matching strategy result `p.eft` has the result of (some) allow, then the final result is true.

Let's look at another example:

`e = some(where (p.eft == allow)) && !some(where (p.eft == deny))`

The logical meaning of this example combination is: if there is a strategy that matches the result of allow and no strategy that matches the result of deny, the result is true. In other words, it is true when the matching strategies are all allow. If there is any deny, both are false (more simply, when allow and deny exist at the same time, deny takes precedence).
