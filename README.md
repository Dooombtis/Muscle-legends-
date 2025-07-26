-- ✅ AUTO FARM COMPLETO + ANTI AFK + UI

-- Variáveis globais
if getgenv().RepFarmRunning == nil then
    getgenv().RepFarmRunning = false
    getgenv().RepFarmDelay = 0.2
    getgenv().RepFarmCount = 0
end

local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local VIM = game:GetService("VirtualInputManager")
local player = Players.LocalPlayer
local muscleEvent = player:WaitForChild("muscleEvent")

-- GUI Setup
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "AutoFarmUI"
gui.ResetOnSpawn = false

-- Botão principal
local toggleButton = Instance.new("TextButton", gui)
toggleButton.Size = UDim2.new(0, 200, 0, 50)
toggleButton.Position = UDim2.new(0.5, -100, 0.5, -120)
toggleButton.Text = "Ativar Auto Farm"
toggleButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.TextScaled = true
toggleButton.Font = Enum.Font.SourceSansBold

-- Slider Frame
local sliderFrame = Instance.new("Frame", gui)
sliderFrame.Size = UDim2.new(0, 200, 0, 40)
sliderFrame.Position = UDim2.new(0.5, -100, 0.5, -60)
sliderFrame.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

local sliderBar = Instance.new("Frame", sliderFrame)
sliderBar.Size = UDim2.new(1, 0, 0.3, 0)
sliderBar.Position = UDim2.new(0, 0, 0.35, 0)
sliderBar.BackgroundColor3 = Color3.fromRGB(120, 120, 120)

local sliderKnob = Instance.new("TextButton", sliderBar)
sliderKnob.Size = UDim2.new(0, 20, 1, 0)
sliderKnob.Position = UDim2.new(0.2, -10, 0, 0)
sliderKnob.BackgroundColor3 = Color3.fromRGB(255, 255, 0)
sliderKnob.Text = ""

local sliderLabel = Instance.new("TextLabel", sliderFrame)
sliderLabel.Size = UDim2.new(1, 0, 1, 0)
sliderLabel.Position = UDim2.new(0, 0, -1, 0)
sliderLabel.Text = "Delay: 0.2s"
sliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
sliderLabel.BackgroundTransparency = 1
sliderLabel.TextScaled = true
sliderLabel.Font = Enum.Font.SourceSansBold

-- Contador
local counterLabel = Instance.new("TextLabel", gui)
counterLabel.Size = UDim2.new(0, 200, 0, 40)
counterLabel.Position = UDim2.new(0.5, -100, 0.5, 10)
counterLabel.Text = "Reps: 0"
counterLabel.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
counterLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
counterLabel.TextScaled = true
counterLabel.Font = Enum.Font.SourceSansBold

-- Anti-AFK
spawn(function()
    while true do
        wait(60) -- a cada 60 segundos
        VIM:SendKeyEvent(true, Enum.KeyCode.Space, false, game)
        VIM:SendKeyEvent(false, Enum.KeyCode.Space, false, game)
    end
end)

-- Auto Farm Loop
local function toggleAutoFarm()
    getgenv().RepFarmRunning = not getgenv().RepFarmRunning

    if getgenv().RepFarmRunning then
        toggleButton.Text = "Parar Auto Farm"
        toggleButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)

        spawn(function()
            while getgenv().RepFarmRunning do
                pcall(function()
                    muscleEvent:FireServer("rep")
                    getgenv().RepFarmCount += 1
                    counterLabel.Text = "Reps: " .. tostring(getgenv().RepFarmCount)
                end)
                wait(getgenv().RepFarmDelay)
            end
        end)
    else
        toggleButton.Text = "Ativar Auto Farm"
        toggleButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
    end
end

-- Clique no botão
toggleButton.MouseButton1Click:Connect(toggleAutoFarm)

-- Tecla de atalho ("R")
UIS.InputBegan:Connect(function(input, gpe)
    if not gpe and input.KeyCode == Enum.KeyCode.R then
        toggleAutoFarm()
    end
end)

-- Slider arrastável
local dragging = false
sliderKnob.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)
UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

RunService.RenderStepped:Connect(function()
    if dragging then
        local mouseX = UIS:GetMouseLocation().X
        local barX = sliderBar.AbsolutePosition.X
        local barW = sliderBar.AbsoluteSize.X
        local percent = math.clamp((mouseX - barX) / barW, 0, 1)
        sliderKnob.Position = UDim2.new(percent, -10, 0, 0)
        
        local delay = math.floor(((0.1 + (0.9 * percent)) * 100 + 0.5)) / 100
        getgenv().RepFarmDelay = delay
        sliderLabel.Text = "Delay: " .. tostring(delay) .. "s"
    end
end)
