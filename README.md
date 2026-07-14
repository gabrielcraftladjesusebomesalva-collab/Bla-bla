-- Evita recriar a interface caso já tenha executado antes
if game.Players.LocalPlayer:WaitForChild("PlayerGui"):FindFirstChild("HospitalHub") then
    game.Players.LocalPlayer.PlayerGui.HospitalHub:Destroy()
end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

-- ==========================================
-- INTERFACE PRINCIPAL (Design Ultra Arredondado)
-- ==========================================
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "HospitalHub"
ScreenGui.Parent = LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 560, 0, 340)
MainFrame.Position = UDim2.new(0.5, -280, 0.5, -170)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 18)
MainFrame.Active = true
MainFrame.Draggable = true 
MainFrame.Parent = ScreenGui

local TopBorder = Instance.new("Frame")
TopBorder.Size = UDim2.new(1, 0, 0, 4)
TopBorder.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
TopBorder.BorderSizePixel = 0
TopBorder.Parent = MainFrame

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 16)
MainCorner.Parent = MainFrame

-- ==========================================
-- BOTÃO DE ABRIR: BARRA REDONDA (HOSPITAL / BIEL)
-- ==========================================
local OpenBtn = Instance.new("TextButton")
OpenBtn.Name = "OpenBtn"
OpenBtn.Size = UDim2.new(0, 180, 0, 50)
OpenBtn.Position = UDim2.new(0, 20, 0, 20)
OpenBtn.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
OpenBtn.Text = "Hospital 🏥\nCRIADOR: BIEL"
OpenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
OpenBtn.TextSize = 13
OpenBtn.Font = Enum.Font.GothamBold
OpenBtn.Visible = false
OpenBtn.Active = true
OpenBtn.Draggable = true 
OpenBtn.Parent = ScreenGui

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(0, 25)
OpenCorner.Parent = OpenBtn

-- Topbar
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 45)
TopBar.BackgroundTransparency = 1
TopBar.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -90, 1, 0)
Title.Position = UDim2.new(0, 18, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "HOSPITAL ANOMALIAS - HUB"
Title.TextColor3 = Color3.fromRGB(245, 245, 245)
Title.TextSize = 13
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

local MinimizeBtn = Instance.new("TextButton")
MinimizeBtn.Size = UDim2.new(0, 30, 0, 30)
MinimizeBtn.Position = UDim2.new(1, -75, 0, 8)
MinimizeBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
MinimizeBtn.Text = "-"
MinimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinimizeBtn.TextSize = 18
MinimizeBtn.Font = Enum.Font.GothamBold
MinimizeBtn.Parent = TopBar

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 8)
MinCorner.Parent = MinimizeBtn

local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -38, 0, 8)
CloseBtn.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 14
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.Parent = TopBar

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 8)
CloseCorner.Parent = CloseBtn

MinimizeBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenBtn.Visible = true
end)

OpenBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenBtn.Visible = false
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Menu Lateral (Arredondado)
local SideMenu = Instance.new("Frame")
SideMenu.Size = UDim2.new(0, 160, 1, -55)
SideMenu.Position = UDim2.new(0, 8, 0, 48)
SideMenu.BackgroundColor3 = Color3.fromRGB(11, 11, 13)
SideMenu.Parent = MainFrame

local SideCorner = Instance.new("UICorner")
SideCorner.CornerRadius = UDim.new(0, 12)
SideCorner.Parent = SideMenu

local ContentContainer = Instance.new("Frame")
ContentContainer.Size = UDim2.new(1, -182, 1, -55)
ContentContainer.Position = UDim2.new(0, 174, 0, 48)
ContentContainer.BackgroundTransparency = 1
ContentContainer.Parent = MainFrame

local abas = {}
local botoesAba = {}

