# Shark-Hub
-- SHARK HUB v2 | Loadstring Ready
-- Cole este código em um Pastebin/GitHub RAW e use:
-- loadstring(game:HttpGet("SEU_LINK_RAW"))()

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local RunService = game:GetService("RunService")

local LP = Players.LocalPlayer
local PG = LP:WaitForChild("PlayerGui")

-- Limpeza
for _, name in ipairs({"SharkHubMain", "SharkHubNotifs"}) do
    local old = PG:FindFirstChild(name)
    if old then old:Destroy() end
end

local Cor = {
    Fundo = Color3.fromRGB(5, 10, 18),
    Painel = Color3.fromRGB(12, 20, 32),
    Painel2 = Color3.fromRGB(18, 28, 45),
    Neon = Color3.fromRGB(0, 200, 255),
    NeonEscuro = Color3.fromRGB(0, 100, 160),
    Texto = Color3.fromRGB(230, 240, 255),
    TextoFraco = Color3.fromRGB(120, 150, 180),
    Perigo = Color3.fromRGB(255, 70, 90),
    Sucesso = Color3.fromRGB(0, 220, 130),
}

-- ============ GUI ============
local gui = Instance.new("ScreenGui")
gui.Name = "SharkHubMain"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.DisplayOrder = 100
gui.Parent = PG

-- ============ FUNDO COM RAIOS ============
local fundoRaios = Instance.new("Frame")
fundoRaios.Size = UDim2.new(1, 0, 1, 0)
fundoRaios.BackgroundColor3 = Color3.fromRGB(2, 5, 10)
fundoRaios.BackgroundTransparency = 0.3
fundoRaios.BorderSizePixel = 0
fundoRaios.ZIndex = 0
fundoRaios.Parent = gui

-- Linhas de raios animadas (efeito visual)
local raios = {}
for i = 1, 6 do
    local linha = Instance.new("Frame")
    linha.Size = UDim2.new(0, math.random(40, 120), 0, 2)
    linha.Position = UDim2.new(math.random(), 0, math.random(), 0)
    linha.BackgroundColor3 = Cor.Neon
    linha.BackgroundTransparency = 0.4
    linha.BorderSizePixel = 0
    linha.Rotation = math.random(-30, 30)
    linha.ZIndex = 0
    linha.Parent = fundoRaios
    table.insert(raios, linha)
end

-- Animação dos raios
task.spawn(function()
    while fundoRaios.Parent do
        for _, linha in ipairs(raios) do
            if linha.Parent then
                TweenService:Create(linha, TweenInfo.new(0.3, Enum.EasingStyle.Linear), {
                    BackgroundTransparency = math.random(60, 90) / 100,
                    Position = UDim2.new(math.random(), 0, math.random(), 0),
                    Rotation = math.random(-40, 40)
                }):Play()
            end
        end
        task.wait(0.4)
    end
end)

-- ============ PAINEL PRINCIPAL ============
local painel = Instance.new("Frame")
painel.Name = "Painel"
painel.Size = UDim2.fromOffset(300, 320)
painel.Position = UDim2.new(0.5, -150, 0.5, -160)
painel.BackgroundColor3 = Cor.Fundo
painel.BackgroundTransparency = 0.15
painel.BorderSizePixel = 0
painel.Active = true
painel.ZIndex = 2
painel.Parent = gui

local pc = Instance.new("UICorner")
pc.CornerRadius = UDim.new(0, 12)
pc.Parent = painel

local ps = Instance.new("UIStroke")
ps.Color = Cor.Neon
ps.Thickness = 2
ps.Parent = painel

-- ============ TOPO ============
local topo = Instance.new("Frame")
topo.Size = UDim2.new(1, 0, 0, 40)
topo.BackgroundColor3 = Cor.Painel
topo.BorderSizePixel = 0
topo.ZIndex = 3
topo.Parent = painel

local tc = Instance.new("UICorner")
tc.CornerRadius = UDim.new(0, 12)
tc.Parent = topo

-- Ícone do tubarão (emoji por enquanto, troque por ImageLabel se tiver asset)
local icone = Instance.new("TextLabel")
icone.Size = UDim2.fromOffset(32, 32)
icone.Position = UDim2.new(0, 8, 0.5, -16)
icone.BackgroundTransparency = 1
icone.Text = "🦈"
icone.TextSize = 22
icone.ZIndex = 4
icone.Parent = topo

