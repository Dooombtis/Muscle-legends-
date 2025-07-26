-- KN HUB v2 - Feito por GPT e KN 🟪 (com Slider e botão Fechar/Abrir)

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "KNHub"
gui.ResetOnSpawn = false

-- Variáveis globais
getgenv().autoPushEnabled = false
getgenv().repDelay = 0.05

-- 🟣 Botão para REABRIR a interface
local reopenBtn = Instance.new("TextButton", gui)
reopenBtn.Size = UDim2.new(0, 120, 0, 30)
reopenBtn.Position = UDim2.new(0, 10, 0, 10)
reopenBtn.Text = "Abrir KN Hub"
reopenBtn.Visible = false
reopenBtn.BackgroundColor3 = Color3.fromRGB(120, 80, 180)
reopenBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
reopenBtn.Font = Enum.Font.SourceSansBold
reopenBtn.TextScaled = true

-- 🪟 Janela principal
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 480, 0, 320)
mainFrame.Position = UDim2.new(0.5, -240, 0.5, -160)
mainFrame.BackgroundColor3 = Color3.fromRGB(50, 20, 70)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true

-- ❌ Botão de fechar
local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.Text = "X"
closeBtn.BackgroundColor3 = Color3.fromRGB(180, 60, 100)
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextScaled = true

closeBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
	reopenBtn.Visible = true
end)

reopenBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = true
	reopenBtn.Visible = false
end)

-- 🟪 Menu lateral
local sideMenu = Instance.new("Frame", mainFrame)
sideMenu.Size = UDim2.new(0, 120, 1, 0)
sideMenu.BackgroundColor3 = Color3.fromRGB(70, 30, 90)

-- 🔮 Conteúdo
local contentFrame = Instance.new("Frame", mainFrame)
contentFrame.Size = UDim2.new(1, -120, 1, 0)
contentFrame.Position = UDim2.new(0, 120, 0, 0)
contentFrame.BackgroundColor3 = Color3.fromRGB(90, 40, 120)

-- 🔘 Menu buttons
local function createMenuButton(text, posY)
	local btn = Instance.new("TextButton", sideMenu)
	btn.Size = UDim2.new(1, 0, 0, 40)
	btn.Position = UDim2.new(0, 0, 0, posY)
	btn.BackgroundColor3 = Color3.fromRGB(120, 60, 160)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.SourceSansBold
	btn.TextScaled = true
	btn.Text = text
	return btn
end

local pushupTabBtn = createMenuButton("PushUps", 10)
local creditTabBtn = createMenuButton("Créditos", 60)

-- 🧱 Abas
local pushupTab = Instance.new("Frame", contentFrame)
pushupTab.Size = UDim2.new(1, 0, 1, 0)
pushupTab.Visible = false
pushupTab.BackgroundTransparency = 1

local creditTab = Instance.new("Frame", contentFrame)
creditTab.Size = UDim2.new(1, 0, 1, 0)
creditTab.Visible = false
creditTab.BackgroundTransparency = 1

-- 🔄 Função auto-farm
local function autoPushLoop()
	while getgenv().autoPushEnabled do
		pcall(function()
			local args = { "rep" }
			player:WaitForChild("muscleEvent"):FireServer(unpack(args))
		end)
		task.wait(getgenv().repDelay)
	end
end

-- ▶️ Botão toggle
local toggleBtn = Instance.new("TextButton", pushupTab)
toggleBtn.Size = UDim2.new(0, 200, 0, 50)
toggleBtn.Position = UDim2.new(0.5, -100, 0.1, 0)
toggleBtn.Text = "Ativar Auto PushUps"
toggleBtn.BackgroundColor3 = Color3.fromRGB(140, 70, 200)
toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleBtn.Font = Enum.Font.SourceSansBold
toggleBtn.TextScaled = true

toggleBtn.MouseButton1Click:Connect(function()
	getgenv().autoPushEnabled = not getgenv().autoPushEnabled
	if getgenv().autoPushEnabled then
		toggleBtn.Text = "Desativar Auto PushUps"
		spawn(autoPushLoop)
	else
		toggleBtn.Text = "Ativar Auto PushUps"
	end
end)

-- 🎚️ Slider
local sliderLabel = Instance.new("TextLabel", pushupTab)
sliderLabel.Size = UDim2.new(0, 200, 0, 30)
sliderLabel.Position = UDim2.new(0.5, -100, 0.3, 0)
sliderLabel.Text = "Delay: 0.05s"
sliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
sliderLabel.Font = Enum.Font.SourceSans
sliderLabel.TextScaled = true
sliderLabel.BackgroundTransparency = 1

local sliderBar = Instance.new("Frame", pushupTab)
sliderBar.Size = UDim2.new(0, 200, 0, 8)
sliderBar.Position = UDim2.new(0.5, -100, 0.4, 0)
sliderBar.BackgroundColor3 = Color3.fromRGB(150, 100, 200)

local sliderKnob = Instance.new("Frame", sliderBar)
sliderKnob.Size = UDim2.new(0, 10, 1, 0)
sliderKnob.Position = UDim2.new((getgenv().repDelay - 0.01) / 0.49, 0, 0, 0)
sliderKnob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

local dragging = false
sliderKnob.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
	end
end)
sliderKnob.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local rel = input.Position.X - sliderBar.AbsolutePosition.X
		local percent = math.clamp(rel / sliderBar.AbsoluteSize.X, 0, 1)
		getgenv().repDelay = 0.01 + (0.49 * percent)
		sliderLabel.Text = string.format("Delay: %.2fs", getgenv().repDelay)
		sliderKnob.Position = UDim2.new(percent, -5, 0, 0)
	end
end)

-- 🖋 Créditos
local creditLabel = Instance.new("TextLabel", creditTab)
creditLabel.Size = UDim2.new(1, 0, 0, 40)
creditLabel.Position = UDim2.new(0, 0, 0.4, 0)
creditLabel.Text = "KN HUB - Feito por GPT e KN 💜"
creditLabel.BackgroundTransparency = 1
creditLabel.TextColor3 = Color3.fromRGB(255, 200, 255)
creditLabel.Font = Enum.Font.SourceSansBold
creditLabel.TextScaled = true

-- Alternar abas
local function showTab(tabName)
	pushupTab.Visible = false
	creditTab.Visible = false
	if tabName == "PushUps" then
		pushupTab.Visible = true
	elseif tabName == "Créditos" then
		creditTab.Visible = true
	end
end

pushupTabBtn.MouseButton1Click:Connect(function()
	showTab("PushUps")
end)

creditTabBtn.MouseButton1Click:Connect(function()
	showTab("Créditos")
end)

-- Mostrar PushUps por padrão
showTab("PushUps")
