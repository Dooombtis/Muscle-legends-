-- KN HUB • 日本風 🔮 -- Interface completa com tema roxo, abas "Auto Click" e "Créditos", auto-farm com delay ajustável, slider, samurai no fundo e transparência.

-- Proteção para evitar duplicatas if getgenv().KNHubInitialized then return end getgenv().KNHubInitialized = true

-- Variáveis globais getgenv().AutoClickEnabled = false getgenv().ClickDelay = 0.05

-- Criar GUI local player = game.Players.LocalPlayer local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui")) gui.Name = "KNHubUI" gui.ResetOnSpawn = false

-- TweenService para animações local TweenService = game:GetService("TweenService")

-- Frame principal local mainFrame = Instance.new("Frame", gui) mainFrame.Size = UDim2.new(0, 400, 0, 350) mainFrame.Position = UDim2.new(0.5, -200, 0.5, -175) mainFrame.BackgroundColor3 = Color3.fromRGB(60, 0, 90) mainFrame.BorderSizePixel = 0 mainFrame.Active = true mainFrame.Draggable = true mainFrame.BackgroundTransparency = 0.2

-- Samurai de fundo local samuraiImage = Instance.new("ImageLabel", mainFrame) samuraiImage.Size = UDim2.new(1, 0, 1, 0) samuraiImage.Position = UDim2.new(0, 0, 0, 0) samuraiImage.Image = "rbxassetid://14245947392" -- ID de exemplo samuraiImage.ImageTransparency = 0.7 samuraiImage.BackgroundTransparency = 1 samuraiImage.ImageColor3 = Color3.fromRGB(255, 60, 60) samuraiImage.ScaleType = Enum.ScaleType.Fit samuraiImage.ZIndex = 0

-- Título local title = Instance.new("TextLabel", mainFrame) title.Size = UDim2.new(1, 0, 0, 40) title.Position = UDim2.new(0, 0, 0, 0) title.BackgroundColor3 = Color3.fromRGB(90, 50, 130) title.Text = "🔮 KN HUB • 日本風 🔮" title.TextColor3 = Color3.fromRGB(255, 220, 255) title.Font = Enum.Font.Fantasy title.TextScaled = true title.BackgroundTransparency = 0.2 title.ZIndex = 2

-- Botão de fechar local closeBtn = Instance.new("TextButton", mainFrame) closeBtn.Size = UDim2.new(0, 30, 0, 30) closeBtn.Position = UDim2.new(1, -35, 0, 5) closeBtn.Text = "X" closeBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0) closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255) closeBtn.ZIndex = 3

-- Botão de reabrir local openBtn = Instance.new("TextButton", gui) openBtn.Size = UDim2.new(0, 100, 0, 40) openBtn.Position = UDim2.new(0, 20, 0, 20) openBtn.Text = "Abrir KN Hub" openBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 120) openBtn.TextColor3 = Color3.fromRGB(255, 255, 255) openBtn.Visible = true

-- Funções de animação local function animateOpen() mainFrame.Visible = true mainFrame.Size = UDim2.new(0, 0, 0, 0) mainFrame.Position = UDim2.new(0.5, 0, 0.5, 0) mainFrame.BackgroundTransparency = 1

local goal = {
    Size = UDim2.new(0, 400, 0, 350),
    Position = UDim2.new(0.5, -200, 0.5, -175),
    BackgroundTransparency = 0.2
}
local tween = TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), goal)
tween:Play()

end

local function animateClose() local goal = { Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0), BackgroundTransparency = 1 } local tween = TweenService:Create(mainFrame, TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.In), goal) tween:Play() tween.Completed:Connect(function() mainFrame.Visible = false end) end

-- Conectar botões à animação closeBtn.MouseButton1Click:Connect(animateClose) openBtn.MouseButton1Click:Connect(animateOpen)

-- Menus laterais e abas local sideMenu = Instance.new("Frame", mainFrame) sideMenu.Size = UDim2.new(0, 140, 1, -40) sideMenu.Position = UDim2.new(0, 0, 0, 40) sideMenu.BackgroundColor3 = Color3.fromRGB(80, 0, 130) sideMenu.BackgroundTransparency = 0.25 sideMenu.ZIndex = 2

local contentFrame = Instance.new("Frame", mainFrame) contentFrame.Size = UDim2.new(1, -140, 1, -40) contentFrame.Position = UDim2.new(0, 140, 0, 40) contentFrame.BackgroundColor3 = Color3.fromRGB(70, 0, 100) contentFrame.BackgroundTransparency = 0.25 contentFrame.ZIndex = 2

