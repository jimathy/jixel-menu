# jixel-menu
Context Menu System for the QBCore Framework
This is a modified version of QBCore's QB-Menu

Original made by QBCore, edited by Jimathy

Edited again by Jimathy666 and Taylor - https://discord.gg/xKgQZ6wZvS

---
### Changes from qb-menu:
- New theme
- Support for ox_lib style variables
    - (eg . `onSelect`, `onExit`, `title`, `description`)
- Support for progress bars in the buttons
    - `progress = 80`
- Search bar
    - Type anything and it will show only buttons with that string
---
### DISCLAIMER:
### THE SCRIPT NEEDS TO BE STARTED BEFORE `[qb]`
### It's Recommended to place this script the `[standalone]` folder

### In version 1.1, this script will now try to claim the `qb-menu` export name so no rename is needed.
---
### Screenshot of the Menu 
<img src="https://im4.ezgif.com/tmp/ezgif-4-1226ab16f4.gif"></img>
![Example Menu](https://media.discordapp.net/attachments/921119357336191007/1167201993933209690/image.png?ex=654d4490&is=653acf90&hm=22cc629064bace97ac70a1098e8fd05647f4fd6215b56a7ea34e3735923adb5b&=&width=355&height=449)
#

# ChangeLog

### v1.1
- Added support for `onSelect`
    - This can be used instead of `params`
- Support for ox_lib style `title` and `description`
- Progress Bar support added
    - In each button you can now add `progress` and optionally `colourScheme`
    - `colorScheme` adheres to https://v6.mantine.dev/theming/colors/#default-colors
    - eg `"red.5"` `"green.9"`
- Script now can be remain named `jixel-menu`
    - The script will attempt to claim the `qb-menu` export name
    - You will need to place this into `[standalone]` and then restart your server

### v1.0
- Added Search bar, using this will show only the buttons with that `"string"`

---

# EXAMPLE MENU

```lua
RegisterCommand('jixelmenutest', function()
    exports['qb-menu']:openMenu({
        {
            header = 'QBCore Test Menu',
            icon = 'fas fa-code',
            isMenuHeader = true, -- Set to true to make a nonclickable title
        },
        {
            header = 'First Button',
            txt = 'Print a message!',
            icon = 'fas fa-code-merge',
            onSelect = function()
                TriggerEvent("jixel-menu:client:testButton", { message = 'This was called by clicking a button' })
            end,
        },
        {
            header = 'Second Button',
            txt = 'Open a secondary menu!',
            arrow = true, -- adding this will force an "arrow" icon to appear to imply it leads to another menu
            -- disabled = false, -- optional, non-clickable and grey scale
            -- hidden = true, -- optional, hides the button completely
            progress = 76,
            params = {
                -- isServer = false, -- optional, specify if its a server event or not
                event = 'jixel-menu:client:subMenu',
                args = {
                    number = 2,
                }
            }
        },
        {
            title = 'Progress Button',
            description = 'Example of a Progress Bar!',
            isMenuHeader = true,
            -- disabled = false, -- optional, non-clickable and grey scale
            -- hidden = true, -- optional, hides the button completely
            progress = 76,
            colorScheme = "red.6",
            params = {
                -- isServer = false, -- optional, specify if its a server event or not
                event = 'jixel-menu:client:subMenu',
                args = {
                    number = 2,
                }
            }
        },
    }, { canClose = true, onExit = function() print("onExit triggered") end, onBack = function() print("onBack triggered") end, })
end)

RegisterNetEvent('jixel-menu:client:subMenu', function(data)
    local number = data.number
    exports['qb-menu']:openMenu({
        {
            header = 'Return to main menu',
            icon = 'fas fa-backward',
            onSelect = function()
                TriggerEvent("jixel-menu:client:mainMenu", {})
            end,
        },
        {
            header = 'Sub-menu button',
            txt = 'Print a message!',
            icon = 'fas fa-code-merge',
            onSelect = function()
                TriggerEvent("jixel-menu:client:testButton", { message = 'This was called by clicking a button' })
            end,
        }
    })
end)

RegisterNetEvent('jixel-menu:client:mainMenu', function() ExecuteCommand('jixelmenutest') end)

RegisterNetEvent('jixel-menu:client:testButton', function(data) print(data.message) end)
```
## For loop menu
```lua
local staff = { -- our table we will loop through to get button values
    ['Jimathy'] = 'Script Monkey',
    ['Taylor'] = 'Script Goblin',
}

RegisterCommand('jixelmenutable', function()
    local staffList = {}
    staffList[#staffList + 1] = { -- create non-clickable header button
        isMenuHeader = true,
        header = 'Jixel Test Loop Menu',
        icon = 'fas fa-infinity'
    }
    for k, v in pairs(staff) do -- loop through our table
        staffList[#staffList + 1] = { -- insert data from our loop into the menu
            header = k,
            txt = 'Yeah they are definitely '..v,
            icon = 'fas fa-face-grin-tears',
            onSelect = function()
                TriggerEvent("jixel-menu:client:notify", { name = k, label = v })
            end,
        }
    end
    exports['qb-menu']:openMenu(staffList) -- open our menu
end)

RegisterNetEvent('jixel-menu:client:notify', function(data)
    print('My favorite Jixelpatterns member is the '..data.label..' '..data.name)
end)
```

# License

    QBCore Framework
    Copyright (C) 2021 Joshua Eger

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>