local function criarAba(nome, id)
    local frameAba = Instance.new("ScrollingFrame")
    frameAba.Size = UDim2.new(1, 0, 1, 0)
    frameAba.BackgroundTransparency = 1
    frameAba.CanvasSize = UDim2.new(0, 0, 2, 0) -- Espaço extra para rolagem de listas
    frameAba.ScrollBarThickness = 3
    frameAba.ScrollBarImageColor3 = Color3.fromRGB(235, 35, 35)
    frameAba.Visible = false
    frameAba.Parent = ContentContainer
    
    local layout = Instance.new("UIListLayout")
    layout.Padding = UDim.new(0, 8)
    layout.Parent = frameAba
    
    abas[id] = frameAba
    
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -12, 0, 36)
    btn.Position = UDim2.new(0, 6, 0, (#botoesAba * 42) + 10)
    btn.BackgroundColor3 = Color3.fromRGB(22, 22, 26)
    btn.Text = nome
    btn.TextColor3 = Color3.fromRGB(160, 160, 160)
    btn.TextSize = 12
    btn.Font = Enum.Font.GothamMedium
    btn.Parent = SideMenu
    
    local btnCorner = Instance.new("UICorner")
    btnCorner.CornerRadius = UDim.new(0, 10)
    btnCorner.Parent = btn
    
    btn.MouseButton1Click:Connect(function()
        for _, f in pairs(abas) do f.Visible = false end
        for _, b in pairs(botoesAba) do 
            b.BackgroundColor3 = Color3.fromRGB(22, 22, 26) 
            b.TextColor3 = Color3.fromRGB(160, 160, 160) 
        end
        frameAba.Visible = true
        btn.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
        btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    end)
    
    table.insert(botoesAba, btn)
    return frameAba
end

-- ==========================================
-- DEFINIÇÃO DAS ABAS
-- ==========================================
local AbaHospital = criarAba("Hospital 🏥", "hospital")
local AbaVisual = criarAba("Visual 👁️", "visual")
local AbaLocalPlayer = criarAba("Local Player 👤", "localplayer")
local AbaSobre = criarAba("Sobre ℹ️", "sobre")

abas["hospital"].Visible = true
botoesAba[1].BackgroundColor3 = Color3.fromRGB(235, 35, 35)
botoesAba[1].TextColor3 = Color3.fromRGB(255, 255, 255)

-- ==========================================
-- CRIADORES DE ELEMENTOS
-- ==========================================
local function criarToggle(parent, texto, callback)
    local Container = Instance.new("Frame")
    Container.Size = UDim2.new(1, -10, 0, 42)
    Container.BackgroundColor3 = Color3.fromRGB(20, 20, 24)
    Container.Parent = parent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Container

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -70, 1, 0)
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = texto
    Label.TextColor3 = Color3.fromRGB(230, 230, 230)
    Label.TextSize = 12
    Label.Font = Enum.Font.GothamMedium
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Container

    local SwitchBg = Instance.new("TextButton")
    SwitchBg.Size = UDim2.new(0, 42, 0, 22)
    SwitchBg.Position = UDim2.new(1, -55, 0.5, -11)
    SwitchBg.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    SwitchBg.Text = ""
    SwitchBg.Parent = Container

    local SwitchCorner = Instance.new("UICorner")
    SwitchCorner.CornerRadius = UDim.new(1, 0)
    SwitchCorner.Parent = SwitchBg

    local SwitchBall = Instance.new("Frame")
    SwitchBall.Size = UDim2.new(0, 16, 0, 16)
    SwitchBall.Position = UDim2.new(0, 3, 0.5, -8)
    SwitchBall.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    SwitchBall.Parent = SwitchBg

    local BallCorner = Instance.new("UICorner")
    BallCorner.CornerRadius = UDim.new(1, 0)
    BallCorner.Parent = SwitchBall

    local estado = false
    SwitchBg.MouseButton1Click:Connect(function()
        estado = not estado
        if estado then
            SwitchBg.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
            SwitchBall.Position = UDim2.new(1, -19, 0.5, -8)
        else
            SwitchBg.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
            SwitchBall.Position = UDim2.new(0, 3, 0.5, -8)
        end
        callback(estado)
    end)
    return SwitchBg
end

local function criarSlider(parent, texto, min, max, padrao, callback)
    local Container = Instance.new("Frame")
    Container.Size = UDim2.new(1, -10, 0, 55)
    Container.BackgroundColor3 = Color3.fromRGB(20, 20, 24)
    Container.Parent = parent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Container

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -80, 0, 25)
    Label.Position = UDim2.new(0, 12, 0, 4)
    Label.BackgroundTransparency = 1
    Label.Text = texto
    Label.TextColor3 = Color3.fromRGB(230, 230, 230)
    Label.TextSize = 12
    Label.Font = Enum.Font.GothamMedium
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Container

    local ValueLabel = Instance.new("TextLabel")
    ValueLabel.Size = UDim2.new(0, 60, 0, 25)
    ValueLabel.Position = UDim2.new(1, -72, 0, 4)
    ValueLabel.BackgroundTransparency = 1
    ValueLabel.Text = tostring(padrao)
    ValueLabel.TextColor3 = Color3.fromRGB(235, 35, 35)
    ValueLabel.TextSize = 12
    ValueLabel.Font = Enum.Font.GothamBold
    ValueLabel.TextXAlignment = Enum.TextXAlignment.Right
    ValueLabel.Parent = Container

    local SliderBg = Instance.new("TextButton")
    SliderBg.Size = UDim2.new(1, -24, 0, 6)
    SliderBg.Position = UDim2.new(0, 12, 0, 38)
    SliderBg.BackgroundColor3 = Color3.fromRGB(45, 45, 50)
    SliderBg.Text = ""
    SliderBg.Parent = Container

    local SliderCorner = Instance.new("UICorner")
    SliderCorner.CornerRadius = UDim.new(1, 0)
    SliderCorner.Parent = SliderBg

    local SliderFill = Instance.new("Frame")
    SliderFill.Size = UDim2.new((padrao - min) / (max - min), 0, 1, 0)
    SliderFill.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
    SliderFill.BorderSizePixel = 0
    SliderFill.Parent = SliderBg

    local FillCorner = Instance.new("UICorner")
    FillCorner.CornerRadius = UDim.new(1, 0)
    FillCorner.Parent = SliderFill

    local arrastando = false
    local function atualizar(input)
        local posX = math.clamp((input.Position.X - SliderBg.AbsolutePosition.X) / SliderBg.AbsoluteSize.X, 0, 1)
        SliderFill.Size = UDim2.new(posX, 0, 1, 0)
        local valor = math.floor(min + (posX * (max - min)))
        ValueLabel.Text = tostring(valor)
        callback(valor)
    end

    SliderBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            arrastando = true
            atualizar(input)
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if arrastando and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            atualizar(input)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            arrastando = false
        end
    end)
