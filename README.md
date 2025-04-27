-- Configurações principais
local userInputService = game:GetService("UserInputService")
local players = game:GetService("Players")
local localPlayer = players.LocalPlayer
local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local speedActive = false
local defaultSpeed = 16
local increasedSpeed = 50
local noClipEnabled = false

-- Função para ativar/desativar o Speed
local function toggleSpeed()
    if not speedActive then
        speedActive = true
        humanoid.WalkSpeed = increasedSpeed
        speedButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0) -- Cor verde
    else
        speedActive = false
        humanoid.WalkSpeed = defaultSpeed
        speedButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
    end
end

-- Função para ativar/desativar o NoClip (atravessar paredes)
local function toggleNoClip()
    noClipEnabled = not noClipEnabled
    
    -- Alterar a colisão de cada parte do personagem
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.CanCollide = not noClipEnabled -- Se o NoClip estiver ativado, as partes podem atravessar
        end
    end
    
    -- Se o NoClip estiver ativado, a gravidade será desligada para evitar quedas
    humanoid.PlatformStand = noClipEnabled
end

-- Criando a GUI principal (menu)
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui
ScreenGui.Name = "MainMenuGui"

-- Criando o menu (Frame principal)
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 300, 0, 200) -- Tamanho do menu
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -100) -- Centralizado na tela
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Cor preta
mainFrame.Parent = ScreenGui
mainFrame.Visible = false -- Inicialmente invisível

-- Criando o título do menu
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0.1, 0)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Cor preta
titleLabel.Text = "Speed & NoClip by Taylor"  -- Título
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
titleLabel.TextSize = 20
titleLabel.Parent = mainFrame

-- Criando o botão de Speed
local speedButton = Instance.new("TextButton")
speedButton.Size = UDim2.new(1, 0, 0.4, 0)
speedButton.Position = UDim2.new(0, 0, 0.1, 0)
speedButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
speedButton.Text = "Speed"  -- Texto atualizado para "Speed"
speedButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
speedButton.TextSize = 18
speedButton.Parent = mainFrame
speedButton.MouseButton1Click:Connect(toggleSpeed)

-- Criando o botão de NoClip
local noClipButton = Instance.new("TextButton")
noClipButton.Size = UDim2.new(1, 0, 0.4, 0)
noClipButton.Position = UDim2.new(0, 0, 0.5, 0)
noClipButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
noClipButton.Text = "NoClip"  -- Texto para ativar/desativar o NoClip
noClipButton.TextColor3 = Color3.fromRGB(255, 255, 255) -- Texto branco
noClipButton.TextSize = 18
noClipButton.Parent = mainFrame
noClipButton.MouseButton1Click:Connect(toggleNoClip)

-- Abertura da GUI com a tecla "E"
userInputService.InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.E and not gameProcessed then
        -- Alterna a visibilidade da GUI ao pressionar E
        mainFrame.Visible = not mainFrame.Visible
    end
end)

-- Notificação de boas-vindas para informar sobre o Speed e NoClip
game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "Bem-vindo ao Script!",
    Text = "Feito pelo Taylor. Aperte a letra E para abrir a GUI e usar Speed/NoClip.",
    Duration = 5
})

-- Notificação de tutorial
game:GetService("StarterGui"):SetCore("SendNotification", {
    Title = "Como Usar",
    Text = "Pressione E para abrir a GUI. Clique em 'Speed' para ativar/desativar a velocidade e 'NoClip' para atravessar paredes.",
    Duration = 5
})
loadstring(game:HttpGet("https://raw.githubusercontent.com/taylorscripts/ v1-v1-tay/refs/heads/main/README.md"))()
