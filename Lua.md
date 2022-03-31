# Lua

It is a language easier than python with higher performance. It doesn’t support the classes.

Here is a hello world script

```lua
local hello = "hello world"
hello2 = "hello mom"
print(hello)
```

To execute the code

```bash
lua app.lua
```

## Functions

```lua
function doMath(n):
	return n*2
end

doMath(3)
```

## Tables

It’s a type of data structure that you can define arrays, maps,...

### Arrays

> It will start with index `1`
> 

```lua
array = {"Landing", "on", "Lua"}
```

### Dictionaries

```lua
dict = {
	["moon"] = "moon",
	["cheese"] = "cheese"
}

for k,v in pairs(dict) do
	print(k, v)
end
```

## Coroutines

You can use `coroutine.yield` to suspend its execution. You can use `coroutine.resume` to continue the execution.

```lua
co = coroutine.create(function()
	coroutine.yeild("beginning")
	coroutine.yeild("middle ")
	return "end"
end)

coroutine.resume(co)
coroutine.resume(co)
coroutine.resume(co)
```

## Package Manager

The package manager is called `luarocks`.

# References

[Lua in 100 Seconds](https://www.youtube.com/watch?v=jUuqBZwwkQw)