end

-- ==========================================
-- LÓGICA DO ESP (MANTIDA NO VISUAL 👁️)
-- ==========================================
_G.ESPAnomalias = false

local function checarSeEhMonstro(npc)
    if npc:GetAttribute("Skinwalker") == true or npc:GetAttribute("CameraEffect") ~= nil or npc:GetAttribute("HasCameraEffect") == true then
        return true
    end
    if npc:FindFirstChild("Skinwalker") or npc:FindFirstChild("CameraEffect") or npc:FindFirstChild("HasCameraEffect") then
        return true
    end
    return false
end

local function aplicarESPPuro(objeto)
    if objeto:IsA("Model") and objeto.Name ~= LocalPlayer.Name then
        local root = objeto:FindFirstChild("HumanoidRootPart") or objeto:FindFirstChild("Torso")
        if root then
            local ehMonstro = checarSeEhMonstro(objeto)
            local corFinal = ehMonstro and Color3.fromRGB(235, 35, 35) or Color3.fromRGB(35, 235, 35)
            local textoFinal = ehMonstro and "Anomalia" or "Patient"

            local highlight = objeto:FindFirstChild("EspHighlightPuro")
            if not highlight then
                highlight = Instance.new("Highlight")
                highlight.Name = "EspHighlightPuro"
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.OutlineTransparency = 0.2
                highlight.FillTransparency = 0.4
                highlight.Adornee = objeto
                highlight.Parent = objeto
            end
            highlight.FillColor = corFinal

            local billboard = root:FindFirstChild("EspTextoPuro")
            if not billboard then
                billboard = Instance.new("BillboardGui")
                billboard.Name = "EspTextoPuro"
                billboard.Size = UDim2.new(0, 120, 0, 30)
                billboard.AlwaysOnTop = true
                billboard.Adornee = root
                billboard.StudsOffset = Vector3.new(0, 3.5, 0)
                billboard.Parent = root

                local tag = Instance.new("TextLabel")
                tag.Name = "TagTexto"
                tag.Size = UDim2.new(1, 0, 1, 0)
                tag.BackgroundTransparency = 1
                tag.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
                tag.TextStrokeTransparency = 0
                tag.TextSize = 14
                tag.Font = Enum.Font.GothamBold
                tag.Parent = billboard
            end
            
            local tagTexto = billboard:FindFirstChild("TagTexto")
            if tagTexto then
                tagTexto.Text = textoFinal
                tagTexto.TextColor3 = corFinal
            end
        end
    end
