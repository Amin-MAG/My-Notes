# ENT Database

The **ent** is a simple & powerful entity framework for Go.

```bash
# To download 
go get -d entgo.io/ent/cmd/ent
```

# Get started

## Create a schema

To create a new User schema

```bash
go run entgo.io/ent/cmd/ent init User
```

To add new fields to this entity you need to use the Fields function

```go
// Fields of the User.
func (User) Fields() []ent.Field {
	return []ent.Field{
		field.Int("age"),
		field.String("name"),
		field.String("username").Unique(),
		field.Time("created_at").Default(time.Now),
	}
}
```

You need to run this command to generate the required files

```bash
go generate ./ent
```

## Create a new entity

To create new client and auto migrate

```go
func main() {
	log.Println("Going to start the ent app...")

	// Create new ent client
	client, err := ent.Open("sqlite3", "file:ent?mode=memory&cache=shared&_fk=1")
	if err != nil {
		log.Fatalf("failed opening connection to sqlite: %v", err)
	}

	// Close the client
	defer func(client *ent.Client) {
		err := client.Close()
		if err != nil {
			log.Println("Can not close the ent client")
		}
	}(client)

	// Run the auto migration tool.
	if err := client.Schema.Create(context.Background()); err != nil {
		log.Fatalf("failed creating schema resources: %v", err)
	}
}
```

> Note: Consider importing the `github.com/mattn/go-sqlite3`
> 

Now, Let’s create a new user entity

```go
func CreateUser(ctx context.Context, client *ent.Client) (*ent.User, error) {
	u, err := client.User.
		Create().
		SetAge(24).
		SetName("Amin").
		SetCreatedAt(time.Now()).
		SetUsername("amin_mag").
		Save(ctx)
	if err != nil {
		return nil, fmt.Errorf("failed creating user: %w", err)
	}
	log.Println("User was created: ", u)

	return u, nil
}
```

The `Create()` returns a builder. Then we can set the values for this entity and at last use the `Save()` to save the data into the database.

## Query an entity

```go
func QueryUser(ctx context.Context, client *ent.Client) (*ent.User, error) {
	u, err := client.User.
		Query().
		Where(user.NameContains("in")).
		// `Only` fails if no user found,
		// or more than 1 user returned.
		Only(ctx)
	if err != nil {
		return nil, fmt.Errorf("failed querying user: %w", err)
	}
	log.Println("User is returned: ", u)

	return u, nil
}
```

The `Query()` returns a builder. The `Only` fails if no user is found or more than one user returned.

## Add a new edge

First, Let’s create organization and device entities.

```bash
# As we know
go run entgo.io/ent/cmd/ent init Organization Device
```

The Organization entity

```go
// Fields of the Organization.
func (Organization) Fields() []ent.Field {
	return []ent.Field{
		// Regexp validation for group name.
		field.String("name").Match(regexp.MustCompile("[a-zA-Z_]+$")),
	}
}
```

The Device entity

```go
// Fields of the Device.
func (Device) Fields() []ent.Field {
	return []ent.Field{
		field.String("model"),
		field.String("serial").Unique(),
		field.Time("registered_at"),
	}
}
```

Each user can have one or more devices in our application. Let’s create the first relation (edge).

```go
// Edges of the User.
func (User) Edges() []ent.Edge {
	return []ent.Edge{
		edge.To("devices", Device.Type),
	}
}
```

Then generate the files

```bash
# To generate the files
go generate ./ent
```

To add one or more devices to a user, you can use the `AddDevices()` method.

```go
func CreateUserWithDevices(ctx context.Context, client *ent.Client) (*ent.User, error) {
	// Create a new device.
	samsung, err := client.Device.
		Create().
		SetModel("Samsung").
		SetRegisteredAt(time.Now()).
		SetSerial("WxStSwqL").
		Save(ctx)
	if err != nil {
		return nil, fmt.Errorf("failed creating the device: %w", err)
	}
	log.Println("Device was created: ", samsung)

	// Create another device.
	acer, err := client.Device.
		Create().
		SetModel("Acer").
		SetRegisteredAt(time.Now().Add(time.Second * 2)).
		SetSerial("owRkVSAo").
		Save(ctx)
	if err != nil {
		return nil, fmt.Errorf("failed creating device: %w", err)
	}
	log.Println("Device was created: ", acer)

	// Create the user with bunch of devices
	u, err := client.User.
		Create().
		SetAge(24).
		SetName("Mathew").
		SetCreatedAt(time.Now()).
		SetUsername("mathew").
		AddDevices(samsung, acer).
		Save(ctx)
	if err != nil {
		return nil, fmt.Errorf("failed creating user: %w", err)
	}
	log.Println("User was created: ", u)

	return u, nil
}
```

To query user devices, you can call `QueryDevices()` on the user.

