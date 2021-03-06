---
title: Lua 5.1 Style Guide
layout: default
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

## Naming

### Variable Name Length
It's a general rule (not necessarily a Lua one) that variable names with larger scope should be more descriptive than those with smaller scope. For example, i is probably a poor choice for a global variable in a large program, but it's perfectly fine as a counter in a small loop.

### Value and object variable naming
Variables holding values or objects are typically lowercase and short (e.g. color). The book Beginning Lua Programming uses CamelCase for user variables, but this seems to largely be for pedagogical reasons to clearly distinguish user-defined variables from built-in ones.
##### Booleans
It can be helpful to prefix Boolean values or functions used as predicates with is, such as is_directory rather than directory (which might store a directory object itself).

### Function naming
Function often follows similar rules to value and object variable naming (functions being first class objects). A function name consisting of multiple words may run together as in (`getmetatable`), as done in the standard Lua functions, though you may choose to use underscores (`print_table`). Some from other programming backgrounds use `CamelCase`, as in `obj::GetValue()`.

### Lua internal variable naming
The [[Lua 5.1 Reference Manual - Lexical Conventions][LuaMan-Lexical]] says:

> As a convention, names starting with an underscore followed by uppercase letters (such as `_VERSION`) are reserved for internal global variables used by Lua."

These are often constants but not necessarily, e.g. `_G`.

[LuaMan-Lexical]: http://www.lua.org/manual/5.1/manual.html#2.1

### Constants naming
Constants, particularly ones that are simple values, are often given in `ALL_CAPS`, with words optionally separated by underscores (e.g. `MAXLINES` in PiL2, 4.3 and also the Markov example in PiL2, Listing 10.6).

### Module/package naming
Module names are often nouns with names that are short and lowercase, with nothing between words. At least, that's the general pattern noticed in Kepler: `luasql.postgres` (not `Lua-SQL.Postgres`).

Examples:
`lxp`
`luasql`
`luasql.postgres`
`luasql.mysql`
`luasql.oci8`
`luasql.sqlite`
`luasql.odbc`
`socket`
`xmlrpc`
`xmlrpc.http`
`soap`
`lualdap`
`logging`
`md5`
`zip`
`stable`
`copas`
`lxp`
`lxp.lom`
`stable`
`lfs`
`htk`

However, see comments in the Modules section below concerning modules used as classes.

### `_` as a variable name
The variable consisting of only an underscore `_` is commonly used as a placeholder when you want to ignore the variable:

```lua
for _,v in ipairs(t) do print(v) end
```
**Note:** This resembles the use of `_` in Haskell, Erlang, Ocaml, and Prolog languages, where `_` takes the special meaning of anonymous (ignored) variables in pattern matches. In Lua, `_` is only a convention with no inherent special meaning though. Semantic editors that normally flag unused variables may avoid doing so for variables named `_` (e.g. [LuaInspect] is such a case).

[LuaInspect]: http://lua-users.org/wiki/LuaInspect

### Keys and Values in Tables
`i`, `k`, `v`, and `t` are often used as follows:

```lua
for k,v in pairs(t) ... end
for i,v in ipairs(t) ... end
mt.__newindex = function(t, k, v) ... end
```

### `M` as the current module
`M` is sometimes used as the "current module table" (e.g., see PIL2, 15.3).

### `self`
`self` refers to the object a method is invoked on (like `this` in C++ or Java). In fact, this is enforced by the `:` syntactic sugar:

```lua
function Car:move(distance)
  self.position = self.position + distance
end
```

### Class names
(or at least metatables representing classes) may be mixed case (`BankAccount`), or they might not be. If so, acronyms (e.g. `XML`) might only uppercase the first letter (`XmlDocument`).

### [Hungarian notations]
Encoding semantic information into variable names can be helpful to readers of your code, particularly if that information cannot otherwise be easily deduced, though overdoing it may be redundant and reduce code readability. In some more static languages (e.g. C), where data types are known to the compiler, it is redundant to encode the data type into the variable name, but, even there, data type is not the only nor necessarily the most useful form of semantic information.

```lua
local function paint(canvas, ntimes)
  for i=1,ntimes do
    local hello_str_asc_lc_en_const = "hello world"
    canvas:draw(hello_str_asc_lc_en_const:toupper())
  end
end
```
The variable naming conventions in the above example imply the following to the reader.

- `canvas` is a canvas object, perhaps one explicitly derived from something like `Canvas`, which is information possibly not otherwise easy to deduce from the code.
- `ntimes` is an integer number of times to draw something.
- `i` is conventionally an integer index, which though obvious, even if called something else, also keeps the code short.
- The `_str_asc_lc_en_const` is redundant, perhaps even confusing, and can be removed: obviously this variable is a constant, English, lowercase, ASCII string.