end

local function limparESP()
    for _, v in pairs(workspace:GetDescendants()) do
        if v.Name == "EspHighlightPuro" or v.Name == "EspTextoPuro" then
            v:Destroy()
        end
    end
end

task.spawn(function()
    while true do
        task.wait(1)
        if _G.ESPAnomalias then
            pcall(function()
                for _, obj in pairs(workspace:GetChildren()) do
                    if not _G.ESPAnomalias then break end
                    if obj:IsA("Model") and obj:FindFirstChildOfClass("Humanoid") and obj.Name ~= LocalPlayer.Name then
                        aplicarESPPuro(obj)
                    end
                end
            end)
        else
            limparESP()
        end
    end
end)

criarToggle(AbaVisual, "Ativar ESP (Verde/Vermelho)", function(val)
    _G.ESPAnomalias = val
end)

-- ==========================================
-- SISTEMA DA ABA HOSPITAL 🏥 (DINÂMICO E COMPLETO)
-- ==========================================

-- Container de rolagem das Listas dentro da Aba Hospital
local HospitalScroll = Instance.new("Frame")
HospitalScroll.Size = UDim2.new(1, -10, 1, 0)
HospitalScroll.BackgroundTransparency = 1
HospitalScroll.Parent = AbaHospital

local HospLayout = Instance.new("UIListLayout")
HospLayout.Padding = UDim.new(0, 10)
HospLayout.Parent = HospitalScroll

-- Cabeçalho / Botão de Atualizar
local HeaderFrame = Instance.new("Frame")
HeaderFrame.Size = UDim2.new(1, 0, 0, 35)
HeaderFrame.BackgroundTransparency = 1
HeaderFrame.Parent = HospitalScroll

local TitleHosp = Instance.new("TextLabel")
TitleHosp.Size = UDim2.new(1, -110, 1, 0)
TitleHosp.BackgroundTransparency = 1
TitleHosp.Text = "SISTEMA DE TRATAMENTO"
TitleHosp.TextColor3 = Color3.fromRGB(245, 245, 245)
TitleHosp.TextSize = 12
TitleHosp.Font = Enum.Font.GothamBold
TitleHosp.TextXAlignment = Enum.TextXAlignment.Left
TitleHosp.Parent = HeaderFrame

local RefreshBtn = Instance.new("TextButton")
RefreshBtn.Size = UDim2.new(0, 100, 1, 0)
RefreshBtn.Position = UDim2.new(1, -100, 0, 0)
RefreshBtn.BackgroundColor3 = Color3.fromRGB(235, 35, 35)
RefreshBtn.Text = "Atualizar 🔄"
RefreshBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
RefreshBtn.TextSize = 11
RefreshBtn.Font = Enum.Font.GothamBold
RefreshBtn.Parent = HeaderFrame

local RefCorner = Instance.new("UICorner")
RefCorner.CornerRadius = UDim.new(0, 8)
RefCorner.Parent = RefreshBtn

-- Listas Visuais
local PatientsContainer = Instance.new("Frame")
PatientsContainer.Size = UDim2.new(1, 0, 0, 90)
PatientsContainer.BackgroundColor3 = Color3.fromRGB(20, 20, 24)
PatientsContainer.Parent = HospitalScroll

local PatCorner = Instance.new("UICorner")
PatCorner.CornerRadius = UDim.new(0, 10)
PatCorner.Parent = PatientsContainer

local PatTitle = Instance.new("TextLabel")
PatTitle.Size = UDim2.new(1, -10, 0, 25)
PatTitle.Position = UDim2.new(0, 10, 0, 3)
PatTitle.BackgroundTransparency = 1
PatTitle.Text = "Pacientes Ativos (Coleta de DNA)"
PatTitle.TextColor3 = Color3.fromRGB(235, 35, 35)
PatTitle.TextSize = 11
PatTitle.Font = Enum.Font.GothamBold
PatTitle.TextXAlignment = Enum.TextXAlignment.Left
PatTitle.Parent = PatientsContainer

local PatScroll = Instance.new("ScrollingFrame")
PatScroll.Size = UDim2.new(1, -10, 1, -30)
PatScroll.Position = UDim2.new(0, 5, 0, 28)
PatScroll.BackgroundTransparency = 1
PatScroll.CanvasSize = UDim2.new(0, 0, 2, 0)
PatScroll.ScrollBarThickness = 2
PatScroll.Parent = PatientsContainer

