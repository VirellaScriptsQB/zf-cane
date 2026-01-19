zf-cane - README
================

Resource Name
-------------
zf-cane

Description
-----------
Usable cane item for ox_inventory (toggle equip).
Attaches a walking cane prop to the player and (optionally) applies an injured walk clipset.
Configuration is shared for both client + server via shared/config.lua.

Requirements
------------
- FiveM (FXServer)
- ox_lib
- ox_inventory
- (Optional) zf-notify (only if you enable it in the config)

File/Folder Structure
---------------------
Make sure your resource looks exactly like this:

zf-cane/
  fxmanifest.lua
  shared/
    config.lua
  client/
    client.lua
  server/
    server.lua

fxmanifest.lua
--------------
Use this manifest (paths + escrow_ignore included):

fx_version 'cerulean'
game 'gta5'

name 'zf-cane'
author 'zf'
description 'Usable cane item for ox_inventory (toggle equip)'
version '1.0.0'

lua54 'yes'

dependencies {
    'ox_lib',
    'ox_inventory'
}

shared_scripts {
    '@ox_lib/init.lua',
    'shared/config.lua',
}

client_scripts {
    'client/client.lua',
}

server_scripts {
    'server/server.lua',
}

escrow_ignore {
    'shared/config.lua',
    'client/**',
    'server/**',
    'fxmanifest.lua'
}

Installation
------------
1) Drag the folder "zf-cane" into your server resources directory:
   resources/[standalone]/zf-cane

2) Ensure dependencies are installed and started before this resource:
   - ox_lib
   - ox_inventory

3) Add to your server.cfg (in the correct order):
   ensure ox_lib
   ensure ox_inventory
   ensure zf-cane

4) Add the item to ox_inventory (items file).
   Add something like:

   ['cane'] = {
       label = 'Cane',
       weight = 500,
       stack = false,
       close = true,
       description = 'A walking cane.',
   },

   Notes:
   - The item name MUST match Config.Cane.itemName in shared/config.lua
   - Default is: itemName = 'cane'

5) (Optional) If you use a different item name:
   - Change Config.Cane.itemName in shared/config.lua
   - Or rename the item in ox_inventory to "cane"

Configuration (shared/config.lua)
---------------------------------
Your config is located here:
shared/config.lua

Current Config Contents:
------------------------
-- shared/config.lua

Config = Config or {}

---Cane settings shared by client + server.
Config.Cane = {
    itemName = 'cane',

    -- Prop
    model = `prop_cs_walking_stick`,
    bone = 57005,
    attach = {
        pos = vec3(0.10, 0.02, -0.02),
        rot = vec3(10.0, 90.0, 180.0),
    },

    -- Movement
    injuredWalk = true,
    injuredClipset = 'move_m@injured',
    blend = 0.25,

    -- Restrictions
    blockInVehicle = true,
    blockWhenDead = true,

    -- Notifications
    notify = {
        enabled = true,
        resource = 'zf-notify',

        -- If your zf-notify uses a client event, set it here:
        event = 'zf-notify:client:Notify',

        -- If your zf-notify uses exports, these are tried in order:
        exportMethods = { 'Notify', 'notify' },
    },
}

Config Options Explained
------------------------
Item
- Config.Cane.itemName
  The ox_inventory item name that toggles the cane.

Prop / Attachment
- Config.Cane.model
  The GTA prop model hash/backtick value to attach as the cane.
- Config.Cane.bone
  Ped bone index used for attachment (default: 57005).
- Config.Cane.attach.pos
  Position offset of the prop.
- Config.Cane.attach.rot
  Rotation offset of the prop.

Movement / Clipset
- Config.Cane.injuredWalk
  If true, applies the injured walk clipset while cane is equipped.
- Config.Cane.injuredClipset
  Clipset name used when injuredWalk is enabled.
- Config.Cane.blend
  Blend value when applying clipset.

Restrictions
- Config.Cane.blockInVehicle
  If true, blocks equipping while in a vehicle.
- Config.Cane.blockWhenDead
  If true, blocks equipping while dead.

Notifications
- Config.Cane.notify.enabled
  Enable/disable notifications.
- Config.Cane.notify.resource
  Your notify resource name (used for checking/organization).
- Config.Cane.notify.event
  Client event to trigger notifications (if your notify uses events).
- Config.Cane.notify.exportMethods
  Export function names to try (if your notify uses exports).

How It Works
------------
- Player uses the configured ox_inventory item (default: "cane").
- The client toggles the cane:
  - Spawns and attaches the prop to the ped bone
  - Optionally applies injured movement clipset
- Using the item again removes the prop and resets movement.

Troubleshooting
---------------
1) "Nothing happens when I use the item"
- Confirm the item exists in ox_inventory and name matches Config.Cane.itemName
- Confirm the resource is started: ensure zf-cane
- Check F8 console for errors

2) "Prop doesn't show / attaches wrong"
- Try adjusting Config.Cane.attach.pos and Config.Cane.attach.rot
- Confirm the prop model exists:
  prop_cs_walking_stick

3) "Walk style not changing"
- Set Config.Cane.injuredWalk = true
- Confirm clipset name is valid: move_m@injured
- Check for other scripts forcing walkstyles

4) "Notifications not showing"
- Set Config.Cane.notify.enabled = true
- Ensure your notify resource is started
- Verify event name:
  zf-notify:client:Notify
- Or confirm your notify exports match:
  Notify / notify

Security / Escrow
-----------------
This resource includes escrow_ignore entries for:
- shared/config.lua
- client/** (all client files)
- server/** (all server files)
- fxmanifest.lua

So you can edit config/client/server without escrow blocking.

Credits
-------
Author: zf

Version
-------
1.0.0