local function createMenuButton(name, yOffset) local btn = Instance.new("TextButton", sideMenu) btn.Size = UDim2.new(1, -10, 0, 40) btn.Position = UDim2.new(0, 5, 0, yOffset) btn.Text = name btn.BackgroundColor3 = Color3.fromRGB(120, 60, 160) btn.TextColor3 = Color3.fromRGB(255, 255, 255) btn.Font = Enum.Font.SourceSansBold btn.TextScaled = true btn.BackgroundTransparency = 0.15 return btn end

local autoClickTabBtn = createMenuButton("🌐 Auto Click", 10) local creditTabBtn = createMenuButton("🏣 Créditos", 60)

local autoClickTab = Instance.new("Frame", contentFrame) autoClickTab.Size = UDim2.new(1, 0, 1, 0) autoClickTab.BackgroundTransparency = 1

local toggleBtn = Instance.new("TextButton", autoClickTab) toggleBtn.Size = UDim2.new(0, 200, 0, 50) toggleBtn.Position = UDim2.new(0.5, -100, 0.2, 0) toggleBtn.Text = "🌐 Ativar Auto Click" toggleBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 200) toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255) toggleBtn.Font = Enum.Font.SourceSansBold toggleBtn.TextScaled = true

toggleBtn.MouseButton1Click:Connect(function() AutoClickEnabled = not AutoClickEnabled toggleBtn.Text = AutoClickEnabled and "🌐 Desativar Auto Click" or "🌐 Ativar Auto Click" spawn(function() while AutoClickEnabled do pcall(function() local args = { "rep" } game:GetService("Players").LocalPlayer:WaitForChild("muscleEvent"):FireServer(unpack(args)) end) task.wait(getgenv().ClickDelay) end end) end)

local sliderLabel = Instance.new("TextLabel", autoClickTab) sliderLabel.Position = UDim2.new(0.5, -100, 0.45, 0) sliderLabel.Size = UDim2.new(0, 200, 0, 20) sliderLabel.Text = "Delay: 0.05s" sliderLabel.BackgroundTransparency = 1 sliderLabel.TextColor3 = Color3.fromRGB(255,255,255)

local slider = Instance.new("TextButton", autoClickTab) slider.Position = UDim2.new(0.5, -100, 0.5, 0) slider.Size = UDim2.new(0, 200, 0, 10) slider.BackgroundColor3 = Color3.fromRGB(180, 100, 220) slider.Text = ""

local handle = Instance.new("Frame", slider) handle.Size = UDim2.new(0, 10, 1, 0) handle.Position = UDim2.new(0, 0, 0, 0) handle.BackgroundColor3 = Color3.fromRGB(255, 255, 255) handle.BorderSizePixel = 0

local dragging = false slider.InputBegan:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = true end end) slider.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.MouseButton1 then dragging = false end end) game:GetService("UserInputService").InputChanged:Connect(function(input) if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then local pos = math.clamp((input.Position.X - slider.AbsolutePosition.X) / slider.AbsoluteSize.X, 0, 1) handle.Position = UDim2.new(pos, -5, 0, 0) local newDelay = math.floor(((0.01 + (0.5 - 0.01) * (1 - pos)) * 1000)) / 1000 getgenv().ClickDelay = newDelay sliderLabel.Text = "Delay: " .. tostring(newDelay) .. "s" end end)

local creditTab = Instance.new("Frame", contentFrame) creditTab.Size = UDim2.new(1, 0, 1, 0) creditTab.BackgroundTransparency = 1

local creditLabel = Instance.new("TextLabel", creditTab) creditLabel.Size = UDim2.new(1, -20, 0, 50) creditLabel.Position = UDim2.new(0, 10, 0, 10) creditLabel.Text = "Feito por: GPT e KN \nTema Japonês Oriental" creditLabel.TextColor3 = Color3.fromRGB(255, 255, 255) creditLabel.Font = Enum.Font.SourceSansItalic creditLabel.TextScaled = true creditLabel.BackgroundTransparency = 1

local function showTab(tab) autoClickTab.Visible = false creditTab.Visible = false tab.Visible = true end autoClickTabBtn.MouseButton1Click:Connect(function() showTab(autoClickTab) end) creditTabBtn.MouseButton1Click:Connect(function() showTab(creditTab) end) showTab(autoClickTab)

