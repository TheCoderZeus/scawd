-- Script ESP com linhas conectando jogadores

local players = game:GetService("Players")
local runService = game:GetService("RunService")
local camera = game.Workspace.CurrentCamera

local localPlayer = players.LocalPlayer

local function createLine()
    local line = Instance.new("Part")
    line.Anchored = true
    line.CanCollide = false
    line.Transparency = 0.5
    line.Color = Color3.fromRGB(255, 0, 0)
    line.Material = Enum.Material.Neon
    line.Size = Vector3.new(0.1, 0.1, 0.1)
    line.Parent = workspace
    return line
end

local lines = {}

local function updateLines()
    for _, line in pairs(lines) do
        line:Destroy()
    end
    lines = {}

    for _, player in pairs(players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local line = createLine()
            table.insert(lines, line)

            local localHRP = localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart")
            local targetHRP = player.Character:FindFirstChild("HumanoidRootPart")
            
            if localHRP and targetHRP then
                local distance = (localHRP.Position - targetHRP.Position).magnitude
                line.Size = Vector3.new(0.1, 0.1, distance)
                line.CFrame = CFrame.new(localHRP.Position, targetHRP.Position) * CFrame.new(0, 0, -distance / 2)
            end
        end
    end
end

runService.RenderStepped:Connect(updateLines)

players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(updateLines)
end)

players.PlayerRemoving:Connect(updateLines)
