```
local players = game:GetService("Players")
local runService = game:GetService("RunService")

local localPlayer = players.LocalPlayer
local espEnabled = true

local function highlightPlayer(player, color)
    local highlight = player.Character:FindFirstChildOfClass("Highlight")
    if not highlight then
        highlight = Instance.new("Highlight")
        highlight.Parent = player.Character
        highlight.Adornee = player.Character
    end
    highlight.FillColor = color
    highlight.FillTransparency = 0.5
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.OutlineTransparency = 0.5
end

local function updateESP()
    for _, player in pairs(players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("Humanoid") then
            if player.Backpack:FindFirstChild("Knife") or player.Character:FindFirstChild("Knife") then
                highlightPlayer(player, Color3.fromRGB(255, 0, 0)) -- Assassino em vermelho
            elseif player.Backpack:FindFirstChild("Gun") or player.Character:FindFirstChild("Gun") then
                highlightPlayer(player, Color3.fromRGB(0, 0, 255)) -- Xerife em azul
            else
                local highlight = player.Character:FindFirstChildOfClass("Highlight")
                if highlight then
                    highlight:Destroy()
                end
            end
        end
    end
end

runService.RenderStepped:Connect(updateESP)

players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(updateESP)
end)

players.PlayerRemoving:Connect(updateESP)

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
        if espEnabled then
            runService.RenderStepped:Connect(updateESP)
        else
            for _, player in pairs(players:GetPlayers()) do
                local highlight = player.Character:FindFirstChildOfClass("Highlight")
                if highlight then
                    highlight:Destroy()
                end
            end
        end
    end)
end

createToggleButton()

```
