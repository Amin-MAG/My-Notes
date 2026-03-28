---
title: Lua
draft: true
tags: [programming]
---
# Lua

Lua is a lightweight scripting language with higher performance than [Python](Python.md). It does not support classes.

Here is a Hello World script:

```lua
local hello = "hello world"
hello2 = "hello mom"
print(hello)
```

To execute the code:

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

Tables are the primary data structure in Lua, used to define arrays, maps, and more.

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
	coroutine.yield("beginning")
	coroutine.yield("middle")
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

# See More

- [Programming Languages](ProgrammingLanguages.md)