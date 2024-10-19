local OrionLib = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Orion/main/source'))()
local Window = OrionLib:MakeWindow({
    Name = "FireX Hub🔥 ┃ Pets Go GUI🎲",
    HidePremium = true,
    SaveConfig = true,
    ConfigFolder = "Swift Hub",
    IntroEnabled = true,
    IntroText = "Welcome to FireX Hub! ┃ Script Made By @FireXhub_ ON YouTube"
})

-- About Us Tab
local AboutUsTab = Window:MakeTab({
    Name = "About Us",
    Icon = "rbxassetid://17586572991",
    PremiumOnly = false
})

AboutUsTab:AddParagraph("REGARDING HUB", "• My Swift HUB in Pets Go is a script that enriches your gaming experience by providing essential features for seamless gameplay.")
AboutUsTab:AddParagraph("Support Me", "By subscribing to my YouTube Channel or joining my Discord Server. Thank you!")

local Pets = require(game:GetService("ReplicatedStorage").Library.Directory.Pets)

-- Variables for pet names
local fromPet = ""
local toPet = ""

-- Create the Pet Hunting Tab
local PetHuntingTab = Window:MakeTab({
    Name = "Pet Hunting",
    Icon = "rbxassetid://17586729056",
    PremiumOnly = false
})

PetHuntingTab:AddParagraph("How To Use", "Type the name of a pet inside the egg you're rolling in the first box, and the name of the pet you want to roll in the second box.")

PetHuntingTab:AddSection({ Name = "Pet Hunter" })

-- Textbox to enter 'fromPet'
PetHuntingTab:AddTextbox({
    Name = "Pet In Egg",
    Default = "",
    TextDisappear = true,
    Callback = function(Value)
        fromPet = Value
        print("From Pet: " .. fromPet)
    end
})

-- Textbox to enter 'toPet'
PetHuntingTab:AddTextbox({
    Name = "Pet You Want",
    Default = "",
    TextDisappear = true,
    Callback = function(Value)
        toPet = Value
        print("To Pet: " .. toPet)
    end
})

-- Button to trigger the pet hunting process
PetHuntingTab:AddButton({
    Name = "Hunt Pet!",
    Callback = function()
        if fromPet == "" or toPet == "" then
            warn("Please fill in both 'From Pet' and 'To Pet' names!")
            return
        end
        
        if Pets[fromPet] and Pets[toPet] then
            -- Clear original pet properties
            for i in pairs(Pets[fromPet]) do
                Pets[fromPet][i] = nil
            end
            -- Copy new pet properties
            for i, v in pairs(Pets[toPet]) do
                Pets[fromPet][i] = v
            end
            print("Successfully spawned", fromPet, "to", toPet)
        else
            warn("Invalid pet names! Please confirm the pet names.")
        end
    end
})

-- Trading Plaza Tab
local TradingPlazaTab = Window:MakeTab({
    Name = "Trading Plaza",
    Icon = "rbxassetid://17316268316",
    PremiumOnly = false
})

TradingPlazaTab:AddParagraph("How To Use", "Type the amount of gems you would like to bug; if it doesn't work, try 1m.")

TradingPlazaTab:AddSection({ Name = "Gems" })

TradingPlazaTab:AddTextbox({
    Name = "Amount To Bug",
    Default = "",
    TextDisappear = false,
    Callback = function(Value)
        print(Value)
    end
})

TradingPlazaTab:AddButton({
    Name = "Bug Gems!",
    Callback = function()
        local function formatNumber(number)
            if number == nil then
                return "0"
            end
            local suffixes = {"", "k", "m", "b", "t"}
            local suffixIndex = 1
            while number >= 1000 do
                number = number / 1000
                suffixIndex = suffixIndex + 1
            end
            return string.format(number % 1 == 0 and "%d%s" or "%.2f%s", number, suffixes[suffixIndex])
        end

        local GetSave = function()
            return require(game.ReplicatedStorage.Library.Client.Save).Get()
        end

        local GemAmount1 = 0
        for _, v in pairs(GetSave().Inventory.Currency) do
            if v.id == "Diamonds" then
                GemAmount1 = v._am
                break
            end
        end

        local t = GemAmount1
        local oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
            local args = {...}
            local method = getnamecallmethod()

            if method == "InvokeServer" and self.Name == "Booths_RequestPurchase" then
                t = t + 1000000
                return oldNamecall(self, ...)
            end
            return oldNamecall(self, ...)
        end)

        local function updateGem(amount)
            local playerGui = game.Players.LocalPlayer.PlayerGui.Main
            playerGui.Top.Diamonds.Amount.Text = formatNumber(amount)
            playerGui.Left["Diamonds Desktop"].Amount.Text = formatNumber(amount)
        end

        game:GetService("RunService").Heartbeat:Connect(function()
            updateGem(t)
        end)

        updateGem(GemAmount1)
    end    
})

-- Create the Pet Spawner Tab
local PetSpawnerTab = Window:MakeTab({
    Name = "Pet Spawner",
    Icon = "rbxassetid://17586693431",
    PremiumOnly = false
})

