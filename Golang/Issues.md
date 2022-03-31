# Issues

## Go get my library, please!

I have trouble in `go get <my-library>`.

Then I tried to do this:

```bash
export GO111MODULE=on
export GOPROXY=direct
export GOSUMDB=off
go get -v -u github.com/alessiosavi/GoGPUtils@v0.0.9
```

Finally I get another error that forced me to use:

```bash
go clean -modcache
```

It deletes all the packages I think. So, never use it :)).