local titulo = Instance.new("TextLabel")
titulo.Size = UDim2.new(0, 150, 1, 0)
titulo.Position = UDim2.new(0, 44, 0, 0)
titulo.BackgroundTransparency = 1
titulo.Text = "SHARK HUB"
titulo.TextColor3 = Cor.Neon
titulo.TextSize = 15
titulo.Font = Enum.Font.GothamBold
titulo.TextXAlignment = Enum.TextXAlignment.Left
titulo.ZIndex = 4
titulo.Parent = topo

-- Botões
local function criarCtrl(xPos, texto, cor, callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.fromOffset(30, 30)
    btn.Position = UDim2.new(1, xPos, 0.5, -15)
    btn.BackgroundColor3 = Cor.Painel2
    btn.Text = texto
    btn.TextColor3 = cor
    btn.TextSize = 14
    btn.Font = Enum.Font.GothamBold
    btn.AutoButtonColor = false
    btn.ZIndex = 4
    btn.Parent = topo
    local c = Instance.new("UICorner")
    c.CornerRadius = UDim.new(0, 6)
    c.Parent = btn
    btn.MouseButton1Click:Connect(callback)
    return btn
end

criarCtrl(-40, "–", Cor.TextoFraco, function()
    painel.Visible = false
    reabrir.Visible = true
end)

criarCtrl(-74, "✕", Cor.Perigo, function()
    gui:Destroy()
end)

-- ============ BOTÃO REABRIR (ARRASTÁVEL) ============
local reabrir = Instance.new("TextButton")
reabrir.Size = UDim2.fromOffset(48, 48)
reabrir.Position = UDim2.new(0, 20, 0.5, -24)
reabrir.BackgroundColor3 = Cor.Fundo
reabrir.Text = "🦈"
reabrir.TextSize = 24
reabrir.Visible = false
reabrir.AutoButtonColor = false
reabrir.ZIndex = 5
reabrir.Parent = gui

local rc = Instance.new("UICorner")
rc.CornerRadius = UDim.new(1, 0)
rc.Parent = reabrir

local rs = Instance.new("UIStroke")
rs.Color = Cor.Neon
rs.Thickness = 2
rs.Parent = reabrir

-- Arrastar o botão reabrir
local arrastandoBtn, inicioBtn, posBtn = false, nil, nil
reabrir.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastandoBtn = true
        inicioBtn = input.Position
        posBtn = reabrir.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if arrastandoBtn and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - inicioBtn
        reabrir.Position = UDim2.new(posBtn.X.Scale, posBtn.X.Offset + delta.X, posBtn.Y.Scale, posBtn.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastandoBtn = false
    end
end)

reabrir.MouseButton1Click:Connect(function()
    painel.Visible = true
    reabrir.Visible = false
end)

-- ============ CONTEÚDO ============
local conteudo = Instance.new("Frame")
conteudo.Size = UDim2.new(1, -16, 1, -56)
conteudo.Position = UDim2.new(0, 8, 0, 46)
conteudo.BackgroundTransparency = 1
conteudo.ZIndex = 3
conteudo.Parent = painel

local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, 0, 1, 0)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0
scroll.ScrollBarThickness = 3
scroll.ScrollBarImageColor3 = Cor.Neon
scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
scroll.ZIndex = 3
scroll.Parent = conteudo

local layout = Instance.new("UIListLayout")
layout.Padding = UDim.new(0, 6)
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Parent = scroll

layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
    scroll.CanvasSize = UDim2.new(0, 0, 0, layout.AbsoluteContentSize.Y + 6)
end)

-- ============ COMPONENTES ============
local function secao(texto)
    local t = Instance.new("TextLabel")
    t.Size = UDim2.new(1, 0, 0, 20)
    t.BackgroundTransparency = 1
    t.Text = "▸ " .. texto
    t.TextColor3 = Cor.Neon
    t.TextSize = 11
    t.Font = Enum.Font.GothamBold
    t.TextXAlignment = Enum.TextXAlignment.Left
    t.ZIndex = 4
    t.Parent = scroll
end

