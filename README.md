
local Versionxx = "2.5.6 "
print("Version: "..Versionxx)
---------------


local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "AUTOFARM " .. Versionxx,
    SubTitle = "by PeetJKA",
    TabWidth = 160,
    Size = UDim2.fromOffset(487.2, 404.8),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

local Tabs = {
    Farming = Window:AddTab({ Title = "Farming", Icon = "skull" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })

}

local Cache = { DevConfig = {} };

Cache.DevConfig["ListOfMonter"] = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://raw.githubusercontent.com/KangKung02/just-bin/main/OPL_MT.lua"));


local function IsSpawned()
    return not game.Players.LocalPlayer.PlayerGui.Load.Frame.Visible;
end




local Options = Fluent.Options

do

     local Section = Tabs.Farming:AddSection("Farming Only With Gun")
    local Input = Tabs.Farming:AddInput("InputDistanceMonter", {
        Title = "Distance",
        Default = "8",
        Placeholder = "Placeholder",
        Numeric = true, -- Only allows numbers
        Finished = false, -- Only calls callback when you press enter
    })
    local Dropdown = Tabs.Farming:AddDropdown("DropdownXYMonter", {
        Title = "Type Position",
        Values = {"X", "Y"},
        Multi = false,
        Default = 1
    })
    local Toggle = Tabs.Farming:AddToggle("MyToggleOneHitMonter", {Title = "One Hit", Description = "Only OPL: Anarchy", Default = false })
    local Toggle = Tabs.Farming:AddToggle("MyToggleBypassOneHitMonter", {Title = "Bypass One Hit", Description = "Only OPL: Anarchy", Default = false })

    local DropdownWToolMonter = Tabs.Farming:AddDropdown("DropdownWTooMonterl", {
        Title = "Select Tools",
        Values = Weaponlist,
        Multi = false,
        Default = 1
    })
    local function updateDropdownWToolMonterOptions()
        local WeaponlistNew = {}
        for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
            table.insert(WeaponlistNew,v.Name)
        end
        DropdownWToolMonter.Values = WeaponlistNew
        DropdownWToolMonter:SetValue(Options.DropdownWTooMonterl.Value)
    end
    Tabs.Farming:AddButton({
        Title = "Refresh",
        Description = "Refresh Tool list",
        Callback = function()
            updateDropdownWToolMonterOptions()
        end
    })
    local Section = Tabs.Farming:AddSection("Farming Monter")
    local MultiDropdown = Tabs.Farming:AddDropdown("MultiDropdownBringMonter", {
        Title = "Select Bring",
        Description = "You can select multiple values.",
        Values = Cache.DevConfig["ListOfMonter"],
        Multi = true,
        Default = {},
    })
    local MultiTeleportMonterDropdown = Tabs.Farming:AddDropdown("MultiDropdownTeleportMonter", {
        Title = "Select Teleport",
        Description = "You can select multiple values.",
        Values = Cache.DevConfig["ListOfMonter"],
        Multi = true,
        Default = {},
    })
    local Toggle = Tabs.Farming:AddToggle("MyToggleBringMonter", {Title = "Bring Monter", Default = false })
    local ToggleTeleportgMonter = Tabs.Farming:AddToggle("MyToggleTeleportgMonter", {Title = "Teleport Monter", Default = false })

    spawn(function()
        while wait() do
            pcall(function()
                if not Options.MyToggleBringMonter.Value or not IsSpawned() then return end;
                for _, Value in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                    if Options.MultiDropdownBringMonter.Value[Value.Name] then
                        Value.HumanoidRootPart.CanCollide = false;
                        Value.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 0) + game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame.LookVector * Options.InputDistanceMonter.Value;
                    end
                end
            end)
        end
    end);
    spawn(function()
        local function Attack(Obj)
            if not Options.DropdownWTooMonterl.Value or Options.DropdownWTooMonterl.Value == "" then return end;
            local ListTools = {"Slingshot", "Stars", "Crossbow", "Flintlock", "Cannon Ball"};
            local Tool;
            repeat
                game.Players.LocalPlayer.Character.Humanoid:UnequipTools();
                for _, v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
                    if not Tool and v.ClassName == "Tool" and string.match(string.lower(v.Name), string.lower(Options.DropdownWTooMonterl.Value)) then
                        v.Parent = game.Players.LocalPlayer.Character;
                        Tool = v;
                        break;
                    end
                end
                if not Options.DropdownWTooMonterl.Value then return end;
                wait();
            until Tool;
            local TimeOut = 0;
            local OldKill = game:GetService("Workspace").UserData["User_" .. game.Players.LocalPlayer.UserId].Data.Kills.Value;
            repeat
                pcall(function()
                    if table.find(ListTools, Tool.Name) then
                        Tool.RemoteEvent:FireServer(CFrame.new(Obj.HumanoidRootPart.Position), Obj.HumanoidRootPart);
                    else
                        Tool:Activate();
                    end
                end);
                TimeOut += 1;
                wait(0.1);
            until OldKill < game:GetService("Workspace").UserData["User_" .. game.Players.LocalPlayer.UserId].Data.Kills.Value or not Options.MyToggleTeleportgMonter.Value or TimeOut > 10;
        end
        while wait() do
            pcall(function()
                if not Options.MyToggleTeleportgMonter.Value or not IsSpawned() then return end;
                for _, Value in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                    if not Options.MyToggleTeleportgMonter.Value then return end;
                    if Options.MultiDropdownTeleportMonter.Value[Value.Name] and Value:FindFirstChild("HumanoidRootPart") and Value:FindFirstChild("Humanoid") and Value.Humanoid.Health > 0 then
                        if Options.DropdownXYMonter.Value == "X" then
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Value.HumanoidRootPart.Position + Vector3.new(0, 0, Options.InputDistanceMonter.Value), Value.HumanoidRootPart.Position);
                            if Options.MyToggleOneHitMonter.Value then Value.Humanoid.Health = 0 end;
                            Attack(Value);
                        elseif Options.DropdownXYMonter.Value == "Y" then
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(Value.HumanoidRootPart.Position + Vector3.new(0, Options.InputDistanceMonter.Value, 0), Value.HumanoidRootPart.Position);
                            if Options.MyToggleOneHitMonter.Value then Value.Humanoid.Health = 0 end;
                            Attack(Value);
                        end
                    end
                end
            end)
        end
    end);
    
    ToggleTeleportgMonter:OnChanged(function() 
        Options.MyToggleNoClip:SetValue(Options.MyToggleTeleportgMonter.Value)
    end)
    
    game:GetService("RunService").Stepped:Connect(function()
        if Options.MyToggleNoClip.Value and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid:ChangeState(11)
        end
    end)
    
    game:GetService("RunService").RenderStepped:Connect(function()
        if Options.MyToggleBypassOneHitMonter.Value then
            setscriptable(game.Players.LocalPlayer, "SimulationRadius", true);
            game.Players.LocalPlayer.SimulationRadius = math.huge * math.huge;
        end
    end);
    
    game.Players.LocalPlayer.SimulationRadiusChanged:Connect(function(radius)
        if Options.MyToggleBypassOneHitMonter.Value then
            radius = 9e9;
            return radius;
        end
    end);

    local Section = Tabs.Farming:AddSection("Farming Only With Fruits")
    local Toggle = Tabs.Farming:AddToggle("MyToggleLightFarm", {Title = "Light Fruit", Default = false })




    local vtc = getsenv(game.Players.LocalPlayer.Character.Powers.Light)["VTCrv"]
    local plr = game:GetService("Players").LocalPlayer
    
    if not plr.Character:FindFirstChild("Powers") or not plr.Character.Powers:FindFirstChild("Light") then
        warn("Powers.Light ไม่พบใน Character")
        return
    end
    
    
    local function attack(target)
        local humanoid = target:FindFirstChildOfClass("Humanoid")
        local humanoidRootPart = target:FindFirstChild("HumanoidRootPart")
        
        if humanoid and humanoidRootPart then
            local teleportPosition = humanoidRootPart.Position + Vector3.new(0, 20, 0)
            plr.Character:SetPrimaryPartCFrame(CFrame.new(teleportPosition))
            wait(0.1)
    
            while humanoid.Health > 0 do
                plr.Character.Powers.Light.RemoteEvent:FireServer(vtc, "LightPower8", "StartCharging", humanoidRootPart.CFrame, workspace.Cave.Stone)
                plr.Character.Powers.Light.RemoteEvent:FireServer(vtc, "LightPower8", "StopCharging", humanoidRootPart.CFrame, workspace.IslandSnowyMountains.Stone.Stone, 100)
                wait(0.1)
            end
        else
            warn("เป้าหมาย " .. target.Name .. " ไม่มี Humanoid หรือ HumanoidRootPart")
        end
    end
    
    local function attackEnemies()
        for _, enemy in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
            local humanoid = enemy:FindFirstChildOfClass("Humanoid")
            if humanoid then
                attack(enemy) 
            end
        end
    end
    
    local function attackPlayers(maxHeight)
        for _, player in pairs(game:GetService("Players"):GetPlayers()) do
            if player ~= plr and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local humanoidRootPart = player.Character.HumanoidRootPart
                if humanoidRootPart.Position.Y <= maxHeight then
                    attack(player.Character)
                end
            end
        end
    end
    
    spawn(function()
        while wait() do
            pcall(function()
                if not Options.MyToggleLightFarm.Value then return end;
                spawn(attackEnemies)
                spawn(function() attackPlayers(200000) end)
            end)
        end
    end);


    end


-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have a interface managment system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)

Fluent:Notify({
    Title = "Fluent",
    Content = "The script has been loaded.",
    Duration = 8
})

-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()

