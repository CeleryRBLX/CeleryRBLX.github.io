# Miscellaneous

## Script environment

### Identify executor

=== "Function definition"

    ```lua
    function identifyexecutor(): string
    ```

=== "Example"

    ```lua
    if ({pcall(identifyexecutor)})[2] == "Celery" then
        print("Using Celery")
    end
    ```

Returns a string to identify what executor is being used. In this case, it's ``#!lua "Celery"``

---

### Get ROBLOX environment

=== "Function definition"

    ```lua
    function getrenv(): table
    ```

Returns the ROBLOX script environment.

---

### Get global environment

=== "Function definition"

    ```lua
    function getgenv(): table
    ```

Identical to ``#!lua getfenv()``.

There is no way to implement this in Celery because ROBLOX automatically sandboxes and protects each script environment for us -- so, there is no way of connecting the environments for Celery scripts, because of how it executes them.

---

### Get Lua registry

=== "Function definition"

    ```lua
    function getreg(): table
    ```

Returns a table containing all elements stored in lua registry.

---

### Get identity

=== "Function definition"

    ```lua
    function getidentity(): number
    ```

Returns the context level of execution that the script is running with, most likely ``#!lua 6``.

---

### Set identity

=== "Function definition"

    ```lua
    function setidentity(<number> identity): void
    ```

Sets the current context level to ``identity``.

<br>

## Tables

### Is read only

=== "Function definition"

    ```lua
    function isreadonly(<table> t): bool
    ```

Returns whether the table ``t`` is read only or not.

---

### Set read only

=== "Function definition"

    ```lua
    function setreadonly(<table> t, <bool> value): value
    ```

Sets whether the table ``t`` is read only or not.

---

### Make read only

=== "Function definition"

    ```lua
    function makereadonly(<table> t): table
    ```

Sets table ``t`` to read only.
Equal to doing ``#!cpp setreadonly(t, true)``.

Returns table ``t``.

---

### Make writeable

=== "Function definition"

    ```lua
    function makewriteable(<table> t): void
    ```

Makes the table ``t`` write-able (not read only).
Equal to doing ``setreadonly(t, false)``.

<br>

## Functions

### Is C closure

=== "Function definition"

    ```lua
    function iscclosure(<function> f): bool
    ```

Returns ``#!cpp true`` is ``f`` is a C closure and not a Lua function.

---

### New C closure

=== "Function definition"

    ```lua
    function newcclosure(<function> f): function
    ```

Returns a C closure function which invokes the Lua function ``f``

---

### Hook function

=== "Function definition"

    ```lua
    function hookfunction(<function> a, <function> b): bool
    ```

Swaps the internal function of ``a`` with ``b``, so every time ``a`` is called it will call ``b`` instead.

Returns the old ``a`` function.

---

### Get namecall method

=== "Function definition"

    ```lua
    function getnamecallmethod(): string
    ```

Returns the current namecall method used by ``#!asm __namecall``, as a string.

---

### Set namecall method

=== "Function definition"

    ```lua
    function setnamecallmethod(<string> method): string
    ```

Sets the current namecall method used by ``#!asm __namecall`` to ``method``.

---

### Get raw metatable

=== "Function definition"

    ```lua
    function getrawmetatable(<table> t): table
    ```

Returns the raw metatable of ``t`` -- basically just bypasses the ``#!asm __metatable`` check for it.

<br>

## Instances

### Scripts

#### Get script bytecode

=== "Function definition"

    ```lua
    function getscriptbytecode(<Instance<LocalScript>> localscript): string
    ```

Returns the LuaU bytecode contained in the ``localscript`` script.

!!! warning

    ModuleScripts are not supported yet

---

#### Dissasemble bytecode

=== "Function definition"

    ```lua
    function disassemble(<string> bytecode): string
    ```

Translates a script's ``bytecode`` into a readable, disassembled output.

---

#### Decompile script

=== "Function definition"

    ```lua
    function decompile(<Instance<LocalScript>> localscript): string
    ```

Decompiles a ``localscript``'s bytecode into readable lua, as close to the original script as possible.

---

### Fire click detector

=== "Function definition"

    ```lua
    function fireclickdetector(<Instance<ClickDetector>> clickdetector): void
    ```

Fires the ``clickdetector`` instance -- calling any signals connected to it.

---

### Fire touch interest

=== "Function definition"

    ```lua
    function firetouchinterest(<Instance> part, <BasePart> toTouch, <number> mode): void
    ```

Fires ``toTouch``'s TouchInterest.

!!! note

    The ``toTouch`` argument must have a child with class ``TouchTransmitter`` in order for this function to work.

---

### Get hidden properties

=== "Function definition"

    ```lua
    function gethiddenproperties(<Instance> instance): table
    ```

Returns hidden lua properties associated with ``instance``.

---

### Get hidden property

=== "Function definition"

    ```lua
    function gethiddenproperty(Instance instance, String property): Variant
    ```

Gets the hidden property ``property`` from ``instance``.

---

### Set hidden property

=== "Function definition"

    ```lua
    function sethiddenproperty(<Instance> instance, <string> property, <Variant> value): void
    ```

Sets the hidden property ``property`` of instance to ``value``.

---

## Client

### Get simulation radius

=== "Function definition"

    ```lua
    function getsimulationradius(): number
    ```

Returns your client's simulation radius.

---

### Set simulation radius

=== "Function definition"

    ```lua
    function setsimulationradius(<number> value): void
    ```

Sets your client's simulation radius to ``value``.
