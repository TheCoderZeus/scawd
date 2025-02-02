-- Script ESP

local players = game:GetService("Players")
local runService = game:GetService("RunService")
local camera = game.Workspace.CurrentCamera

local function createESP(player)
    local highlight = Instance.new("Highlight")
    highlight.Parent = player.Character
    highlight.Adornee = player.Character
    highlight.FillColor = Color3.fromRGB(255, 0, 0)
    highlight.FillTransparency = 0.5
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.OutlineTransparency = 0.5
end

local function onPlayerAdded(player)
    player.CharacterAdded:Connect(function(character)
        createESP(player)
    end)
end

players.PlayerAdded:Connect(onPlayerAdded)

for _, player in pairs(players:GetPlayers()) do
    if player.Character then
        createESP(player)
    end
end

runService.RenderStepped:Connect(function()
    for _, player in pairs(players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChildOfClass("Highlight") then
            player.Character.Highlight.Adornee = player.Character
        end
    end
end)
