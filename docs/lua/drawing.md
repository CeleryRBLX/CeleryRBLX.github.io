# Drawing library

## Drawing new

=== "Function definition"

    ```lua
    function Drawing.new(<string> classname): Instance
    ```

Creates a new drawing object.
``classname`` can be any of the following:

- :material-text-box: ``#!lua "text"``
- :material-minus: ``#!lua "line"``
- :material-triangle: ``#!lua "triangle"``
- :material-square: ``#!lua "square"``
- :material-circle: ``#!lua "circle"``
- :material-quadcopter: ``#!lua "quad"`` (basically four points that you can control, see at as each corner of a square or as a 4 point triangle)
