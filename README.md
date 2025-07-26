-- KN HUB • 日本風 🔮 -- Interface completa com tema roxo, abas "Auto Click" e "Créditos", auto-farm com delay ajustável, slider, samurai no fundo e transparência.

-- Proteção para evitar duplicatas if getgenv().KNHubInitialized then return end getgenv().KNHubInitialized = true

-- Variáveis globais getgenv().AutoClickEnabled = false getgenv().ClickDelay = 0.05

-- Função de animação RGB local function animateStrokeColor(stroke) coroutine.wrap(function() while stroke and stroke.Parent do local t = tick() local r = math.sin(t) * 127 + 128 local g = math.sin(t + 2) * 127 + 128 local b = math.sin(t + 4) * 127 + 128 stroke.Color = Color3.fromRGB(r, g, b) wait(0.05) end end)() end

-- Criar GUI local player = game.Players.LocalPlayer local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui")) gui.Name = "KNHubUI" gui.ResetOnSpawn = false

-- Frame principal local mainFrame = Instance.new("Frame", gui) mainFrame.Size = UDim2.new(0, 400, 0, 350) mainFrame.Position = UDim2.new(0.5, -200, 0.5, -175) mainFrame.BackgroundColor3 = Color3.fromRGB(60, 0, 90) mainFrame.BorderSizePixel = 0 mainFrame.Active = true mainFrame.Draggable = true mainFrame.BackgroundTransparency = 0.2

local mainStroke = Instance.new("UIStroke", mainFrame) mainStroke.Thickness = 2 mainStroke.Transparency = 0.2 animateStrokeColor(mainStroke)

-- Samurai de fundo local samuraiImage = Instance.new("ImageLabel", mainFrame) samuraiImage.Size = UDim2.new(1, 0, 1, 0) samuraiImage.Position = UDim2.new(0, 0, 0, 0) samuraiImage.Image = "rbxassetid://14245947392" -- ID de exemplo samuraiImage.ImageTransparency = 0.7 samuraiImage.BackgroundTransparency = 1 samuraiImage.ImageColor3 = Color3.fromRGB(255, 60, 60) samuraiImage.ScaleType = Enum.ScaleType.Fit samuraiImage.ZIndex = 0

-- Título local title = Instance.new("TextLabel", mainFrame) title.Size = UDim2.new(1, 0, 0, 40) title.Position = UDim2.new(0, 0, 0, 0) title.BackgroundColor3 = Color3.fromRGB(90, 50, 130) title.Text = "🔮 KN HUB • 日本風 🔮" title.TextColor3 = Color3.fromRGB(255, 220, 255) title.Font = Enum.Font.Fantasy title.TextScaled = true title.BackgroundTransparency = 0.2 title.ZIndex = 2

-- Botão de fechar local closeBtn = Instance.new("TextButton", mainFrame) closeBtn.Size = UDim2.new(0, 30, 0, 30) closeBtn.Position = UDim2.new(1, -35, 0, 5) closeBtn.Text = "X" closeBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0) closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255) closeBtn.ZIndex = 3

closeBtn.MouseButton1Click:Connect(function() mainFrame.Visible = false end)

-- Botão de reabrir local openBtn = Instance.new("TextButton", gui) openBtn.Size = UDim2.new(0, 100, 0, 40) openBtn.Position = UDim2.new(0, 20, 0, 20) openBtn.Text = "Abrir KN Hub" openBtn.BackgroundColor3 = Color3.fromRGB(90, 0, 120) openBtn.TextColor3 = Color3.fromRGB(255, 255, 255) openBtn.Visible = true

openBtn.MouseButton1Click:Connect(function() mainFrame.Visible = true end)

-- Menus laterais e abas local sideMenu = Instance.new("Frame", mainFrame) sideMenu.Size = UDim2.new(0, 140, 1, -40) sideMenu.Position = UDim2.new(0, 0, 0, 40) sideMenu.BackgroundColor3 = Color3.fromRGB(80, 0, 130) sideMenu.BackgroundTransparency = 0.25 sideMenu.ZIndex = 2

local sideStroke = Instance.new("UIStroke", sideMenu) sideStroke.Thickness = 2 sideStroke.Transparency = 0.2 animateStrokeColor(sideStroke)

local contentFrame = Instance.new("Frame", mainFrame) contentFrame.Size = UDim2.new(1, -140, 1, -40) contentFrame.Position = UDim2.new(0, 140, 0, 40) contentFrame.BackgroundColor3 = Color3.fromRGB(70, 0, 100) contentFrame.BackgroundTransparency = 0.25 contentFrame.ZIndex = 2

local contentStroke = Instance.new("UIStroke", contentFrame) contentStroke.Thickness = 2 contentStroke.Transparency = 0.2 animateStrokeColor(contentStroke)

-- Função para criar botão de menu local function createMenuButton(name, yOffset) local btn = Instance.new("TextButton", sideMenu) btn.Size = UDim2.new(1, -10, 0, 40) btn.Position = UDim2.new(0, 5, 0, yOffset) btn.Text = name btn.BackgroundColor3 = Color3.fromRGB(120, 60, 160) btn.TextColor3 = Color3.fromRGB(255, 255, 255) btn.Font = Enum.Font.SourceSansBold btn.TextScaled = true btn.BackgroundTransparency = 0.15

local btnStroke = Instance.new("UIStroke", btn)
btnStroke.Thickness = 1.5
btnStroke.Transparency = 0.2
animateStrokeColor(btnStroke)

return btn

end

-- Abas local autoClickTabBtn = createMenuButton("🌐 Auto Click", 10) local creditTabBtn = createMenuButton("🏣 Créditos", 60)

-- Conteúdos das abas local autoClickTab = Instance.new("Frame", contentFrame) autoClickTab.Size = UDim2.new(1, 0, 1, 0) autoClickTab.BackgroundTransparency = 1

local toggleBtn = Instance.new("TextButton", autoClickTab) toggleBtn.Size = UDim2.new(0, 200, 0, 50) toggleBtn.Position = UDim2.new(0.5, -100, 0.2, 0) toggleBtn.Text = "🌐 Ativar Auto Click" toggleBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 200) toggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255) toggleBtn.Font = Enum.Font.SourceSansBold toggleBtn.TextScaled = true


