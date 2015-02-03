---
title: Lua 5.1 Style Guide
---
# lua-users.org Lua Style Guide
Original: http://lua-users.org/wiki/LuaStyleGuide

This document is a markdown formatted version of the Lua Users Lua 5.1 Style Guide. Some of it is a little out-dated or questionable, but efforts to improve and modernise it will be made over time.

## Formatting

### Indentation
Indenting often uses two spaces. This is followed in Programming in Lua, the Lua Reference Manual, Beginning Lua Programming, and the Lua users wiki. (Why this is the case, I don't know, but perhaps it is because Lua statements can tend to be deeply nested, even in a LISP or functional manner, or perhaps it is affected by the fact that the code in these examples is small and pedagogical.) You'll see other common conventions (e.g. 3-4 spaces or tabs).

```lua
for i,v in ipairs(t) do
  if type(v) == "string" then
    print(v)
  end
end
```
