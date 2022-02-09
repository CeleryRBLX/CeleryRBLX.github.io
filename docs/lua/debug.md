# Debug Library

!!! note "Disclaimer #1"
    Synapse claims that there's an ACE vulnerability but they won't provide any evidence of what was involved to actually execute shell-code. The possibility of such a thing happening, when you have more than usual sanitization checks, would rely on external means or another custom function that is apparently flawed. Until this _other_ function is uncovered and the ACE is actually proven not by some staged video, the actual cause of the ACE is unknown, and I will not remove my debug library

!!! note "Disclaimer #2"
    Use obfuscated scripts at your own risk, since any obfuscated script can destroy your PC at any given time, and even synapse cannot fully prevent this. Use obfuscated scripts only if they're from a TRUSTED source. I will not take responsibility for it

## Constants

### Get constants

=== "Function definition"

    ```lua
    function debug.getconstants(<function> f): table
    ```

Returns the constants used in function ``f``.

---

### Set constants

=== "Function definition"

    ```lua
    function debug.setconstants(<function> f, <table> constants): void
    ```

Sets the constants used in function ``f`` to the table ``constants``.

---

### Get constant

=== "Function definition"

    ```lua
    function debug.getconstant(<function> f, <number> index): Variant
    ```

Returns the constant at ``index`` from the function ``f``'s constants.

---

### Set constant

=== "Function definition"

    ```lua
    function debug.setconstant(<function> f, <number> index, <Variant> constant): void
    ```

Sets the constant at ``index`` from function ``f`` to ``constant``.

## Protos

### Get protos

=== "Function definition"

    ```lua
    function debug.getprotos(<function> f): table
    ```

Returns the protos used in function ``f``.

---

### Set protos

=== "Function definition"

    ```lua
    function debug.setprotos(<function> f, <table> protos): void
    ```

Sets the protos used in function ``f`` to the table ``protos``.

---

### Get proto

=== "Function definition"

    ```lua
    function debug.getproto(<function> f, <number> index): function
    ```

Returns the proto at ``index`` from the function ``f``'s protos.

---

### Set proto

=== "Function definition"

    ```lua
    function debug.setproto(<function> f, <number> index, <function> proto): void
    ```

Sets the proto at ``index`` from the function ``f``'s protos to ``proto``.

## Instructions/code

### Get code

=== "Function definition (1 arg)"

    ```lua
    function debug.getcode(<function> f): table
    ```

    Returns the instructions in function ``f``'s code.

=== "Function definition (2 args)"

    ```lua
    function debug.getcode(<function> f, <number> index): number
    ```
    Returns the instruction at ``index`` from function ``f``'s code.

---

### Set code

=== "Function definition (2 args)"

    ```lua
    function debug.setcode(<function> f, <table> code): void
    ```

    Set the instructions in function ``f`` to ``code``.

=== "Function definition (3 args)"

    ```lua
    function debug.setcode(<function> f, <number> index, <number> instruction): void
    ```

    Sets the instruction at ``index`` from function ``f``'s code to ``instruction``.

!!!warning
    Disabled unless you are in experimental mode

## Stack

### Get stack

=== "Function definition (no args)"

    ```lua
    function debug.getstack(): table
    ```

    Returns all elements in the current thread's stack.

=== "Function definition (1 arg)"

    ```lua
    function debug.getstack(<number> index): Variant
    ```

    Returns the element at ``index`` from the current function's stack.

---

### Set stack

=== "Function definition (1 argument)"

    ```lua
    function debug.setstack(<table> stack): void
    ```

    Sets the current thread's stack to ``stack``.

=== "Function definition (2 arguments)"

    ```lua
    function debug.setstack(<number> index, <Variant> value): void
    ```

    Sets the element at ``index`` from the current function's stack to ``value``.

## Upvalues

### Get upvalues

=== "Function definition"

    ```lua
    function debug.getupvalues(<function> f): table
    ```

Returns the upvalues in the function ``f``.

---

### Set upvalues

=== "Function definition"

    ```lua
    function debug.setupvalues(<function> f, <table> upvalues): void
    ```

Sets the upvalues in the function ``f`` to ``upvalues``.

---

### Get upvalue

=== "Function definition"

    ```lua
    function debug.getupvalue(<function> f, <number> index): Variant
    ```

Returns the upvalue at ``index`` from the function ``f``'s upvalues.

---

### Set upvalue

=== "Function definition"

    ```lua
    function debug.setupvalue(<function> f, <number> index, <Variant> value): void
    ```

Sets the upvalue at ``index`` from the function ``f``'s upvalues to ``value``.
