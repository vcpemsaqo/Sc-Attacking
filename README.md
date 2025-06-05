--System Center | ESP + Aimbot com FOV + Auto Revistar + Auto Quit + Noclip 
-- Ignora mortos, mira no mais próximo, funciona com botão direito

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-- CONFIGURAÇÕES
getgenv().espEnabled = true
getgenv().aimbotEnabled = true
getgenv().fovRadius = 120
getgenv().autoRevistarEnabled = false
getgenv().revistarKey = Enum.KeyCode.E
getgenv().autoQuitEnabled = true -- Ativa auto quit
getgenv().quitHealthThreshold = 10 -- Porcentagem da vida para sair do jogo
getgenv().noclipEnabled = false -- Noclip desligado por padrão
getgenv().noclipToggleKey = Enum.KeyCode.N -- Tecla padrão para toggle noclip

local holding = false

-- INTERFACE
local gui = Instance.new("ScreenGui")
gui.Name = "SystemCenterUI"
gui.Parent = game.CoreGui
gui.ResetOnSpawn = false

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 300, 0, 360) -- Largura aumentada para caber novo campo
frame.Position = UDim2.new(0, 20, 0, 20)
frame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(0, 255, 0)
title.Text = "System Center"
title.TextSize = 22

local minimizeBtn = Instance.new("TextButton", frame)
minimizeBtn.Size = UDim2.new(0, 30, 0, 30)
minimizeBtn.Position = UDim2.new(1, -35, 0, 0)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
minimizeBtn.BorderSizePixel = 0
minimizeBtn.Font = Enum.Font.Code
minimizeBtn.TextSize = 24
minimizeBtn.TextColor3 = Color3.new(1,1,1)
minimizeBtn.Text = "−"
minimizeBtn.AutoButtonColor = false

minimizeBtn.MouseEnter:Connect(function()
	minimizeBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
end)
minimizeBtn.MouseLeave:Connect(function()
	minimizeBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
end)

local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
	minimized = not minimized
	if minimized then
		for _, child in ipairs(frame:GetChildren()) do
			if child ~= title and child ~= minimizeBtn then
				child.Visible = false
			end
		end
		minimizeBtn.Text = "+"
		frame.Size = UDim2.new(0, 300, 0, 30)
	else
		for _, child in ipairs(frame:GetChildren()) do
			child.Visible = true
		end
		minimizeBtn.Text = "−"
		frame.Size = UDim2.new(0, 300, 0, 360)
	end
end)

-- Botão ESP
local espBtn = Instance.new("TextButton", frame)
espBtn.Size = UDim2.new(1, -20, 0, 30)
espBtn.Position = UDim2.new(0, 10, 0, 40)
espBtn.BackgroundColor3 = Color3.fromRGB(20, 120, 20)
espBtn.TextColor3 = Color3.new(1,1,1)
espBtn.Font = Enum.Font.Gotham
espBtn.TextSize = 16
espBtn.Text = "ESP: ON"

espBtn.MouseButton1Click:Connect(function()
	getgenv().espEnabled = not getgenv().espEnabled
	espBtn.Text = getgenv().espEnabled and "ESP: ON" or "ESP: OFF"
end)

-- Botão Aimbot
local aimBtn = Instance.new("TextButton", frame)
aimBtn.Size = UDim2.new(1, -20, 0, 30)
aimBtn.Position = UDim2.new(0, 10, 0, 80)
aimBtn.BackgroundColor3 = Color3.fromRGB(20, 120, 20)
aimBtn.TextColor3 = Color3.new(1,1,1)
aimBtn.Font = Enum.Font.Gotham
aimBtn.TextSize = 16
aimBtn.Text = "Aimbot: ON"

aimBtn.MouseButton1Click:Connect(function()
	getgenv().aimbotEnabled = not getgenv().aimbotEnabled
	aimBtn.Text = getgenv().aimbotEnabled and "Aimbot: ON" or "Aimbot: OFF"
end)