[Hungarian Notations]: http://en.wikipedia.org/wiki/Hungarian_notation


## Libraries and Features
Avoid using the debug library unless necessary, especially if trusted code is being run. (Using the debug library is sometimes a hack: [StringInterpolation].)

Avoid deprecated features. In 5.1 these include `table.getn`, `table.setn`, `table.foreach[i]`, and `gcinfo`. Refer to the [[Lua 5.1 Reference Manual - 7 - Incompatibilities with the Previous Version][LuaMan-Deprecations]] for alternatives.

[StringInterpolation]: http://lua-users.org/wiki/StringInterpolation
[LuaMan-Deprecations]: http://www.lua.org/manual/5.1/manual.html#7


## Scope
Use locals rather than globals whenever possible.

```lua
local x = 0
local function count()
  x = x + 1
  print(x)
end
```
Globals have larger scopes and lifetimes and therefore increase [[coupling]] and complexity. [[1]] Don't pollute the environment. In Lua, access to locals is also faster than globals [[PiL 4.2]] since globals require a table lookup at run-time, while locals exist as registers [[ScopeTutorial]].

A useful method of detecting inadvertent use of globals is given in [DetectingUndefinedVariables][]. In Lua, globals are sometimes the result of mispellings and other lurking errors in your code.

At times it is useful to further limit the scope of local variables with do-blocks [[PiL 4.2]]:

```lua
local v
do
  local x = u2*v3-u3*v2
  local y = u3*v1-u1*v3
  local z = u1*v2-u2*v1
  v = {x,y,z}
end

local count
do
  local x = 0
  count = function() x = x + 1; return x end
end
```
Global variables scope can be reduced as well via the Lua module system [PiL2 15] or [setfenv].

[coupling]: http://en.wikipedia.org/wiki/Coupling_(computer_science)
[1]: http://gpwiki.org/index.php/Programming_Techniques:Global_Variables
[PiL 4.2]: http://www.lua.org/pil/4.2.html
[ScopeTutorial]: http://lua-users.org/wiki/ScopeTutorial
[DetectingUndefinedVariables]: http://lua-users.org/wiki/DetectingUndefinedVariables
[PiL2 15]: http://www.inf.puc-rio.br/~roberto/pil2/chapter15.pdf
[setfenv]: http://www.lua.org/manual/5.1/manual.html#pdf-setfenv


## Modules
The Lua 5.1 module system is often recommended. However, there have been some criticisms of the Lua 5.1 module system. For details, see [LuaModuleFunctionCritiqued], but in summary, you might think of writing a module like this:

```lua
-- hello/mytest.lua
module(..., package.seeall)
local function test() print(123) end
function test1() test() end
function test2() test1(); test1() end
```
and use it like this:

```lua
require "hello.mytest"
hello.mytest.test2()
```
The criticisms are that this creates a global variable `hello` in all modules (which is a side-effect), and the global environment is exposed through the `hello` table, e.g. `hello.mytest.print == _G.print` (which among various things could be detrimental to sandboxing and is just plain weird).

These problems can be avoided by not using the `module` function but instead simply defining modules in the following simple way:

```lua
-- hello/mytest.lua
local M = {}

local function test() print(123) end
function M.test1() test() end
function M.test2() M.test1(); M.test1() end

return M
```
and importing modules this way:

```lua
local MT = require "hello.mytest"
MT.test2()
```

A module containing a class with constructor (in the object-oriented sense) can be packaged in a number of ways in a module. Here is one reasonably good approach.[*2]

```lua
-- file: finance/BankAccount.lua
local M = {}; M.__index = M

local function construct()
  local self = setmetatable({balance = 0}, M)
  return self
end
setmetatable(M, {__call = construct})

function M:add(value) self.balance = self.balance + 1 end

return M
```
A module defined in this way typically only contains a single class (or at least a single public one), which is the module itself.

It can be used like this:

```lua
local BankAccount = require "finance.BankAccount"
local account = BankAccount()
```
or even like this:

```lua
local new = require
local account = new "finance.BankAccount" ()
```
The above followed somewhat the Java convention of the package "finance" being in all lowercase, while the class `BankAccount` being in mixed (Camel) case and objects being lower-case. Notice the advantage. The classes are easy to spot and differentiate from instantiations of classes (i.e. objects), which are lower-case. If you spot something like `BankAccount:add(1)`, it is almost certainly an error since `:` is a method call on an object, but you'll notice that `BankAccount` is obviously a class due to the case convention.

The above is not the only approach used. You will see many other styles:

