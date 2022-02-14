# RakNet

Often abreviated as ``rnet``, RakNet is the [protocol](http://www.jenkinssoftware.com/raknet/manual/introduction.html) Roblox uses for sending/receiving multiplayer packets.

## Packets type

=== "Opcodes <-> Name relation"
    You can take a look at Arissath's roblox-dissector/Sala's [RakNet Layer](https://github.com/Arisstath/roblox-dissector/blob/master/peer/RakNetLayer.go#L74)

=== "Table representation"
    | Opcode   | Type                                                                   |
    | -------- | ---------------------------------------------------------------------- |
    | ``0x00`` | :material-lan-pending: ``ID_CONNECTED_PING``                           |
    | ``0x01`` | :material-lan-pending: ``ID_UNCONNECTED_PING``                         |
    | ``0x03`` | :material-lan-check: ``ID_CONNECTED_PONG``                             |
    | ``0x04`` | :material-lan-disconnect: ``ID_DETECT_LOST_CONNECTIONS``               |
    | ``0x05`` | :material-lan-pending: ``ID_OPEN_CONNECTION_REQUEST_1``                |
    | ``0x06`` | :material-lan-check: ``ID_OPEN_CONNECTION_REPLY_1``                    |
    | ``0x07`` | :material-lan-pending: ``ID_OPEN_CONNECTION_REQUEST_2``                |
    | ``0x08`` | :material-lan-check: ``ID_OPEN_CONNECTION_REPLY_2``                    |
    | ``0x09`` | :material-lan-pending: ``ID_CONNECTION_REQUEST``                       |
    | ``0x10`` | :fontawesome-solid-handshake: ``ID_CONNECTION_REQUEST_ACCEPTED``       |
    | ``0x11`` | :fontawesome-solid-handshake-slash: ``ID_CONNECTION_ATTEMPT_FAILED``   |
    | ``0x13`` | :material-lan-connect: ``ID_NEW_INCOMING_CONNECTION``                  |
    | ``0x15`` | :material-lan-disconnect: ``ID_DISCONNECTION_NOTIFICATION``            |
    | ``0x18`` | :material-form-textbox-password: ``ID_INVALID_PASSWORD``               |
    | ``0x1B`` | :material-timetable: ``ID_TIMESTAMP``                                  |
    | ``0x1C`` | :material-lan-check: ``ID_UNCONNECTED_PONG``                           |
    | ``0x81`` | :fontawesome-solid-globe-americas: ``ID_SET_GLOBALS``                  |
    | ``0x82`` | :material-book: ``ID_TEACH_DESCRIPTOR_DICTIONARIES``                   |
    | ``0x83`` | :material-data-matrix-scan: ``ID_DATA``                                |
    | ``0x84`` | :material-map-marker: ``ID_MARKER``                                    |
    | ``0x85`` | :material-mirror-variant: ``ID_PHYSICS``                               |
    | ``0x86`` | :material-mirror-variant: ``ID_TOUCHES``                               |
    | ``0x87`` | :material-chat: ``ID_CHAT_ALL``                                        |
    | ``0x88`` | :material-chat: ``ID_CHAT_TEAM``                                       |
    | ``0x89`` | :octicons-report-24: ``ID_REPORT_ABUSE``                               |
    | ``0x8A`` | :material-tag-text: ``ID_SUBMIT_TICKET``                               |
    | ``0x8B`` | :material-chat: ``ID_CHAT_GAME``                                       |
    | ``0x8C`` | :material-chat: ``ID_CHAT_PLAYER``                                     |
    | ``0x8D`` | :material-server-network: ``ID_CLUSTER``                               |
    | ``0x8E`` | :material-lan: ``ID_PROTOCOL_MISMATCH``                                |
    | ``0x8F`` | :material-selection-ellipse-arrow-inside: ``ID_PREFERRED_SPAWN_NAME``  |
    | ``0x90`` | :material-sync: ``ID_PROTOCOL_SYNC``                                   |
    | ``0x91`` | :material-sync: ``ID_SCHEMA_SYNC``                                     |
    | ``0x92`` | :octicons-verified-24: ``ID_PLACEID_VERIFICATION``                     |
    | ``0x93`` | :material-book-cog: ``ID_DICTIONARY_FORMAT``                           |
    | ``0x94`` | :octicons-shield-x-24: ``ID_HASH_MISMATCH``                            |
    | ``0x95`` | :octicons-shield-x-24: ``ID_SECURITYKEY_MISMATCH``                     |
    | ``0x96`` | :material-table-eye: ``ID_REQUEST_STATS``                              |
    | ``0x97`` | :material-floor-plan: ``ID_NEW_SCHEMA``                                |

---

## Send physics

=== "Function definition"

    ```lua
    function rnet.sendphysics(<CFrame> value): void
    ```

Tells the server to locate your character at ``value``.

!!! warn
    Rotation compression is still getting worked on.
    ``rnet.sendphysics`` only supports position from the CFRame at the moment.

---

## Start capture

=== "Function definition"

    ```lua
    function rnet.startcapture(): void
    ```

Displays outgoing packets in Celery's debug console.

---

## Stop capture

=== "Function definition"

    ```lua
    function rnet.stopcapture(): void
    ```

Stops outgoing packets from being displayed in Celery's debug console.

---

## Capture signal

=== "Signal definition"

    ```lua
    Signal rnet.Capture
    ```

=== "Example"

    ```lua
    local packetViewer = rnet.Capture:Connect(function(packet)
        local str = "";
        for _,v in pairs(packet.data) do
            str = str .. string.format("%02X ", v);
        end
        print("Sending packet... ID: " .. string.format("%02X", packet.id) .. ". Full packet:");
        print(str);
        print("\n");
    end)

    wait(30);
    packetViewer:disconnect();
    ```

This event allows you to view/log packets yourself, to display them however you want.

---

## Set filter

=== "Function definition"

    ```lua
    function rnet.setfilter(<table> t): void
    ```

Sets a packet filter of packets to be ignored, or completely blocked. The first couple of bytes in the packet will be compared with the bytes in ``t``.

If a packet starts with 1B and you run ``#!lua rnet.setfilter({0x1B})`` then the packet will be blocked. Use ``#!lua rnet.setfilter({})`` to clear this filter.

<br>

---

## Send raw

=== "Function definition"

    ```lua
    function rnet.sendraw(<string|table> value): void
    ```

=== "Fencing example"

    ```lua
    toolname = "Foil"
    tool = game.Players.LocalPlayer.Backpack[toolname];

    game.Players.LocalPlayer.Character.Humanoid:UnequipTools();
    wait(.1);
    game.Players.LocalPlayer.Character.Humanoid:EquipTool(tool);

    local t = {0};
    while not (t[1] == 0x83 and t[2] == 3 and t[3] == 1) do
        t = rnet.getpacket(); -- automatically suspends/waits
    end

    local equip_packet = "";
    for _,v in pairs(t) do
        equip_packet = equip_packet .. string.format("%02X ", v);
    end
    print("Equip tool packet: ", equip_packet);
    setclipboard(equip_packet);

    game.Players.LocalPlayer.Character.Humanoid:UnequipTools();
    wait(1);
    rnet.sendraw(equip_packet);
    ```

    Here's an example which can be used in [Fencing](https://www.roblox.com/games/12109643/Fencing), which gets the packet for equipping your tool.
    It then replays this packet, after the tool has been unequipped.

    If you look at this from another ROBLOX client joined in the same server, you will see that the foil is not being held properly. 
    This is because another packet is expected to make your character's arm actually hold the tool.

=== "Sword fights example"

    ```lua
    user = game.Players.LocalPlayer
    mouse = user:GetMouse();
    pressed = false;
    control = nil;

    mouse.KeyDown:Connect(function(key)
        if key:lower() == 'f' then
            pressed = true;
        end

        if key:lower() == 's' then
            control = nil;
        end

        if key:lower() == 'r' then
            for _,v in pairs(user.Character:GetChildren()) do
                if v:IsA("Tool") then
                    v.Parent = workspace;
                end
            end
        end
    end)

    mouse.KeyUp:Connect(function(key)
        if key:lower() == 'f' then
            pressed = false;
        end
    end)

    mouse.Button1Down:Connect(function()
        if pressed then
            print'TPing';
            local target = mouse.Target
            if target then
                if not target.Anchored then
                    local pos = mouse.Hit.p;
                    local force = 5;
                    for i=1,force do
                        rnet.sendposition(pos);
                        target.CFrame = user.Character.HumanoidRootPart.CFrame;
                        control = target;
                        wait(.01);
                        rnet.sendposition(user.Character.Head.Position);
                        wait(.01);
                    end
                    user.Character:MoveTo(user.Character.Head.Position)
                end
            end
        end
    end)

    while wait() do
        if control then
            control.Velocity = Vector3.new(0,40,0);
        end
    end
    ```

    Hold down ++f++ and click a part to bring it to you. Then, hold ++f++ and click a player to teleport them
    There's a weird effect which actually brings players with the object if it's a certain mass.
    You can __ONLY__ move unanchored parts, so in sword fights on the heights, that would be the gyro plates and lava spinners (click them).

Sends a packet to the ROBLOX network, either by a hex-formatted String or a Table of bytes.
