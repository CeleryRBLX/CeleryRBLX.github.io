# I/O & HTTP

## Filesystem

### Read file

=== "Function definition"

    ```lua
    function readfile(<string> path): string
    ```

Reads the content of the file at ``path`` as a string.
This works with plain-text and binary files.

---

### Write file

=== "Function definition"

    ```lua
    function writefile(<string> path, <string> content): string
    ```

Writes the string ``content`` to the file located at ``path``. Only string content is supported.

<br>

## Clipboard

### Get clipboard

=== "Function definition"

    ```lua
    function getclipboard(): string
    ```

Returns the text that is saved to the host machine's clipboard.

Typically achieved by doing ++ctrl+v++.

---

### Set clipboard

=== "Function definition"

    ```lua
    function setclipboard(<string> text): string
    ```

Stores ``text`` in the host machine's clipboard.

<br>

## Mouse/Keyboard

Self-explanatory I/O functions which are supported:

- ``#!lua function mouse1click(): void``
- ``#!lua function mouse1down(): void``
- ``#!lua function mouse1up(): void``
- ``#!lua function mouse2click(): void``
- ``#!lua function mouse2down(): void``
- ``#!lua function mouse2up(): void``
- ``#!lua function presskey(): void``
- ``#!lua function releasekey(): void``

<br>

## HTTP

### HTTP GET

=== "Function definition"

    ```lua
    function httpget(<string> url): string
    ```

Requests the content at ``url`` and returns it's body as a plain-text string.

---

### Synapse request (syn.request)

=== "Function definition"

    ```lua
    function syn.request(<table> options): table
    ```

=== "Arguments"

    | Name        | Type           | Required         | Description                                                                                                                                                                                                                         |
    | ----------- | -------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | ``Url``     | ``string``     | :material-check: | :material-link: The target URL for this request. <br> Must use ``http`` or ``https`` protocols.                                                                                                                                                          |
    | ``Method``  | ``string``     | :material-close:  | :material-web-plus: The HTTP [method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) being used by this request. <br> Most often ``GET`` or ``POST``. Defaults to ``GET``.                                                                       |
    | ``Headers`` | ``dictionary`` | :material-close: | :material-table-cog: A dictionary of headers to be used with this request. <br> Most HTTP headers are accepted here, but not all.                                                                                                                             |
    | ``Cookies`` | ``dictionary`` | :material-close: | :material-cookie-lock: A dictionary of cookies to be used with this request.                                                                                                                                                                               |
    | ``Body``    | ``string``     | :material-close: | :material-text-long: The request ``body``. <br> Can be any string or binary data. <br> __Must__ be excluded when using the ``GET``/``HEAD`` HTTP methods. <br><br> It might be necessary to specify the ``Content-Type`` header <br> when sending JSON or other formats.

=== "Response dictionary"

    | Name              | Type           | Description                                                                                                                           |
    |------------------ | -------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
    | ``Success``       | ``bool``       | :material-check: The success status of the request. <br> This is true if and only if the StatusCode lies within the range [200, 299]. |
    | ``StatusCode``    | ``number``     | :material-numeric: The HTTP response code identifying the status of the response.                                                     |
    | ``StatusMessage`` | ``string``     | :material-text: The status message that was sent back.                                                                                |
    | ``Headers``       | ``dictionary`` | :material-table-cog: A dictionary of headers that were set in this response.                                                          |
    | ``Cookies``       | ``dictionary`` | :material-cookie-lock: A dictionary of cookies that were set in this                                                                  |
    | ``Body``          | ``string``     | :material-text-long: The request body (content) received in the response.                                                             |

=== "Example"

    ```lua
    local response = syn.request(
        {
            Url = "http://httpbin.org/post",  -- This website helps debug HTTP requests
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json"  -- When sending JSON, set this!
            },
            Body = game:GetService("HttpService"):JSONEncode({hello = "world"})
        }
    )
        
    for i,v in pairs(response) do
        print(i,v)
        
        if type(v) == "table" then
            for i2,v2 in pairs(v) do
                warn(i2,v2)
            end
        end
    end
    ```

You can refer to [S^X docs](https://x.synapse.to/docs/reference/syn_lib.html#request).

!!! warning "Deprecation note"

    ``game:HttpGet`` and ``game:HttpGetAsync`` are supported for legacy reasons but it should not be used since they have been fully removed from ROBLOX for a couple of years now. 
    
    Either use Celery's ``httpget`` or ``syn.request``.