```lua
account = finance.newBankAccount()
account = finance.create_bank_account()
account = finance.bankaccount.create()
account = finance.BankAccount.new()
```
It can be argued that variety, without good justification, is not a good thing.

[LuaModuleFunctionCritiqued]: http://lua-users.org/wiki/LuaModuleFunctionCritiqued


# Commenting
Use a space after `--`.

```lua
return nil  -- not found    (suggested)
return nil  --not found     (discouraged)
```
(The above follows luarefman, PiL, luagems minus chap 21, BLP, and Kepler/[LuaRocks].)

There is no standard convention for commenting.

Docstrings can be simulated (see DecoratorsAndDocstrings). POD format has also been advocated (see [LuaSearch]). There is also [[LuaDoc]].

Kepler sometimes uses this doxygen/Javadoc-like style:

```lua
-- taken from cgilua/src/cgilua/session.lua
-------------------------------------
-- Deletes a session.
-- @param id Session identification.
-------------------------------------
function delete (id)
  assert (check_id (id))
  remove (filename (id))
end
```

[LuaRocks]: http://lua-users.org/wiki/LuaRocks
[DecoratorsAndDocstrings]: http://lua-users.org/wiki/DecoratorsAndDocstrings
[LuaSearch]: http://lua-users.org/wiki/LuaSearch
[LuaDoc]: http://luadoc.luaforge.net/

### End Terminator
Because `end` is a terminator for many different constructs, it can help the reader (especially in a large block) if a comment is used to clarify which construct is being terminated: [*3]

```lua
for i,v in ipairs(t) do
  if type(v) == "string" then
    ...lots of code here...
  end -- if string
end -- for each t
```


## Software Licensing
The choice of software license for your Lua code depends on your goals and what type of code it is (e.g. module or application). Licensing choice is particularly significant for modules, which are often distributed with other modules and applications.

There are advantages to licensing Lua modules, or at least those intended for the general Lua community, under the same terms as Lua[[2]] itself, which as of version 5.0 is the MIT license[[3]]. Not only is the MIT license a very simple to understand and unrestrictive license (in fact, no more restrictive than Lua itself), but consistency in licensing between modules and with Lua allows simplified distribution of bundles of modules and Lua together, such as for distributions and embedded versions of Lua. (This basic approach has worked very well in the past for the Perl language, which has thousands of modules most entirely under an MIT-like license called the Artistic License, under which Perl is also distributed. Perl modules often indicate their licensing simply with the statement "Licensed under the same terms as Perl itself.") Those advantages of using an MIT license are reduced, though not eliminated, if your Lua module acts as a binding to some C library released under some very different license such (L)GPL or a closed source one since the latter code impose stronger restrictions on distribution anyway. Avoid, whenever possible, writing your own license or adding additional clauses but rather consider strongly the words of warning about consistency at the top of this document since inconsistency is a detriment to reusability, and reusability is a main advantage of modules.

[2]: http://www.lua.org/license.html
[3]: http://en.wikipedia.org/wiki/MIT_license


## Lua Idioms
To test whether a variable is not `nil` in a conditional, it is terser to just write the variable name rather than explicitly compare against `nil`. Lua treats `nil` and `false` as `false` (and all other values as `true`) in a conditional:

```lua
local line = io.read()
if line then  -- instead of line ~= nil
  ...
end
...
if not line then  -- instead of line == nil
  ...
end
```
However, if the variable tested can ever contain `false` as well, then you will need to be explicit if the two conditions must be differentiated: `line == nil` vs. `line == false`.

`and` and `or` may be used for terser code:

```lua
local function test(x)
  x = x or "idunno"
    -- rather than if x == false or x == nil then x = "idunno" end
  print(x == "yes" and "YES!" or x)
    -- rather than if x == "yes" then print("YES!") else print(x) end
end
```

Clone a *small* table `t` (warning: this has a system dependent limit on table size; it was just over 2000 on one system):

```lua
u = {unpack(t)}
```

Determine if a table `t` is empty (including non-integer keys, which `#t` ignores):

```lua
if next(t) == nil then ...
```

To append to an array, it can be terser and more efficient to do `t[#t+1] = 1` rather than `table.insert(t, 1)`.


## Design Patterns
Lua is small language with a small number of simple building blocks that can be combined in a vast number of powerful ways. With this freedom comes the need for self-discipline in the form of design patterns. Often there is an idiomatic way or design pattern to achieving a certain effect in Lua that can be reused frequently (e.g. [ObjectOrientationTutorial] or ReadOnlyTables). See the LuaTutorial and SampleCode for common solutions to such problems.

[ObjectOrientationTutorial]: http://lua-users.org/wiki/ObjectOrientationTutorial
[ReadOnlyTables]: http://lua-users.org/wiki/ReadOnlyTables