local PatLayout = Instance.new("UIListLayout")
PatLayout.Padding = UDim.new(0, 5)
PatLayout.Parent = PatScroll

-- Listas de Computadores e Dispositivos
local MachinesContainer = Instance.new("Frame")
MachinesContainer.Size = UDim2.new(1, 0, 0, 110)
MachinesContainer.BackgroundColor3 = Color3.fromRGB(20, 20, 24)
MachinesContainer.Parent = HospitalScroll

local MacCorner = Instance.new("UICorner")
MacCorner.CornerRadius = UDim.new(0, 10)
MacCorner.Parent = MachinesContainer

local MacTitle = Instance.new("TextLabel")
MacTitle.Size = UDim2.new(1, -10, 0, 25)
MacTitle.Position = UDim2.new(0, 10, 0, 3)
MacTitle.BackgroundTransparency = 1
MacTitle.Text = "Computadores & Exames Detectados"
MacTitle.TextColor3 = Color3.fromRGB(235, 35, 35)
MacTitle.TextSize = 11
MacTitle.Font = Enum.Font.GothamBold
MacTitle.TextXAlignment = Enum.TextXAlignment.Left
MacTitle.Parent = MachinesContainer

local MacScroll = Instance.new("ScrollingFrame")
MacScroll.Size = UDim2.new(1, -10, 1, -30)
MacScroll.Position = UDim2.new(0, 5, 0, 28)
MacScroll.BackgroundTransparency = 1
MacScroll.CanvasSize = UDim2.new(0, 0, 2, 0)
MacScroll.ScrollBarThickness = 2
MacScroll.Parent = MachinesContainer

local MacLayout = Instance.new("UIListLayout")
MacLayout.Padding = UDim.new(0, 5)
MacLayout.Parent = MacScroll

-- FUNÇÕES DE TELEPORTE
local function teleportarPara(targetInstance)
    local char = LocalPlayer.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        local targetCF = nil
        if targetInstance:IsA("Model") then
            targetCF = targetInstance:GetPivot()
        elseif targetInstance:IsA("BasePart") then
            targetCF = targetInstance.CFrame
        end
        if targetCF then
            char.HumanoidRootPart.CFrame = targetCF * CFrame.new(0, 2, 0)
        end
    end
end