```go
func QueryDevices(ctx context.Context, u *ent.User) error {
	// To query all devices
	devices, err := u.QueryDevices().All(ctx)
	if err != nil {
		return fmt.Errorf("failed querying user devices: %w", err)
	}
	log.Println("Returned devices for this users:", devices)

	// To filter some devices of the user
	acer, err := u.QueryDevices().
		Where(device.ModelContains("Acer")).
		Only(ctx)
	if err != nil {
		return fmt.Errorf("failed querying user devices: %w", err)
	}
	log.Println("Returned filter devices for this user:", acer)

	return nil
}
```

## Inverse Edges

Let’s think that we have a Samsung device and want to find the user owner. To add this inverse edge

```go
func QueryDeviceOwner(ctx context.Context, u *ent.User) error {
	devices, err := u.QueryDevices().All(ctx)
	if err != nil {
		return fmt.Errorf("failed querying user devices: %w", err)
	}

	// Query the inverse edge.
	for _, d := range devices {
		owner, err := d.QueryOwner().Only(ctx)
		if err != nil {
			return fmt.Errorf("failed querying device %q owner: %w", d.Model, err)
		}
		log.Printf("Device %q owner: %q\n", d.Model, owner.Name)
	}

	return nil
}
```

Create an inverse-edge called "owner" of type `User` and reference it to the "cars" edge (in User schema) explicitly using the `Ref` method.

Setting the edge to `unique` ensures that a car can have only one owner.

## Graph Traversal

I added another Organization entity. To have a graph, Let’s create some entities and connect them together.

```go
func CreateGraph(ctx context.Context, client *ent.Client) error {
	// First, create the users.
	emily, err := client.User.
		Create().
		SetAge(30).
		SetName("Emily").
		SetUsername("emily1991").
		SetCreatedAt(time.Now()).
		Save(ctx)
	if err != nil {
		return err
	}
	john, err := client.User.
		Create().
		SetAge(28).
		SetName("John").
		SetUsername("john_smith").
		SetCreatedAt(time.Now().Add(time.Hour)).
		Save(ctx)
	if err != nil {
		return err
	}

	// Then, create the devices and attach them to the users in the creation.
	err = client.Device.
		Create().
		SetModel("Samsung").
		SetRegisteredAt(time.Now()).
		SetSerial("riowFowQ").
		// attach this graph to Emily.
		SetOwner(emily).
		Exec(ctx)
	if err != nil {
		return err
	}
	err = client.Device.
		Create().
		SetModel("Acer").
		SetRegisteredAt(time.Now()).
		SetSerial("pqWORIFa").
		// attach this graph to Emily.
		SetOwner(emily).
		Exec(ctx)
	if err != nil {
		return err
	}
	err = client.Device.
		Create().
		SetModel("Huawei").
		SetRegisteredAt(time.Now()).
		SetSerial("nvhSHFWi").
		// attach this graph to John.
		SetOwner(john).
		Exec(ctx)
	if err != nil {
		return err
	}

	// Create the organizations, and add their users in the creation.
	err = client.Organization.
		Create().
		SetName("Google").
		AddUsers(john, emily).
		Exec(ctx)
	if err != nil {
		return err
	}
	err = client.Organization.
		Create().
		SetName("Discord").
		AddUsers(emily).
		Exec(ctx)
	if err != nil {
		return err
	}
	log.Println("The graph was created successfully")

	return nil
}
```

`Exec()` executes the query and returns an error, but `Save()` returns the created entity.

You can use `SetOwner()` to specify the user owner in the device.

### Queries

1. To get all devices in Google organization

```go
func QueryGoogleOrgDevices(ctx context.Context, client *ent.Client) error {
	devices, err := client.Organization.
		Query().
		Where(organization.Name("Google")).
		QueryUsers().
		QueryDevices().
		All(ctx)
	if err != nil {
		return fmt.Errorf("failed getting devices: %w", err)
	}
	log.Println("Devices returned:", devices)

	return nil
}
```

1. To get all devices owned by Emily or Emily’s Organizations except those whose names are “Acer”

```go
func QueryEmilyOrganizationsDevicesExceptAcer(ctx context.Context, client *ent.Client) error {
	emily := client.User.
		Query().
		Where(
			user.UsernameContains("emily"),
			user.NameContains("Emily"),
		).
		OnlyX(ctx)
	devices, err := emily.
		QueryOrganizations().
		QueryUsers().
		QueryDevices().
		Where(
			device.Not(
				device.ModelContains("Acer"),
			),
		).
		All(ctx)
	if err != nil {
		return fmt.Errorf("failed getting devices: %w", err)
	}
	log.Println("Devices returned:", devices)

	return nil
}
```

`OnlyX()` panics if only and just only one item doesn’t exist.

1. To get all Organizations with at least one user

```go
func QueryOrganizationsWithUsers(ctx context.Context, client *ent.Client) error {
	organizations, err := client.Organization.
		Query().
		Where(organization.HasUsers()).
		All(ctx)
	if err != nil {
		return fmt.Errorf("failed getting organizations: %w", err)
	}
	log.Println("Organizations returned:", organizations)

	return nil
}
```

Full code of this section: 

[](https://github.com/Amin-MAG/Ent/releases/tag/get-started)