PetSpawnerTab:AddParagraph("How To Use", "Type the name of a pet currently in your inventory in the 'from pet' box and the pet you would like to spawn in the 'to pet' box.")

PetSpawnerTab:AddSection({ Name = "Pet Spawner" })

-- Textbox to enter 'fromPet'
PetSpawnerTab:AddTextbox({
    Name = "From Pet",
    Default = "",
    TextDisappear = false,
    Callback = function(Value)
        fromPet = Value
        print("From Pet: " .. fromPet)
    end
})

-- Textbox to enter 'toPet'
PetSpawnerTab:AddTextbox({
    Name = "To Pet",
    Default = "",
    TextDisappear = false,
    Callback = function(Value)
        toPet = Value
        print("To Pet: " .. toPet)
    end
})

-- Button to trigger the pet spawning process
PetSpawnerTab:AddButton({
    Name = "Spawn Pet!",
    Callback = function()
        if fromPet == "" or toPet == "" then
            warn("Please fill in both 'From Pet' and 'To Pet' names!")
            return
        end
        
        if Pets[fromPet] and Pets[toPet] then
            for i in pairs(Pets[fromPet]) do
                Pets[fromPet][i] = nil
            end
            for i, v in pairs(Pets[toPet]) do
                Pets[fromPet][i] = v
            end
            print("Successfully spawned", fromPet, "to", toPet)
        else
            warn("Invalid pet names! Please confirm the pet names.")
        end
    end
})

-- Gem Spawning Tab
local GemSpawnTab = Window:MakeTab({
    Name = "Gem Spawning",
    Icon = "rbxassetid://17586779012",
    PremiumOnly = false
})

GemSpawnTab:AddParagraph("How To Use", "Type the amount of gems you would like to spawn in the box then click spawn once.")

local spawnAmount = "0"

GemSpawnTab:AddTextbox({
    Name = "Amount to spawn",
    Default = "",
    TextDisappear = true,
    Callback = function(Value)
        spawnAmount = Value
    end	  
})

GemSpawnTab:AddButton({
    Name = "Spawn!",
    Callback = function()
        game.Players.LocalPlayer.PlayerGui.Main.Top.Diamonds.Amount.Text = spawnAmount
    end    
})

-- Coin Spawning Tab
local CoinSpawnTab = Window:MakeTab({
    Name = "Coin Spawning",
    Icon = "rbxassetid://17586674425",
    PremiumOnly = false
})

CoinSpawnTab:AddParagraph("How To Use", "Type the amount of coins you would like to spawn and click spawn once.")

CoinSpawnTab:AddTextbox({
    Name = "Amount to spawn",
    Default = "",
    TextDisappear = true,
    Callback = function(Value)
        spawnAmount = Value
    end	  
})

CoinSpawnTab:AddButton({
    Name = "Spawn!",
    Callback = function()
        game.Players.LocalPlayer.PlayerGui.Main.Top.Coins.Amount.Text = spawnAmount
    end    
})

-- Auto Farming GUI Tab
local AutoFarmTab = Window:MakeTab({
    Name = "Auto Farming GUI",
    Icon = "rbxassetid://17586593789",
    PremiumOnly = false
})

AutoFarmTab:AddParagraph("Auto Farm GUIs", "Click one of the GUIs for auto farm, auto roll, craft potions, and more.")

AutoFarmTab:AddButton({
    Name = "Auto Farm Gui 1 (KEY)",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/PETS-GO!-NEW-The-Best-Free-Script-ZapHub-20119"))()
    end    
})

AutoFarmTab:AddButton({
    Name = "Auto Farm Gui 2 (KEY)",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/PETS-GO!-NEW-BEST-SCRIPT-20019"))()
    end    
})

AutoFarmTab:AddButton({
    Name = "Auto Farm Gui 3 (NO KEY)",
    Callback = function()
        loadstring(game:HttpGet("https://rawscripts.net/raw/PETS-GO!-NEW-Speed-Hub-X-Keyless-20021"))()
    end    
})

AutoFarmTab:AddButton({
    Name = "Auto Farm Gui 4 (NO KEY)",
    Callback = function()
  loadstring(game:HttpGet("https://rawscripts.net/raw/PETS-GO!-NEW-AtherHub-20271"))()
end    
})

-- Extra Tab
local ExtraTab = Window:MakeTab({
    Name = "Extra",
    Icon = "rbxassetid://17586795822",
    PremiumOnly = false
})

ExtraTab:AddParagraph("Disclaimer", "All features are visual and for looks only; none of the gems are real.")
ExtraTab:AddParagraph("More", "Expect more features that work soon.")

ExtraTab:AddButton({
    Name = "Click to copy Discord link",
    Callback = function()
    setclipboard("https://discord.gg/twHaX3kGXG")
    end    
})

ExtraTab:AddButton({
    Name = "Anti AFK",
    Callback = function()
    end    
})

ExtraTab:AddButton({
    Name = "Anti MailStealer",
    Callback = function()
end 
})

ExtraTab:AddButton({
    Name = "Anti Lag",
    Callback = function()
        -- Add anti-Lag functionality if needed
    end    
})

-- Initialize the UI
OrionLib:Init()