local function toggle(texto, padrao, callback)
    local valor = padrao or false
    local linha = Instance.new("TextButton")
    linha.Size = UDim2.new(1, 0, 0, 40)
    linha.BackgroundColor3 = Cor.Painel2
    linha.BackgroundTransparency = 0.2
    linha.Text = ""
    linha.AutoButtonColor = false
    linha.ZIndex = 4
    linha.Parent = scroll

    local lc = Instance.new("UICorner")
    lc.CornerRadius = UDim.new(0, 8)
    lc.Parent = linha

    local nome = Instance.new("TextLabel")
    nome.Size = UDim2.new(1, -55, 1, 0)
    nome.Position = UDim2.new(0, 10, 0, 0)
    nome.BackgroundTransparency = 1
    nome.Text = texto
    nome.TextColor3 = Cor.Texto
    nome.TextSize = 12
    nome.Font = Enum.Font.Gotham
    nome.TextXAlignment = Enum.TextXAlignment.Left
    nome.ZIndex = 5
    nome.Parent = linha

    local bolinha = Instance.new("Frame")
    bolinha.Size = UDim2.fromOffset(34, 18)
    bolinha.Position = UDim2.new(1, -44, 0.5, -9)
    bolinha.BackgroundColor3 = Color3.fromRGB(30, 40, 55)
    bolinha.ZIndex = 5
    bolinha.Parent = linha

    local bc = Instance.new("UICorner")
    bc.CornerRadius = UDim.new(1, 0)
    bc.Parent = bolinha

    local knob = Instance.new("Frame")
    knob.Size = UDim2.fromOffset(14, 14)
    knob.Position = UDim2.new(0, 2, 0.5, -7)
    knob.BackgroundColor3 = Cor.TextoFraco
    knob.ZIndex = 6
    knob.Parent = bolinha

    local kc = Instance.new("UICorner")
    kc.CornerRadius = UDim.new(1, 0)
    kc.Parent = knob

    local function aplicar(v)
        valor = v
        knob.Position = v and UDim2.new(1, -16, 0.5, -7) or UDim2.new(0, 2, 0.5, -7)
        knob.BackgroundColor3 = v and Cor.Neon or Cor.TextoFraco
        bolinha.BackgroundColor3 = v and Cor.NeonEscuro or Color3.fromRGB(30, 40, 55)
        if callback then pcall(callback, v) end
    end

    linha.MouseButton1Click:Connect(function() aplicar(not valor) end)
    aplicar(valor)
end

local function botao(texto, callback, cor)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, 0, 0, 36)
    btn.BackgroundColor3 = Cor.Painel2
    btn.Text = texto
    btn.TextColor3 = cor or Cor.Texto
    btn.TextSize = 12
    btn.Font = Enum.Font.GothamBold
    btn.AutoButtonColor = false
    btn.ZIndex = 4
    btn.Parent = scroll

    local bc = Instance.new("UICorner")
    bc.CornerRadius = UDim.new(0, 8)
    bc.Parent = btn

    btn.MouseButton1Click:Connect(function()
        if callback then pcall(callback) end
    end)
end

-- ============ FUNÇÕES (PLACEHOLDER) ============
secao("FARM")
toggle("Auto Coletar Ovos", false, function(v)
    print("[SHARK] Auto Coletar:", v)
end)
toggle("Auto Roubar (Steal)", false, function(v)
    print("[SHARK] Auto Steal:", v)
end)
toggle("Auto Chocar", false, function(v)
    print("[SHARK] Auto Hatch:", v)
end)
toggle("Auto Vender", false, function(v)
    print("[SHARK] Auto Sell:", v)
end)

secao("MOVIMENTO")
toggle("Speed Hack", false, function(v)
    local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed = v and 32 or 16 end
end)
toggle("Jump Hack", false, function(v)
    local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.UseJumpPower = true; hum.JumpPower = v and 100 or 50 end
end)

secao("VISUAL")
toggle("Full Bright", false, function(v)
    Lighting.Brightness = v and 3 or 2
    if v then Lighting.ClockTime = 14 end
end)

secao("MISC")
botao("Resetar Personagem", function()
    local hum = LP.Character and LP.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.Health = 0 end
end, Cor.Perigo)

-- ============ ARRASTAR PAINEL ============
local arrastando, inicio, posInicial = false, nil, nil
topo.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastando = true
        inicio = input.Position
        posInicial = painel.Position
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if arrastando and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
        local delta = input.Position - inicio
        painel.Position = UDim2.new(posInicial.X.Scale, posInicial.X.Offset + delta.X, posInicial.Y.Scale, posInicial.Y.Offset + delta.Y)
    end
end)
UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        arrastando = false
    end
end)

print("[SHARK HUB] Carregado.")
