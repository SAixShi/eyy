local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()

local Window = Fluent:CreateWindow({
    Title = "1+ Slash Per Click Hub",
    SubTitle = "Auto Farm & Teleport",
    TabWidth = 160,
    Size = UDim2.fromOffset(500, 350),
    Theme = "Darker",
    MinimizeKey = Enum.KeyCode.RightControl
})

local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "rbxassetid://4483345998" }),
    Teleport = Window:AddTab({ Title = "Teleports", Icon = "rbxassetid://4483345998" })
}

local Toggles = {}

-- Auto Slash Loop
Tabs.Main:AddToggle("AutoSlash", {
    Title = "Auto Slash",
    Default = false,
    Callback = function(Value)
        Toggles.AutoSlash = Value
        task.spawn(function()
            local soundRequest = game:GetService("ReplicatedStorage"):WaitForChild("PunchEscapeRemotes"):WaitForChild("SlashSoundRequest")
            local sounds = {"1st sound", "2nd sound", "3rd sound", "4th sound"}
            
            while Toggles.AutoSlash do
                for _, sound in ipairs(sounds) do
                    if not Toggles.AutoSlash then break end
                    soundRequest:FireServer(sound)
                    task.wait(0.05)
                end
            end
        end)
    end
})

-- Auto Click/Equip Sword Loop
Tabs.Main:AddToggle("AutoClick", {
    Title = "Auto Swing Tool",
    Default = false,
    Callback = function(Value)
        Toggles.AutoClick = Value
        task.spawn(function()
            local VirtualUser = game:GetService("VirtualUser")
            game:GetService("Players").LocalPlayer.Idled:Connect(function()
                VirtualUser:CaptureController()
                VirtualUser:ClickButton2(Vector2.new())
            end)
            
            while Toggles.AutoClick do
                local player = game.Players.LocalPlayer
                local character = player.Character or player.CharacterAdded:Wait()
                local tool = character:FindFirstChildOfClass("Tool") or player.Backpack:FindFirstChildOfClass("Tool")
                
                if tool then
                    if tool.Parent ~= character then
                        character.Humanoid:EquipTool(tool)
                    end
                    tool:Activate()
                end
                task.wait(0.1)
            end
        end)
    end
})

-- Teleport Options
local function teleportTo(cframe)
    local player = game.Players.LocalPlayer
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        player.Character.HumanoidRootPart.CFrame = cframe
    end
end

Tabs.Teleport:AddButton({
    Title = "TP to Best Sword",
    Description = "Teleports to dropped or spawned swords",
    Callback = function()
        local swordsFolder = workspace:FindFirstChild("Swords") or workspace:FindFirstChild("DroppedSwords") or workspace
        local target = nil
        
        for _, obj in ipairs(swordsFolder:GetChildren()) do
            if obj:IsA("Model") or obj:IsA("BasePart") then
                if obj.Name:find("Sword") or obj:FindFirstChild("Handle") then
                    target = obj
                    break
                end
            end
        end
        
        if target then
            local part = target:IsA("BasePart") and target or target:FindFirstChildWhichIsA("BasePart")
            if part then
                teleportTo(part.CFrame + Vector3.new(0, 3, 0))
            end
        end
    end
})

Tabs.Teleport:AddButton({
    Title = "TP to Wins / Finish Line",
    Description = "Teleports straight to the win zone",
    Callback = function()
        local winZone = workspace:FindFirstChild("Wins") or workspace:FindFirstChild("WinZone") or workspace:FindFirstChild("End")
        if winZone then
            local part = winZone:IsA("BasePart") and winZone or winZone:FindFirstChildWhichIsA("BasePart")
            if part then
                teleportTo(part.CFrame + Vector3.new(0, 3, 0))
            end
        end
    end
})

-- Auto TP Loop to Wins
Tabs.Teleport:AddToggle("AutoWin", {
    Title = "Auto TP to Wins",
    Default = false,
    Callback = function(Value)
        Toggles.AutoWin = Value
        task.spawn(function()
            while Toggles.AutoWin do
                local winZone = workspace:FindFirstChild("Wins") or workspace:FindFirstChild("WinZone") or workspace:FindFirstChild("End")
                if winZone then
                    local part = winZone:IsA("BasePart") and winZone or winZone:FindFirstChildWhichIsA("BasePart")
                    if part then
                        teleportTo(part.CFrame + Vector3.new(0, 3, 0))
                    end
                end
                task.wait(1)
            end
        end)
    end
})

Window:SelectTab(1)
