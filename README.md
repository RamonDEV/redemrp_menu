# redemrp_menu
 A Menu Base for RedEM:RP
This script allows you create menu like RDR2.

## 1. Installation
- Be sure you have RedEM and RedEM:RP Installed
redem_roleplay FrameWork ```https://github.com/RedEM-RP/redem_roleplay```
- Clone redemrp_menu into [redemrp] folder
- add ```ensure redemrp_menu``` after ```ensure redem_roleplay```

## 2.Usage
Add this on top your client side file
```
----------------------------REDEMRP_MENU----------------------------
MenuData = {}
TriggerEvent("rdr_menu:getData",function(call)
    MenuData = call
end)
----------------------------REDEMRP_MENU----------------------------

```
Example:
```
    MenuData.CloseAll()
    local elements = {
        {label = "Foods", value = 'foods', desc = "Cooking"},
        {label = "Drinks", value = 'drinks', desc = "Drinks"},
        {label = "Inventory", value = 'inven', desc = "Job Inventory"},
    }

    if jobgrade > 4 then
        table.insert(elements, {label = "Hire Employees", value = "hiremployees", desc = "Hire Employees"})
        table.insert(elements, {label = "Fire an employee", value = "firemployees", desc = "Fire Employees"})
    end

    MenuData.Open(
    'default', GetCurrentResourceName(), 'Saloon_Menu',
    {
        title    = 'Saloon',

        subtext    = 'Select a Category',

        align    = 'top-right',

        elements = elements,
    },
    function(data, menu)
        if(data.current.value == 'foods') then
            MenuCook()
        elseif(data.current.value == 'drinks') then
            MenuDrinks()
        elseif(data.current.value == 'inven') then
            menu.close()
            TriggerServerEvent("vesgoboy_craft:storage", jobcraft, 1500) -- JOB NAME AND STASH WEIGHT
        elseif(data.current.value == 'hiremployees') then
            MenuData.CloseAll()
            AddTextEntry("FMMC_MPM_TYP5", "Type Citizen ID Of Employe (must be online)")
            DisplayOnscreenKeyboard(3, "FMMC_MPM_TYP5", "", "", "", "", "", 30)
            while (UpdateOnscreenKeyboard() == 0) do
                DisableAllControlActions(0)
                Citizen.Wait(0)
            end
            if (GetOnscreenKeyboardResult()) then
                kbdRes = GetOnscreenKeyboardResult()
            else
                return
            end
            if #(kbdRes) >= 1 then
                TriggerServerEvent("vesgoboy_craft:server:HireMember", kbdRes)
            else
                RedEM.Functions.NotifyLeft("Invalid entry!", "Add a Valid Document.", "menu_textures", "menu_icon_alert", 4000)
            end
        elseif data.current.value == 'firemployees' then
            MenuData.CloseAll()
            TriggerServerEvent("vesgoboy_craft:server:GetFireList")
        end
    end,
    function(data, menu)
        menu.close()
    end)
```

## 3.Credits
- https://github.com/ktos93
- https://github.com/ESX-Org
