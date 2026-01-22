# Yes--[[
--[[
    Wall Hop v1.1 (Updated: Faster Turn + Strong Jump)
    Cores: Fundo Preto, Texto Vermelho
]]

local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local UserInputService = game:GetService("UserInputService")

local wallHopEnabled = false

-- --- CRIAÇÃO DA INTERFACE (UI) ---
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local OnButton = Instance.new("TextButton")
local OffButton = Instance.new("TextButton")

ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.Name = "WallHopPanelV1_1"

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
MainFrame.Position = UDim2.new(0.5, -75, 0.5, -50)
MainFrame.Size = UDim2.new(0, 150, 0, 110)
MainFrame.Active = true
MainFrame.Draggable = true

Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, 0, 0.4, 0)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "wall hop v1.1"
Title.TextColor3 = Color3.new(1, 0, 0)
Title.TextSize = 22

OnButton.Name = "OnButton"
OnButton.Parent = MainFrame
OnButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
OnButton.Position = UDim2.new(0.1, 0, 0.55, 0)
OnButton.Size = UDim2.new(0.35, 0, 0.3, 0)
OnButton.Font = Enum.Font.SourceSansBold
OnButton.Text = "ON"
OnButton.TextColor3 = Color3.new(1, 1, 1)
OnButton.TextSize = 16

OffButton.Name = "OffButton"
OffButton.Parent = MainFrame
OffButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
OffButton.Position = UDim2.new(0.55, 0, 0.55, 0)
OffButton.Size = UDim2.new(0.35, 0, 0.3, 0)
OffButton.Font = Enum.Font.SourceSansBold
OffButton.Text = "OFF"
OffButton.TextColor3 = Color3.new(1, 1, 1)
OffButton.TextSize = 16

-- --- LÓGICA DE FUNCIONAMENTO ---

OnButton.MouseButton1Click:Connect(function()
    wallHopEnabled = true
    OnButton.BackgroundColor3 = Color3.new(0, 0.5, 0)
    OffButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
end)

OffButton.MouseButton1Click:Connect(function()
    wallHopEnabled = false
    OffButton.BackgroundColor3 = Color3.new(0.5, 0, 0)
    OnButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
end)

-- Sistema de Pulo, Giro Rápido e Raycast
UserInputService.JumpRequest:Connect(function()
    if wallHopEnabled then
        local rayParams = RaycastParams.new()
        rayParams.FilterDescendantsInstances = {Character}
        rayParams.FilterType = Enum.RaycastFilterType.Blacklist
        
        -- Raycast frontal para detectar parede
        local ray = workspace:Raycast(RootPart.Position, RootPart.CFrame.LookVector * 2.5, rayParams)
        
        if ray then
            -- 1. Mini Infinite Jump (Impulso Forte)
            RootPart.Velocity = Vector3.new(RootPart.Velocity.X, Humanoid.JumpPower * 1.35, RootPart.Velocity.Z)
            
            -- 2. Giro Ultra Rápido (Reduzido para 2 etapas de 90 graus)
            -- Isso faz o giro ser quase instantâneo, mas ainda processado pelo motor gráfico
            for i = 1, 2 do
                RootPart.CFrame = RootPart.CFrame * CFrame.Angles(0, math.rad(90), 0)
                task.wait() -- Espera o tempo mínimo de um frame
            end
        end
    end
end)
