# Go Effective

## Formatting

## Commentary

## Names

### Package names

short, concise, evocative. By convention, packages are given lower case, single-word names. `src/encoding/base64` is imported as `"encoding/base64"` but has name `base64`, not `encoding_base64` and not `encodingBase64`.

### Getters & Setters

 If you have a field called `owner` (lower case, unexported), the getter method should be called `Owner` (upper case, exported), not `GetOwner`. A setter function, if needed, will likely be called `SetOwner`. Both names read well in practice:
 
```go
owner := obj.Owner()
if owner != user {
    obj.SetOwner(user)
}
```

### Interface

By convention, one-method interfaces are named by the method name plus an `-er` suffix or similar modification to construct an agent noun: `Reader`, `Writer`, `Formatter`, `CloseNotifier` etc.