-- Botão Auto Revistar
local revBtn = Instance.new("TextButton", frame)
revBtn.Size = UDim2.new(1, -20, 0, 30)
revBtn.Position = UDim2.new(0, 10, 0, 120)
revBtn.BackgroundColor3 = Color3.fromRGB(20, 120, 20)
revBtn.TextColor3 = Color3.new(1,1,1)
revBtn.Font = Enum.Font.Gotham
revBtn.TextSize = 16
revBtn.Text = "Auto Revistar: OFF"

revBtn.MouseButton1Click:Connect(function()
	getgenv().autoRevistarEnabled = not getgenv().autoRevistarEnabled
	revBtn.Text = getgenv().autoRevistarEnabled and "Auto Revistar: ON" or "Auto Revistar: OFF"
end)

-- Botão Auto Quit
local quitBtn = Instance.new("TextButton", frame)
quitBtn.Size = UDim2.new(1, -20, 0, 30)
quitBtn.Position = UDim2.new(0, 10, 0, 160)
quitBtn.BackgroundColor3 = Color3.fromRGB(120, 20, 20)
quitBtn.TextColor3 = Color3.new(1,1,1)
quitBtn.Font = Enum.Font.Gotham
quitBtn.TextSize = 16
quitBtn.Text = "Auto Quit: ON"

quitBtn.MouseButton1Click:Connect(function()
	getgenv().autoQuitEnabled = not getgenv().autoQuitEnabled
	quitBtn.Text = getgenv().autoQuitEnabled and "Auto Quit: ON" or "Auto Quit: OFF"
end)

-- Botão Noclip
local noclipBtn = Instance.new("TextButton", frame)
noclipBtn.Size = UDim2.new(0, 140, 0, 30)
noclipBtn.Position = UDim2.new(0, 10, 0, 320)
noclipBtn.BackgroundColor3 = Color3.fromRGB(20, 120, 20)
noclipBtn.TextColor3 = Color3.new(1,1,1)
noclipBtn.Font = Enum.Font.Gotham
noclipBtn.TextSize = 16
noclipBtn.Text = "Noclip: OFF"

noclipBtn.MouseButton1Click:Connect(function()
	getgenv().noclipEnabled = not getgenv().noclipEnabled
	noclipBtn.Text = getgenv().noclipEnabled and "Noclip: ON" or "Noclip: OFF"
end)

-- Caixa para definir a tecla de toggle do noclip
local noclipKeyBox = Instance.new("TextBox", frame)
noclipKeyBox.Size = UDim2.new(0, 140, 0, 30)
noclipKeyBox.Position = UDim2.new(0, 160, 0, 320)
noclipKeyBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
noclipKeyBox.TextColor3 = Color3.new(1,1,1)
noclipKeyBox.Font = Enum.Font.Gotham
noclipKeyBox.TextSize = 16
noclipKeyBox.PlaceholderText = "Tecla Noclip"
noclipKeyBox.Text = tostring(getgenv().noclipToggleKey.Name) or "N"

noclipKeyBox.FocusLost:Connect(function(enterPressed)
	if enterPressed then
		local keyName = noclipKeyBox.Text
		local success, keyEnum = pcall(function()
			return Enum.KeyCode[keyName]
		end)
		if success and keyEnum then
			getgenv().noclipToggleKey = keyEnum
			noclipKeyBox.Text = keyEnum.Name
		else
			noclipKeyBox.Text = tostring(getgenv().noclipToggleKey.Name)
		end
	end
end)

-- Campo de Entrada para Threshold (Auto Quit)
local thresholdLabel = Instance.new("TextLabel", frame)
thresholdLabel.Size = UDim2.new(1, -20, 0, 20)
thresholdLabel.Position = UDim2.new(0, 10, 0, 200)
thresholdLabel.BackgroundTransparency = 1
thresholdLabel.Font = Enum.Font.Gotham
thresholdLabel.TextSize = 14
thresholdLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
thresholdLabel.Text = "Vida para Auto Quit (%)"

local thresholdBox = Instance.new("TextBox", frame)
thresholdBox.Size = UDim2.new(1, -20, 0, 30)
thresholdBox.Position = UDim2.new(0, 10, 0, 220)
thresholdBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
thresholdBox.TextColor3 = Color3.new(1,1,1)
thresholdBox.Font = Enum.Font.Gotham
thresholdBox.TextSize = 16
thresholdBox.PlaceholderText = "10"
thresholdBox.Text = tostring(getgenv().quitHealthThreshold)

