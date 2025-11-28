--// GG DARK HUB //--

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local savedPos = nil

local noclipEnabled = false
local floatEnabled = false

------------------------
-- GUI PRINCIPAL
------------------------

local gui = Instance.new("ScreenGui")
gui.Name = "GG_Dark_Hub"
gui.ResetOnSpawn = false
gui.Parent = game.CoreGui

-- Botão abrir painel
local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0, 120, 0, 40)
openBtn.Position = UDim2.new(0, 20, 0, 200)
openBtn.Text = "Abrir GG Dark"
openBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 200)
openBtn.TextColor3 = Color3.new(1,1,1)
openBtn.Parent = gui

-- Painel principal
local main = Instance.new("Frame")
main.Size = UDim2.new(0, 300, 0, 350)
main.Position = UDim2.new(0, 20, 0, 250)
main.BackgroundColor3 = Color3.fromRGB(100, 0, 150)
main.Visible = false
main.Parent = gui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,0,0,40)
title.Text = "GG Dark Hub"
title.BackgroundColor3 = Color3.fromRGB(80, 0, 120)
title.TextColor3 = Color3.new(1,1,1)
title.Parent = main

-- Botão fechar
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 80, 0, 35)
closeBtn.Position = UDim2.new(1, -90, 0, 5)
closeBtn.Text = "Fechar"
closeBtn.BackgroundColor3 = Color3.fromRGB(70, 0, 110)
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.Parent = main

------------------------
-- FUNÇÃO CRIAR BOTÃO
------------------------

local function makeBtn(text, y)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(1, -20, 0, 40)
    b.Position = UDim2.new(0, 10, 0, y)
    b.Text = text
    b.BackgroundColor3 = Color3.fromRGB(150, 0, 200)
    b.TextColor3 = Color3.new(1,1,1)
    b.Parent = main
    return b
end

local saveBtn   = makeBtn("Salvar Posição", 60)
local tpBtn     = makeBtn("Teleportar", 110)
local speedBtn  = makeBtn("Speed", 160)
local onoffBtn  = makeBtn("Painel ON/OFF", 210)

------------------------
-- PAINEL SPEED
------------------------

local speedPanel = Instance.new("Frame")
speedPanel.Size = UDim2.new(0, 200, 0, 140)
speedPanel.Position = UDim2.new(0, 330, 0, 250)
speedPanel.BackgroundColor3 = Color3.fromRGB(100, 0, 150)
speedPanel.Visible = false
speedPanel.Parent = gui

local speedTitle = Instance.new("TextLabel")
speedTitle.Size = UDim2.new(1,0,0,30)
speedTitle.Text = "Speed (max 100)"
speedTitle.BackgroundColor3 = Color3.fromRGB(80, 0, 120)
speedTitle.TextColor3 = Color3.new(1,1,1)
speedTitle.Parent = speedPanel

local speedBox = Instance.new("TextBox")
speedBox.Size = UDim2.new(1, -20, 0, 40)
speedBox.Position = UDim2.new(0, 10, 0, 40)
speedBox.PlaceholderText = "Velocidade"
speedBox.BackgroundColor3 = Color3.fromRGB(150, 0, 200)
speedBox.TextColor3 = Color3.new(1,1,1)
speedBox.Parent = speedPanel

local applySpeed = Instance.new("TextButton")
applySpeed.Size = UDim2.new(1, -20, 0, 40)
applySpeed.Position = UDim2.new(0, 10, 0, 90)
applySpeed.Text = "Aplicar"
applySpeed.BackgroundColor3 = Color3.fromRGB(150, 0, 200)
applySpeed.TextColor3 = Color3.new(1,1,1)
applySpeed.Parent = speedPanel

------------------------
-- PAINEL ON/OFF
------------------------

local onoffPanel = Instance.new("Frame")
onoffPanel.Size = UDim2.new(0, 220, 0, 200)
onoffPanel.Position = UDim2.new(0, 340, 0, 420)
onoffPanel.BackgroundColor3 = Color3.fromRGB(100, 0, 150)
onoffPanel.Visible = false
onoffPanel.Parent = gui

local onoffTitle = Instance.new("TextLabel")
onoffTitle.Size = UDim2.new(1,0,0,30)
onoffTitle.Text = "Funções ON/OFF"
onoffTitle.BackgroundColor3 = Color3.fromRGB(80, 0, 120)
onoffTitle.TextColor3 = Color3.new(1,1,1)
onoffTitle.Parent = onoffPanel

-- Botões ON/OFF
local function makeToggle(name, posY)
    local t = Instance.new("TextButton")
    t.Size = UDim2.new(1, -20, 0, 40)
    t.Position = UDim2.new(0, 10, 0, posY)
    t.BackgroundColor3 = Color3.fromRGB(150, 0, 200)
    t.TextColor3 = Color3.new(1,1,1)
    t.Text = name
    t.Parent = onoffPanel
    return t
end

local jumpOffBtn  = makeToggle("Jump OFF", 40)
local floatBtn    = makeToggle("Flutuar ON/OFF", 90)
local noclipBtn   = makeToggle("Noclip ON/OFF", 140)

------------------------
-- FUNÇÕES DO SISTEMA
------------------------

openBtn.MouseButton1Click:Connect(function()
    main.Visible = true
end)

closeBtn.MouseButton1Click:Connect(function()
    main.Visible = false
end)

-- Salvar posição
saveBtn.MouseButton1Click:Connect(function()
    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
    if hrp then savedPos = hrp.CFrame end
end)

-- Teleportar
tpBtn.MouseButton1Click:Connect(function()
    if savedPos then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then hrp.CFrame = savedPos end
    end
end)

-- Abrir painel speed
speedBtn.MouseButton1Click:Connect(function()
    speedPanel.Visible = not speedPanel.Visible
end)

-- Aplicar speed
applySpeed.MouseButton1Click:Connect(function()
    local v = tonumber(speedBox.Text)
    if v and v <= 100 then
        player.Character.Humanoid.WalkSpeed = v
    end
end)

-- Abrir painel ON/OFF
onoffBtn.MouseButton1Click:Connect(function()
    onoffPanel.Visible = not onoffPanel.Visible
end)

------------------------
-- FUNÇÕES ON/OFF
------------------------

-- Jump OFF
jumpOffBtn.MouseButton1Click:Connect(function()
    player.Character.Humanoid.JumpPower = 0
end)

-- Flutuar
floatBtn.MouseButton1Click:Connect(function()
    floatEnabled = not floatEnabled
end)

game:GetService("RunService").Heartbeat:Connect(function()
    if floatEnabled then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.Velocity = Vector3.new(0, 4, 0)
        end
    end
end)

-- Noclip
noclipBtn.MouseButton1Click:Connect(function()
    noclipEnabled = not noclipEnabled
end)

game:GetService("RunService").Stepped:Connect(function()
    if noclipEnabled then
        for _, v in pairs(player.Character:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end
end)
