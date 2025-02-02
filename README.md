```
local players = game:GetService("Players")
local runService = game:GetService("RunService")

local localPlayer = players.LocalPlayer
local espEnabled = false

local function highlightPlayer(player, color)
    if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
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
end

local function updateESP()
    if espEnabled then
        for _, player in pairs(players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                local isMurderer = player.Backpack:FindFirstChild("Knife") or player.Character:FindFirstChild("Knife")
                local isSheriff = player.Backpack:FindFirstChild("Gun") or player.Character:FindFirstChild("Gun")
                
                if isMurderer then
                    highlightPlayer(player, Color3.fromRGB(255, 0, 0)) -- Assassino em vermelho
                elseif isSheriff then
                    highlightPlayer(player, Color3.fromRGB(0, 0, 255)) -- Xerife em azul
                else
                    local highlight = player.Character:FindFirstChildOfClass("Highlight")
                    if highlight then
                        highlight:Destroy()
                    end
                end
            end
        end
    else
        for _, player in pairs(players:GetPlayers()) do
            local highlight = player.Character:FindFirstChildOfClass("Highlight")
            if highlight then
                highlight:Destroy()
            end
        end
    end
end

runService.RenderStepped:Connect(updateESP)

players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(updateESP)
end)

players.PlayerRemoving:Connect(updateESP)

-- Função para criar o menu de ativação/desativação do ESP
local function createToggleMenu()
    local screenGui = Instance.new("ScreenGui")
    local frame = Instance.new("Frame")
    local textLabel = Instance.new("TextLabel")
    local toggleButton = Instance.new("TextButton")
    local openMenuButton = Instance.new("TextButton")
    local uiCorner = Instance.new("UICorner")
    local uiStroke = Instance.new("UIStroke")

    screenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    screenGui.Name = "ESPToggleGui"

    frame.Size = UDim2.new(0, 250, 0, 150)
    frame.Position = UDim2.new(0.5, -125, 0.5, -75)
    frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    frame.BackgroundTransparency = 0.1
    frame.Visible = false
    frame.Parent = screenGui

    uiCorner.CornerRadius = UDim.new(0, 10)
    uiCorner.Parent = frame

    uiStroke.Color = Color3.fromRGB(255, 255, 255)
    uiStroke.Thickness = 2
    uiStroke.Parent = frame

    textLabel.Size = UDim2.new(1, 0, 0.4, 0)
    textLabel.Position = UDim2.new(0, 0, 0, 0)
    textLabel.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.Font = Enum.Font.SourceSans
    textLabel.TextSize = 24
    textLabel.Text = "ESP Menu"
    textLabel.Parent = frame

    toggleButton.Size = UDim2.new(0.8, 0, 0.3, 0)
    toggleButton.Position = UDim2.new(0.1, 0, 0.6, 0)
    toggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    toggleButton.Text = "Ativar ESP"
    toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggleButton.Font = Enum.Font.SourceSans
    toggleButton.TextSize = 24
    toggleButton.Parent = frame

    uiCorner:Clone().Parent = toggleButton
    uiStroke:Clone().Parent = toggleButton

    toggleButton.MouseButton1Click:Connect(function()
        espEnabled = not espEnabled
        if espEnabled then
            toggleButton.Text = "Desativar ESP"
        else
            toggleButton.Text = "Ativar ESP"
        end
        updateESP()
    end)

    -- Botão para abrir o menu
    openMenuButton.Size = UDim2.new(0, 100, 0, 50)
    openMenuButton.Position = UDim2.new(0, 10, 0, 10)
    openMenuButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    openMenuButton.Text = "Menu"
    openMenuButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    openMenuButton.Font = Enum.Font.SourceSans
    openMenuButton.TextSize = 24
    openMenuButton.Parent = screenGui

    uiCorner:Clone().Parent = openMenuButton
    uiStroke:Clone().Parent = openMenuButton

    openMenuButton.MouseButton1Click:Connect(function()
        frame.Visible = not frame.Visible
    end)
end

createToggleMenu()
```
