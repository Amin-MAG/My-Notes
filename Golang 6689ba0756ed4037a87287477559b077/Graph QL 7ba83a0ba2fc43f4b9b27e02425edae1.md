# Graph QL

# Initial Setup

Create a directory for the project and initialize the go modules file:

```bash
go mod init github.com/[username]/hackernews
```

after using `init` command to set up a gqlgen project.

```bash
go run github.com/99designs/gqlgen init
```

Here is a description from gqlgen about the generated files:

- `gqlgen.yml` — The gqlgen config file, knobs for controlling the generated code.
- `graph/generated/generated.go` — The GraphQL execution runtime, the bulk of the generated code.
- `graph/model/models_gen.go` — Generated models required to build the graph. Often you will override these with your own models. Still very useful for input types.
- `graph/schema.graphqls` — This is the file where you will add GraphQL schemas.
- `graph/schema.resolvers.go` — This is where your application code lives. generated.go will call into this to get the data the user has requested.
- `server.go` — This is a minimal entry point that sets up an HTTP.Handler to the generated GraphQL server. start the server with `go run server.go` and open your browser and you should see the graphql playground, So the setup is right!

To generate the files:

```bash
go run github.com/99designs/gqlgen generate
```

# Check mutation or query

```go
if op := graphql.GetOperationContext(ctx).Operation; op == nil || op.Operation != ast.Mutation {
		return next(ctx)
}
```

# References

[Building a GraphQL Server with Go Backend Tutorial | Getting Started](https://www.howtographql.com/graphql-go/1-getting-started/)