-- CARREGA OS ITENS ATIVOS NA INTERFACE
local function RecarregarHospital()
    -- Limpa listas antigas
    for _, item in pairs(PatScroll:GetChildren()) do
        if not item:IsA("UIListLayout") then item:Destroy() end
    end
    for _, item in pairs(MacScroll:GetChildren()) do
        if not item:IsA("UIListLayout") then item:Destroy() end
    end

    -- 1. Popula Pacientes Humanos (que não são anomalias)
    for _, obj in pairs(workspace:GetChildren()) do
        if obj:IsA("Model") and obj:FindFirstChildOfClass("Humanoid") and obj.Name ~= LocalPlayer.Name then
            if not checarSeEhMonstro(obj) then
                local Card = Instance.new("Frame")
                Card.Size = UDim2.new(1, -10, 0, 30)
                Card.BackgroundColor3 = Color3.fromRGB(28, 28, 33)
                Card.Parent = PatScroll

                local CardCorner = Instance.new("UICorner")
                CardCorner.CornerRadius = UDim.new(0, 6)
                CardCorner.Parent = Card

                local NameLbl = Instance.new("TextLabel")
                NameLbl.Size = UDim2.new(1, -90, 1, 0)
                NameLbl.Position = UDim2.new(0, 10, 0, 0)
                NameLbl.BackgroundTransparency = 1
                NameLbl.Text = "👤 Paciente: " .. obj.Name
                NameLbl.TextColor3 = Color3.fromRGB(220, 220, 220)
                NameLbl.TextSize = 11
                NameLbl.Font = Enum.Font.GothamMedium
                NameLbl.TextXAlignment = Enum.TextXAlignment.Left
                NameLbl.Parent = Card

                local TpBtn = Instance.new("TextButton")
                TpBtn.Size = UDim2.new(0, 80, 0, 22)
                TpBtn.Position = UDim2.new(1, -85, 0.5, -11)
                TpBtn.BackgroundColor3 = Color3.fromRGB(35, 150, 35)
                TpBtn.Text = "Teleportar"
                TpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
                TpBtn.TextSize = 10
                TpBtn.Font = Enum.Font.GothamBold
                TpBtn.Parent = Card

                local TpCorner = Instance.new("UICorner")
                TpCorner.CornerRadius = UDim.new(0, 5)
                TpCorner.Parent = TpBtn

                TpBtn.MouseButton1Click:Connect(function()
                    teleportarPara(obj)
                end)
            end
        end
    end

    -- 2. Varredura dinâmica de Computadores, Bancadas e Dispensadores
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("Model") or obj:IsA("BasePart") then
            local nomeObj = string.lower(obj.Name)
            local ehComputador = string.find(nomeObj, "computador") or string.find(nomeObj, "computer") or string.find(nomeObj, "pc")
            local ehDnaRemedio = string.find(nomeObj, "dna") or string.find(nomeObj, "remedio") or string.find(nomeObj, "medicamento") or string.find(nomeObj, "medicine") or string.find(nomeObj, "dispenser")

            if ehComputador or ehDnaRemedio then
                local prefixo = ehComputador and "🖥️ " or "🧪 "
                local corBtn = ehComputador and Color3.fromRGB(35, 120, 200) or Color3.fromRGB(200, 120, 35)

                local Card = Instance.new("Frame")
                Card.Size = UDim2.new(1, -10, 0, 30)
                Card.BackgroundColor3 = Color3.fromRGB(28, 28, 33)
                Card.Parent = MacScroll

                local CardCorner = Instance.new("UICorner")
                CardCorner.CornerRadius = UDim.new(0, 6)
                CardCorner.Parent = Card

                local NameLbl = Instance.new("TextLabel")
                NameLbl.Size = UDim2.new(1, -90, 1, 0)
                NameLbl.Position = UDim2.new(0, 10, 0, 0)
                NameLbl.BackgroundTransparency = 1
                NameLbl.Text = prefixo .. obj.Name
                NameLbl.TextColor3 = Color3.fromRGB(220, 220, 220)
                NameLbl.TextSize = 11
                NameLbl.Font = Enum.Font.GothamMedium
                NameLbl.TextXAlignment = Enum.TextXAlignment.Left
                NameLbl.Parent = Card

                local TpBtn = Instance.new("TextButton")
                TpBtn.Size = UDim2.new(0, 80, 0, 22)
                TpBtn.Position = UDim2.new(1, -85, 0.5, -11)
                TpBtn.BackgroundColor3 = corBtn
                TpBtn.Text = "Teleportar"
                TpBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
                TpBtn.TextSize = 10
                TpBtn.Font = Enum.Font.GothamBold
                TpBtn.Parent = Card

                local TpCorner = Instance.new("UICorner")
                TpCorner.CornerRadius = UDim.new(0, 5)
                TpCorner.Parent = TpBtn

                TpBtn.MouseButton1Click:Connect(function()
                    teleportarPara(obj)
                end)
            end
        end
    end
end

RefreshBtn.MouseButton1Click:Connect(RecarregarHospital)
task.spawn(RecarregarHospital) -- Carrega no início do script

-- ==========================================
-- LÓGICA LOCAL PLAYER (Velocidade & Pulo)
-- ==========================================
local padraoSpeed = 16
local padraoJump = 50

local function getHumanoid()
    local char = LocalPlayer.Character
    return char and char:FindFirstChildOfClass("Humanoid")
end

criarSlider(AbaLocalPlayer, "Velocidade", 16, 250, 16, function(val)
    padraoSpeed = val
    local hum = getHumanoid()
    if hum then hum.WalkSpeed = val end
end)

criarSlider(AbaLocalPlayer, "Pulo", 50, 350, 50, function(val)
    padraoJump = val
    local hum = getHumanoid()
    if hum then 
        hum.JumpPower = val 
        hum.UseJumpPower = true
    end
end)

LocalPlayer.CharacterAdded:Connect(function(char)
    local hum = char:WaitForChild("Humanoid", 5)
    if hum then
        hum.WalkSpeed = padraoSpeed
        hum.JumpPower = padraoJump
        hum.UseJumpPower = true
    end
end)

-- ==========================================
-- FLY DURO (Estilo Infinite Yield)
-- ==========================================
local FlyAtivo = false
local FlyVel = 50
local flyConnection = nil