thresholdBox.FocusLost:Connect(function(enterPressed)
	if enterPressed then
		local val = tonumber(thresholdBox.Text)
		if val and val >= 0 and val <= 100 then
			getgenv().quitHealthThreshold = val
		else
			thresholdBox.Text = tostring(getgenv().quitHealthThreshold)
		end
	end
end)

-- Campo para FOV do Aimbot
local fovLabel = Instance.new("TextLabel", frame)
fovLabel.Size = UDim2.new(1, -20, 0, 20)
fovLabel.Position = UDim2.new(0, 10, 0, 260)
fovLabel.BackgroundTransparency = 1
fovLabel.Font = Enum.Font.Gotham
fovLabel.TextSize = 14
fovLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
fovLabel.Text = "FOV do Aimbot (px)"

local fovBox = Instance.new("TextBox", frame)
fovBox.Size = UDim2.new(1, -20, 0, 30)
fovBox.Position = UDim2.new(0, 10, 0, 280)
fovBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
fovBox.TextColor3 = Color3.new(1,1,1)
fovBox.Font = Enum.Font.Gotham
fovBox.TextSize = 16
fovBox.PlaceholderText = "120"
fovBox.Text = tostring(getgenv().fovRadius)

fovBox.FocusLost:Connect(function(enterPressed)
	if enterPressed then
		local val = tonumber(fovBox.Text)
		if val and val > 10 and val <= 500 then
			getgenv().fovRadius = val
		else
			fovBox.Text = tostring(getgenv().fovRadius)
		end
	end
end)

-- Circulo de FOV no mouse
local fovCircle = Drawing.new("Circle")
fovCircle.Radius = getgenv().fovRadius
fovCircle.Color = Color3.fromRGB(0, 255, 0)
fovCircle.Thickness = 1
fovCircle.Filled = false
fovCircle.Transparency = 0.6

-- Função para verificar se o personagem está vivo
local function isAlive(plr)
	local character = plr.Character
	if character and character:FindFirstChild("Humanoid") then
		return character.Humanoid.Health > 0
	end
	return false
end

-- ESP
local espBoxes = {}
local espNames = {}

local function createESP(plr)
	if espBoxes[plr] then return end
	local box = Drawing.new("Square")
	box.Color = Color3.fromRGB(0, 255, 0)
	box.Thickness = 1
	box.Filled = false
	espBoxes[plr] = box

	local nameTag = Drawing.new("Text")
	nameTag.Color = Color3.fromRGB(0, 255, 0)
	nameTag.Size = 16
	nameTag.Center = true
	nameTag.Outline = true
	nameTag.Font = 3
	espNames[plr] = nameTag
end

local function removeESP(plr)
	if espBoxes[plr] then
		espBoxes[plr]:Remove()
		espBoxes[plr] = nil
	end
	if espNames[plr] then
		espNames[plr]:Remove()
		espNames[plr] = nil
	end
end

-- Auto Revistar (exemplo simples: imprime "Revistando" no console)
local function revistar()
	print("Revistando...")
	-- Aqui você pode adicionar o código real para a ação de revistar
end

-- Auto Quit
local function checkQuit()
	local character = LocalPlayer.Character
	if character and character:FindFirstChild("Humanoid") then
		local humanoid = character.Humanoid
		local healthPercent = (humanoid.Health / humanoid.MaxHealth) * 100
		if getgenv().autoQuitEnabled and healthPercent <= getgenv().quitHealthThreshold then
			print("Vida abaixo do limite, saindo do jogo...")
			game:Shutdown() -- Fecha o jogo
		end
	end
end

-- Noclip
local function noclipOn()
	local character = LocalPlayer.Character
	if character then
		for _, part in ipairs(character:GetChildren()) do
			if part:IsA("BasePart") and part.CanCollide == true then
				part.CanCollide = false
			end
		end
	end
end

local function noclipOff()
	local character = LocalPlayer.Character
	if character then
		for _, part in ipairs(character:GetChildren()) do
			if part:IsA("BasePart") and part.CanCollide == false then
				part.CanCollide = true
			end
		end
	end
