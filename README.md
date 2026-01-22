# Yes--[[
    Wall Hop v1 
    Cores: Fundo Preto, Texto Vermelho
]]

local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local RootPart = Character:WaitForChild("HumanoidRootPart")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local wallHopEnabled = false

-- --- CRIAÇÃO DA INTERFACE (UI) ---
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local OnButton = Instance.new("TextButton")
local OffButton = Instance.new("TextButton")

ScreenGui.Parent = game:GetService("CoreGui") -- Para não sumir ao morrer
ScreenGui.Name = "WallHopPanel"

-- Painel Principal
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.new(0, 0, 0) -- Preto
MainFrame.Position = UDim2.new(0.5, -75, 0.5, -50)
MainFrame.Size = UDim2.new(0, 150, 0, 100)
MainFrame.Active = true
MainFrame.Draggable = true -- Permite arrastar o painel

-- Título
Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(1, 0, 0.4, 0)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "wall hop v1"
Title.TextColor3 = Color3.new(1, 0, 0) -- Vermelho
Title.TextSize = 20

-- Botão ON
OnButton.Name = "OnButton"
OnButton.Parent = MainFrame
OnButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
OnButton.Position = UDim2.new(0.1, 0, 0.5, 0)
OnButton.Size = UDim2.new(0.35, 0, 0.35, 0)
OnButton.Font = Enum.Font.SourceSans
OnButton.Text = "ON"
OnButton.TextColor3 = Color3.new(1, 1, 1)
OnButton.TextSize = 18

-- Botão OFF
OffButton.Name = "OffButton"
OffButton.Parent = MainFrame
OffButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
OffButton.Position = UDim2.new(0.55, 0, 0.5, 0)
OffButton.Size = UDim2.new(0.35, 0, 0.35, 0)
OffButton.Font = Enum.Font.SourceSans
OffButton.Text = "OFF"
OffButton.TextColor3 = Color3.new(1, 1, 1)
OffButton.TextSize = 18

-- --- LÓGICA DO SCRIPT ---

OnButton.MouseButton1Click:Connect(function()
    wallHopEnabled = true
    OnButton.BackgroundColor3 = Color3.new(0, 0.5, 0) -- Fica verde ao ligar
    OffButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
end)

OffButton.MouseButton1Click:Connect(function()
    wallHopEnabled = false
    OffButton.BackgroundColor3 = Color3.new(0.5, 0, 0) -- Fica vermelho ao desligar
    OnButton.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
end)

-- Função de Wall Hop (O "Flick")
UserInputService.JumpRequest:Connect(function()
    if wallHopEnabled then
        -- Verifica se o personagem está encostando em algo (parede)
        local raycastParams = RaycastParams.new()
        raycastParams.FilterDescendantsInstances = {Character}
        raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
        
        -- Lança um pequeno raio para frente para detectar a parede
        local ray = workspace:Raycast(RootPart.Position, RootPart.CFrame.LookVector * 2, raycastParams)
        
        if ray then
            -- Faz o personagem virar 180 graus de forma visível mas rápida
            for i = 1, 3 do
                RootPart.CFrame = RootPart.CFrame * CFrame.Angles(0, math.rad(180/3), 0)
                task.wait(0.01) -- Pequeno delay para "disfarçar"
            end
        end
    end
end)