local function IniciarFly()
    local char = LocalPlayer.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hrp or not hum then return end

    hrp.Velocity = Vector3.new(0, 0, 0)

    local bg = Instance.new("BodyGyro")
    bg.Name = "IY_Gyro"
    bg.P = 9e4
    bg.maxTorque = Vector3.new(9e9, 9e9, 9e9)
    bg.cframe = hrp.CFrame
    bg.Parent = hrp

    local bv = Instance.new("BodyVelocity")
    bv.Name = "IY_Velocity"
    bv.velocity = Vector3.new(0, 0, 1)
    bv.maxForce = Vector3.new(9e9, 9e9, 9e9)
    bv.Parent = hrp

    hum.PlatformStand = true

    flyConnection = RunService.RenderStepped:Connect(function()
        if not FlyAtivo or not hrp or not hum then 
            if flyConnection then flyConnection:Disconnect() end
            return 
        end

        local camera = workspace.CurrentCamera
        local direcao = Vector3.new(0, 0, 0)

        if UserInputService:IsKeyDown(Enum.KeyCode.W) then
            direcao = direcao + camera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then
            direcao = direcao - camera.CFrame.LookVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then
            direcao = direcao - camera.CFrame.RightVector
        end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then
            direcao = direcao + camera.CFrame.RightVector
        end

        bg.cframe = camera.CFrame

        if direcao.Magnitude > 0 then
            bv.velocity = direcao.Unit * FlyVel
        else
            bv.velocity = Vector3.new(0, 0, 0)
        end
    end)
end

local function PararFly()
    local char = LocalPlayer.Character
    if char then
        local hrp = char:FindFirstChild("HumanoidRootPart")
        local hum = char:FindFirstChildOfClass("Humanoid")
        if hrp then
            local bg = hrp:FindFirstChild("IY_Gyro")
            local bv = hrp:FindFirstChild("IY_Velocity")
            if bg then bg:Destroy() end
            if bv then bv:Destroy() end
        end
        if hum then
            hum.PlatformStand = false
        end
    end
    if flyConnection then
        flyConnection:Disconnect()
        flyConnection = nil
    end
end

criarToggle(AbaLocalPlayer, "Ativar Fly (Tecla E)", function(val)
    FlyAtivo = val
    if FlyAtivo then
        IniciarFly()
    else
        PararFly()
    end
end)

UserInputService.InputBegan:Connect(function(input, processed)
    if processed then return end
    if input.KeyCode == Enum.KeyCode.E then
        FlyAtivo = not FlyAtivo
        if FlyAtivo then
            IniciarFly()
        else
            PararFly()
        end
    end
end)

criarSlider(AbaLocalPlayer, "Velocidade do Fly", 10, 300, 50, function(val)
    FlyVel = val
end)

-- ==========================================
-- ABA SOBRE (CRIADOR)
-- ==========================================
local FrameSobre = Instance.new("Frame")
FrameSobre.Size = UDim2.new(1, -10, 0, 80)
FrameSobre.BackgroundColor3 = Color3.fromRGB(20, 20, 24)
FrameSobre.Parent = AbaSobre

local SobreCorner = Instance.new("UICorner")
SobreCorner.CornerRadius = UDim.new(0, 10)
SobreCorner.Parent = FrameSobre

local DevLabel = Instance.new("TextLabel")
DevLabel.Size = UDim2.new(1, 0, 0, 30)
DevLabel.Position = UDim2.new(0, 12, 0, 10)
DevLabel.BackgroundTransparency = 1
DevLabel.Text = "Criador: BIEL"
DevLabel.TextColor3 = Color3.fromRGB(235, 35, 35)
DevLabel.TextSize = 16
DevLabel.Font = Enum.Font.GothamBold
DevLabel.TextXAlignment = Enum.TextXAlignment.Left
DevLabel.Parent = FrameSobre

local DescLabel = Instance.new("TextLabel")
DescLabel.Size = UDim2.new(1, -24, 0, 30)
DescLabel.Position = UDim2.new(0, 12, 0, 40)
DescLabel.BackgroundTransparency = 1
DescLabel.Text = "Script atualizado com interface premium de bordas ultra arredondadas, Fly rígido estilo IY e abas dedicadas para o seu jogo."
DescLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
DescLabel.TextSize = 11
DescLabel.Font = Enum.Font.Gotham
DescLabel.TextWrapped = true
DescLabel.TextXAlignment = Enum.TextXAlignment.Left
DescLabel.Parent = FrameSobre
