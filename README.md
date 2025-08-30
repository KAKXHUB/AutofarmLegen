


local Versionxx = "2.5.4 "
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

Cache.DevConfig["ListOfBox"] = {"Common Box", "Uncommon Box", "Rare Box", "Ultra Rare Box"};
Cache.DevConfig["ListOfDrink"] = {"Cider+", "Lemonade+", "Juice+", "Smoothie+"};
Cache.DevConfig["ListOfDrinkFormMixer"] = {"Cider", "Lemonade", "Juice", "Smoothie", "Milk", "Golden Apple"};
Cache.DevConfig["ListOfDevillFruit"] = {"Magma", "Gas", "Sand", "Dark", "Chilly", "Rumble", "Snow", "Light", "String", "Flare", "Love", "Phoenix", "Quake", "Candy", "Bomb", "Venom", "Rumble1", "Gravity", "Plasma"};
Cache.DevConfig["ListOfTypeSkillTaget"] = {"Mouse", "Yourself", "Monsters", "Players"};
Cache.DevConfig["FindFruitArgumet"] = loadstring(game:HttpGet"https://raw.githubusercontent.com/KangKung02/The-Last-Krypton/master/Back-End/src/public/api/UWU.lua")();
Cache.DevConfig["ListOfDveilFruit"] = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://raw.githubusercontent.com/KangKung02/just-bin/main/OPL_ALF.json"));
Cache.DevConfig["ListOfMonter"] = game:GetService("HttpService"):JSONDecode(game:HttpGet("https://raw.githubusercontent.com/KangKung02/just-bin/main/OPL_MT.lua"));

local Allset = { DevSetting = {} };
Allset.DevSetting["ListTeleportIsland"] = {
    ["Crab island"] = CFrame.new(-6.00764561, 215.999954, -304.312866);
    ["Vokun"] = CFrame.new(4564.3999, 526, 5712.5);
    ["Purple"] = CFrame.new(-5283, 534.199829, -7761.5);
    ["Rocky"] = CFrame.new(-32.2999992, 228.999954, 2156.6001);
    ["Boss"] = CFrame.new(4809.43555, 590.268005, -7000.63379);
    ["Sand Castle"] = CFrame.new(921, 223.999954, -3275);
    ["Boss sniper"] = CFrame.new(-1075.32813, 360.999908, 1676.62024);
    ["Snow"] = CFrame.new(-1897, 224.999954, 3298);
    ["Drink"] = CFrame.new(1505, 260.38623, 2170.8999);
    ["Tree"] = CFrame.new(1166, 217.079941, 3287);
    ["Big Snow"] = CFrame.new(6616, 417.998901, -1497.30005);
    ["Cave"] = CFrame.new(-79.5663376, 215.999985, -890.889465);
    ["Forest"] = CFrame.new(-6006.75977, 402, 7.19999981);
    ["Pyramid"] = CFrame.new(119, 310, 4945.8999);
    ["Sam"] = CFrame.new(-1281.68347, 218, -1351.09534);
    ["Fish Man"] = CFrame.new(-1683.59998, 216, -333.299988);
    ["Mountain"] = CFrame.new(2053.5, 492, -637);
    ["Boss Aura"] = CFrame.new(-1575.63281, 219.278564, 9939.50684);
}
Allset.DevSetting["ListTeleportIslandIndex"] = {"Crab island", "Vokun", "Purple", "Rocky", "Boss", "Sand Castle", "Boss sniper", "Snow", "Drink", "Tree", "Big Snow", "Cave", "Forest", "Pyramid", "Sam", "Fish Man", "Mountain", "Boss Aura" };


local function IsSpawned()
    return not game.Players.LocalPlayer.PlayerGui.Load.Frame.Visible;
end
local function InstanceToName(t)
    local Content = {};
    for _, Value in pairs(t) do
        table.insert(Content, Value.Name);
    end
    return Content;
end
local CheckPath = function(Path, ...)
    local Args = {...};
    for _, v in pairs(Args) do
        if Path:FindFirstChild(v) then
            Path = Path[v]
        else
            return false;
        end
    end
    return true;
end



local Options = Fluent.Options

do

     local Section = Tabs.Farming:AddSection("Farming Only With Gun")


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