end

-- Toggle noclip ao pressionar tecla
UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == getgenv().noclipToggleKey then
		getgenv().noclipEnabled = not getgenv().noclipEnabled
		if getgenv().noclipEnabled then
			noclipBtn.Text = "Noclip: ON"
		else
			noclipBtn.Text = "Noclip: OFF"
		end
	end
end)

-- Aimbot: função para pegar o jogador mais próximo dentro do FOV
local function getClosestPlayer()
	local mousePos = UIS:GetMouseLocation()
	local closestPlayer = nil
	local shortestDist = math.huge
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and isAlive(plr) and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			local rootPart = plr.Character.HumanoidRootPart
			local screenPos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
			if onScreen then
				local dist = (Vector2.new(screenPos.X, screenPos.Y) - Vector2.new(mousePos.X, mousePos.Y)).Magnitude
				if dist < shortestDist and dist <= getgenv().fovRadius then
					closestPlayer = plr
					shortestDist = dist
				end
			end
		end
	end
	return closestPlayer
end

-- Mover o mouse para o jogador alvo (usando mousemoverel para simular movimento suave)
local UserInputService = UIS
local function aimAtPlayer(plr)
	if not plr or not plr.Character then return end
	local rootPart = plr.Character:FindFirstChild("HumanoidRootPart")
	if not rootPart then return end
	local rootPos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
	if not onScreen then return end

	local mousePos = UserInputService:GetMouseLocation()
	-- Calcula o delta do mouse para o ponto do inimigo
	local delta = Vector2.new(rootPos.X, rootPos.Y) - Vector2.new(mousePos.X, mousePos.Y)

	-- Aqui você pode fazer um movimento mais suave, ou simplesmente setar o mouse na posição
	-- Para simplicidade, vamos mover o mouse um pouco em direção ao alvo (ajuste speed)
	local speed = 0.5 -- Ajuste de velocidade do aimbot, menor é mais lento
	mousemoverel(delta.X * speed, delta.Y * speed)
end

-- Manipulação do botão direito para ativar aimbot
UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.UserInputType == Enum.UserInputType.MouseButton2 then
		holding = true
	end
end)

UIS.InputEnded:Connect(function(input, gameProcessed)
	if input.UserInputType == Enum.UserInputType.MouseButton2 then
		holding = false
	end
end)

-- Auto Revistar com tecla
UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == getgenv().revistarKey then
		revistar()
	end
end)

-- Atualização da ESP e lógica principal
RunService.RenderStepped:Connect(function()
	-- Atualiza circulo FOV
	fovCircle.Radius = getgenv().fovRadius
	fovCircle.Position = Vector2.new(UIS:GetMouseLocation().X, UIS:GetMouseLocation().Y)
	fovCircle.Visible = getgenv().aimbotEnabled

	-- ESP
	for _, plr in ipairs(Players:GetPlayers()) do
		if plr ~= LocalPlayer and isAlive(plr) and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
			if getgenv().espEnabled then
				createESP(plr)
				local rootPart = plr.Character.HumanoidRootPart
				local screenPos, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
				if onScreen then
					local box = espBoxes[plr]
					local nameTag = espNames[plr]

					-- Calcula box 2D (quadrado simples ao redor do rootPart)
					local size = 50 -- tamanho fixo para simplificação
					box.Position = Vector2.new(screenPos.X - size/2, screenPos.Y - size/2)
					box.Size = Vector2.new(size, size)
					box.Color = Color3.fromRGB(0,255,0)
					box.Visible = true

					nameTag.Position = Vector2.new(screenPos.X, screenPos.Y - size/2 - 16)
					nameTag.Text = plr.Name
					nameTag.Visible = true
				else
					removeESP(plr)
				end
			else
				removeESP(plr)
			end
		else
			removeESP(plr)
		end
	end

	-- Auto Quit
	checkQuit()

	-- Noclip
	if getgenv().noclipEnabled then
		noclipOn()
	else
		noclipOff()
	end

	-- Aimbot
	if getgenv().aimbotEnabled and holding then
		local target = getClosestPlayer()
		if target then
			aimAtPlayer(target)
		end
	end
end)
