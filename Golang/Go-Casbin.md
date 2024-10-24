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

## Policy

Policies contains the actual access control policies. Policies are flexible to be stored in different places. You just need to use the correct Adapter.


### General Adapter

You can store the policies in any way if you have the adapter.

| Method                 | Type     | Description                                                |
| ---------------------- | -------- | ---------------------------------------------------------- |
| LoadPolicy()           | basic    | Load all policy rules from the storage                     |
| SavePolicy()           | basic    | Save all policy rules to the storage                       |
| AddPolicy()            | optional | Add a policy rule to the storage                           |
| RemovePolicy()         | optional | Remove a policy rule from the storage                      |
| RemoveFilteredPolicy() | optional | Remove policy rules that match the filter from the storage |

### CSV Adapter

The `policy.csv` file contains the actual access control policies in a CSV format. Each line represents a policy rule, and the columns correspond to the entities defined in the `model.conf` file. Here's an example of a policy file:

```csv
p,alice,data1,read
p,bob,data2,write
g,alice,admin
g,admin,data_reader
```

- The first line allows the user `alice` to `read` `data1`.
- The second line allows the user `bob` to `write` `data2`.
- The third line assigns the `admin` role to the user `alice`.
- The fourth line defines that the `admin` role inherits the `data_reader` role.

> The structure of the `policy.csv` file depends on the access control model defined in the `model.conf` file. The order and meaning of the columns in the CSV file correspond to the entities defined in the model.

### Database Adapter 

Corresponding database structure (such as MySQL)

| id  | ptype | v0          | v1    | v2    | v3  | v4  | v5  |
| --- | ----- | ----------- | ----- | ----- | --- | --- | --- |
| 1   | p     | data2_admin | data2 | read  |     |     |     |
| 2   | p     | data2_admin | data2 | write |     |     |     |
| 3   | g     | alice       | admin |       |     |     |     |

Meaning of each column:
- `id`: The primary key in the database. It does not exist as part of the Casbin policy. The way it is generated depends on the specific adapter.
- `ptype`: It corresponds to `p`, `g`, `g2`, etc.
- `v0-v5`: The column names have no specific meaning and correspond to the values in the policy CSV from left to right. The number of columns depends on how many you define yourself. In theory, there can be an infinite number of columns, but generally only **6** columns are implemented in the adapter. If this is not enough for you, please submit an issue to the corresponding adapter repository.

You can keep track of actual policies and roles in a database like MySQL. Here is an example

```go
package main

import (
	"log"

	"github.com/casbin/casbin/v2"
	"github.com/casbin/casbin/v2/model"

	xormadapter "github.com/casbin/xorm-adapter/v2"
	_ "github.com/go-sql-driver/mysql"
)

func main() {
	// Initialize a Xorm adapter with MySQL database.
	sqlAdapter, err := xormadapter.NewAdapter("mysql", "admin:admin@tcp(127.0.0.1:3306)/")
	if err != nil {
		log.Fatal("error creating the casbin adapter for mysql: ", err.Error())
	}

	m, err := model.NewModelFromFile("model.conf")
	if err != nil {
		log.Fatal("can not creat the model from the file: ", err.Error())
	}

	e, err := casbin.NewEnforcer(m, sqlAdapter)
	if err != nil {
		log.Fatal("can no create the enforcer in casbin: ", err.Error())
	}
	// ...
}
```

# See More

- [Access-Control-Models](../Access-Control-Models.md)
