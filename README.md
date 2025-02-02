```
local players = game:GetService("Players")
local runService = game:GetService("RunService")
local camera = game.Workspace.CurrentCamera
local userInputService = game:GetService("UserInputService")
local tweenService = game:GetService("TweenService")

local localPlayer = players.LocalPlayer
local espEnabled = true
local lines = {}

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

local function updateLines()
    for _, line in pairs(lines) do
        line:Destroy()
    end
    lines = {}

    if espEnabled then
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
end

-- Função para criar o botão com efeito RGB
local function createToggleButton()
    local screenGui = Instance.new("ScreenGui")
    local button = Instance.new("TextButton")

    screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    screenGui.Name = "ESPToggleGui"

    button.Size = UDim2.new(0, 200, 0, 50)
    button.Position = UDim2.new(0.5, -100, 0.9, -25)
    button.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    button.Text = "Toggle ESP"
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSans
    button.TextSize = 24
    button.Parent = screenGui

    -- Efeito RGB
    local hue = 0
    runService.RenderStepped:Connect(function()
        hue = hue + 0.01
        if hue > 1 then
            hue = 0
        end
        button.BackgroundColor3 = Color3.fromHSV(hue, 1, 1)
    end)

    button.MouseButton1Click:Connect(function()
        espEnabled = not espEnabled
        updateLines()
    end)
end

createToggleButton()
runService.RenderStepped:Connect(updateLines)

players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(updateLines)
end)

players.PlayerRemoving:Connect(updateLines)

```
