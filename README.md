-- KN HUB feito por GPT e KN 😎🔥 (Versão Roxa)

local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "KNHub"
gui.ResetOnSpawn = false

-- 🟪 Janela principal (dragável)
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 450, 0, 300)
mainFrame.Position = UDim2.new(0.5, -225, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.fromRGB(50, 20, 70)
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true

-- 🟣 Menu lateral
local sideMenu = Instance.new("Frame", mainFrame)
sideMenu.Size = UDim2.new(0, 120, 1, 0)
sideMenu.Position = UDim2.new(0, 0, 0, 0)
sideMenu.BackgroundColor3 = Color3.fromRGB(70, 30, 90)

-- 🔮 Área de conteúdo
local contentFrame = Instance.new("Frame", mainFrame)
contentFrame.Size = UDim2.new(1, -120, 1, 0)
contentFrame.Position = UDim2.new(0, 120, 0, 0)
contentFrame.BackgroundColor3 = Color3.fromRGB(90, 40, 120)

-- 🔘 Botões do menu
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

-- 🔧 Aba: PushUps
local pushupTab = Instance.new("Frame", contentFrame)
pushupTab.Size = UDim2.new(1, 0, 1, 0)
pushupTab.Visible = true
pushupTab.BackgroundTransparency = 1

local pushButton = Instance.new("TextButton", pushupTab)
pushButton.Size = UDim2.new(0, 200, 0, 50)
pushButton.Position = UDim2.new(0.5, -100, 0.5, -25)
pushButton.BackgroundColor3 = Color3.fromRGB(170, 100, 255)
pushButton.Text = "Ativar PushUp Auto"
pushButton.TextColor3 = Color3.fromRGB(20, 0, 40)
pushButton.Font = Enum.Font.SourceSansBold
pushButton.TextScaled = true

local doingPush = false
pushButton.MouseButton1Click:Connect(function()
	doingPush = not doingPush
	pushButton.Text = doingPush and "Parar Auto PushUp" or "Ativar PushUp Auto"
	if doingPush then
		spawn(function()
			while doingPush do
				pcall(function()
					game:GetService("Players").LocalPlayer:WaitForChild("muscleEvent"):FireServer("pushup")
				end)
				wait(0.3)
			end
		end)
	end
end)

-- ✨ Aba: Créditos
local creditTab = Instance.new("Frame", contentFrame)
creditTab.Size = UDim2.new(1, 0, 1, 0)
creditTab.Visible = false
creditTab.BackgroundTransparency = 1

local creditText = Instance.new("TextLabel", creditTab)
creditText.Size = UDim2.new(1, -40, 0, 50)
creditText.Position = UDim2.new(0, 20, 0.4, 0)
creditText.BackgroundTransparency = 1
creditText.Text = "KN HUB - Feito por GPT e KN 😎🔥"
creditText.TextColor3 = Color3.fromRGB(255, 200, 255)
creditText.Font = Enum.Font.SourceSansBold
creditText.TextScaled = true

-- 🧭 Alternar abas
local function showTab(tab)
	pushupTab.Visible = false
	creditTab.Visible = false
	tab.Visible = true
end

pushupTabBtn.MouseButton1Click:Connect(function()
	showTab(pushupTab)
end)

creditTabBtn.MouseButton1Click:Connect(function()
	showTab(creditTab)
end)
