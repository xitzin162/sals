local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Sistema de Keys
local validKeys = {
    "taf-xk8p-menu", "taf-m2q9-menu", "taf-7n5r-menu", "taf-w4t6-menu", "taf-j9p3-menu",
    "taf-l1s8-menu", "taf-v6k2-menu", "taf-z3f7-menu", "taf-h9d4-menu", "taf-q5m1-menu",
    "taf-b8r6-menu", "taf-n2w9-menu", "taf-y7j3-menu", "taf-f4l8-menu", "taf-t6p5-menu",
    "taf-k9s2-menu", "taf-x1v7-menu", "taf-g4n8-menu", "taf-r7k3-menu", "taf-u2m6-menu",
    "taf-i5q9-menu", "taf-c8f1-menu", "taf-o3t7-menu", "taf-e6w4-menu", "taf-a9l2-menu",
}
local usedKeys = {}
local authenticated = false

local function checkKey(inputKey)
    -- Verifica se a key já foi usada
    for _, usedKey in ipairs(usedKeys) do
        if usedKey:lower() == inputKey:lower() then
            return false, "Key já foi utilizada!"
        end
    end
    
    -- Verifica se a key é válida
    for _, key in ipairs(validKeys) do
        if inputKey:lower() == key:lower() then
            table.insert(usedKeys, inputKey:lower()) -- Marca a key como usada
            return true, "Key válida!"
        end
    end
    return false, "Key inválida!"
end

local function getChar()
    return LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
end

local function getHum()
    return getChar():FindFirstChildOfClass("Humanoid")
end

-- Key Authentication GUI
local keyGui = Instance.new("ScreenGui")
keyGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
keyGui.Name = "TAFKeyAuth"
keyGui.ResetOnSpawn = false

local keyFrame = Instance.new("Frame")
keyFrame.Parent = keyGui
keyFrame.Size = UDim2.new(0, 350, 0, 200)
keyFrame.Position = UDim2.new(0.5, -175, 0.5, -100)
keyFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
keyFrame.Active = true
keyFrame.Draggable = true
local keyCorner = Instance.new("UICorner")
keyCorner.Parent = keyFrame
keyCorner.CornerRadius = UDim.new(0, 16)

-- Gradient
local gradient = Instance.new("UIGradient")
gradient.Parent = keyFrame
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(30, 30, 30)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(15, 15, 15))
}
gradient.Rotation = 45

local keyTitle = Instance.new("TextLabel")
keyTitle.Parent = keyFrame
keyTitle.Size = UDim2.new(1, 0, 0, 50)
keyTitle.Text = "🔐 TAF MENU - KEY AUTH"
keyTitle.TextColor3 = Color3.fromRGB(100, 200, 255)
keyTitle.Font = Enum.Font.GothamBlack
keyTitle.TextSize = 18
keyTitle.BackgroundTransparency = 1

local keyInput = Instance.new("TextBox")
keyInput.Parent = keyFrame
keyInput.Size = UDim2.new(1, -40, 0, 40)
keyInput.Position = UDim2.new(0, 20, 0, 70)
keyInput.PlaceholderText = "Digite sua key aqui..."
keyInput.Text = ""
keyInput.TextColor3 = Color3.new(1, 1, 1)
keyInput.Font = Enum.Font.Gotham
keyInput.TextSize = 16
keyInput.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
local inputCorner = Instance.new("UICorner")
inputCorner.Parent = keyInput
inputCorner.CornerRadius = UDim.new(0, 8)

local submitBtn = Instance.new("TextButton")
submitBtn.Parent = keyFrame
submitBtn.Size = UDim2.new(1, -40, 0, 40)
submitBtn.Position = UDim2.new(0, 20, 0, 125)
submitBtn.Text = "VERIFICAR KEY"
submitBtn.TextColor3 = Color3.new(1, 1, 1)
submitBtn.Font = Enum.Font.GothamBold
submitBtn.TextSize = 16
submitBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
local submitCorner = Instance.new("UICorner")
submitCorner.Parent = submitBtn
submitCorner.CornerRadius = UDim.new(0, 8)

local statusLabel = Instance.new("TextLabel")
statusLabel.Parent = keyFrame
statusLabel.Size = UDim2.new(1, 0, 0, 20)
statusLabel.Position = UDim2.new(0, 0, 1, -25)
statusLabel.Text = ""
statusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
statusLabel.Font = Enum.Font.Gotham
statusLabel.TextSize = 14
statusLabel.BackgroundTransparency = 1

-- Variables for connections
local flyConnection = nil
local noclipConnection = nil
local godmodeConnection = nil
local speedConnection = nil
local jumpConnection = nil
local auraConnection = nil
local espConnection = nil

local function loadMainMenu()
    if not authenticated then return end

    -- Main GUI
    local gui = Instance.new("ScreenGui")
    gui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    gui.Name = "TAFMenu"
    gui.ResetOnSpawn = false

    local frame = Instance.new("Frame")
    frame.Parent = gui
    frame.Size = UDim2.new(0, 450, 0, 600)
    frame.Position = UDim2.new(0.5, -225, 0.5, -300)
    frame.BackgroundColor3 = Color3.fromRGB(10, 10, 10)
    frame.Active = true
    frame.Draggable = true
    local frameCorner = Instance.new("UICorner")
    frameCorner.Parent = frame
    frameCorner.CornerRadius = UDim.new(0, 16)

    -- Gradient Background
    local bgGradient = Instance.new("UIGradient")
    bgGradient.Parent = frame
    bgGradient.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Color3.fromRGB(25, 25, 35)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(10, 10, 15))
    }
    bgGradient.Rotation = 135

    -- Stroke
    local stroke = Instance.new("UIStroke")
    stroke.Parent = frame
    stroke.Color = Color3.fromRGB(100, 200, 255)
    stroke.Thickness = 2
    stroke.Transparency = 0.5

    local title = Instance.new("TextLabel")
    title.Parent = frame
    title.Size = UDim2.new(1, 0, 0, 50)
    title.Text = "⚡ TAF MENU V2.0"
    title.TextColor3 = Color3.fromRGB(100, 200, 255)
    title.Font = Enum.Font.GothamBlack
    title.TextSize = 24
    title.BackgroundTransparency = 1

    local subtitle = Instance.new("TextLabel")
    subtitle.Parent = frame
    subtitle.Size = UDim2.new(1, 0, 0, 20)
    subtitle.Position = UDim2.new(0, 0, 0, 35)
    subtitle.Text = "Premium Exploit Menu"
    subtitle.TextColor3 = Color3.fromRGB(150, 150, 150)
    subtitle.Font = Enum.Font.Gotham
    subtitle.TextSize = 12
    subtitle.BackgroundTransparency = 1

    local minimize = Instance.new("TextButton")
    minimize.Parent = frame
    minimize.Size = UDim2.new(0, 35, 0, 35)
    minimize.Position = UDim2.new(1, -40, 0, 5)
    minimize.Text = "─"
    minimize.TextColor3 = Color3.new(1, 1, 1)
    minimize.Font = Enum.Font.GothamBold
    minimize.TextSize = 16
    minimize.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    local minCorner = Instance.new("UICorner")
    minCorner.Parent = minimize
    minCorner.CornerRadius = UDim.new(0, 8)

    local close = Instance.new("TextButton")
    close.Parent = frame
    close.Size = UDim2.new(0, 35, 0, 35)
    close.Position = UDim2.new(1, -80, 0, 5)
    close.Text = "✕"
    close.TextColor3 = Color3.new(1, 1, 1)
    close.Font = Enum.Font.GothamBold
    close.TextSize = 16
    close.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
    local closeCorner = Instance.new("UICorner")
    closeCorner.Parent = close
    closeCorner.CornerRadius = UDim.new(0, 8)

    -- Tabs (Movement tab removed)
    local tabs = {"Combat", "Visual", "Utility", "Settings"}
    local currentTab = nil
    local tabFrames = {}
    
    local content = Instance.new("Frame")
    content.Parent = frame
    content.Size = UDim2.new(1, -20, 1, -70)
    content.Position = UDim2.new(0, 10, 0, 60)
    content.BackgroundTransparency = 1

    local tabContainer = Instance.new("Frame")
    tabContainer.Parent = content
    tabContainer.Size = UDim2.new(1, 0, 0, 40)
    tabContainer.BackgroundTransparency = 1
    local tabLayout = Instance.new("UIListLayout")
    tabLayout.Parent = tabContainer
    tabLayout.FillDirection = Enum.FillDirection.Horizontal
    tabLayout.Padding = UDim.new(0, 5)

    for i, tabName in ipairs(tabs) do
        local btn = Instance.new("TextButton")
        btn.Parent = tabContainer
        btn.Size = UDim2.new(0, 85, 1, 0)
        btn.Text = tabName
        btn.TextColor3 = Color3.new(1, 1, 1)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 12
        btn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
        local tabCorner = Instance.new("UICorner")
        tabCorner.Parent = btn
        tabCorner.CornerRadius = UDim.new(0, 8)

        local tabFrame = Instance.new("ScrollingFrame")
        tabFrame.Parent = content
        tabFrame.Size = UDim2.new(1, 0, 1, -50)
        tabFrame.Position = UDim2.new(0, 0, 0, 50)
        tabFrame.BackgroundTransparency = 1
        tabFrame.Visible = false
        tabFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
        tabFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
        tabFrame.ScrollBarThickness = 8
        tabFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 200, 255)
        
        local layout = Instance.new("UIListLayout")
        layout.Parent = tabFrame
        layout.SortOrder = Enum.SortOrder.LayoutOrder
        layout.Padding = UDim.new(0, 8)

        tabFrames[tabName] = tabFrame

        btn.MouseButton1Click:Connect(function()
            for _, tf in pairs(tabFrames) do tf.Visible = false end
            for _, tabBtn in pairs(tabContainer:GetChildren()) do
                if tabBtn:IsA("TextButton") then
                    tabBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
                end
            end
            tabFrame.Visible = true
            btn.BackgroundColor3 = Color3.fromRGB(100, 200, 255)
            currentTab = tabName
        end)

        if i == 1 then
            tabFrame.Visible = true
            btn.BackgroundColor3 = Color3.fromRGB(100, 200, 255)
            currentTab = tabName
        end
    end

    -- Close and minimize functionality
    close.MouseButton1Click:Connect(function()
        gui:Destroy()
    end)

    local minimized = false
    minimize.MouseButton1Click:Connect(function()
        minimized = not minimized
        content.Visible = not minimized
        minimize.Text = minimized and "+" or "─"
        local targetSize = minimized and UDim2.new(0, 450, 0, 60) or UDim2.new(0, 450, 0, 600)
        local tween = TweenService:Create(frame, TweenInfo.new(0.3), {Size = targetSize})
        tween:Play()
    end)

    -- Button States
    local buttonStates = {}

    local function createButton(tab, label, callback, description)
        local tabFrame = tabFrames[tab]
        
        local container = Instance.new("Frame")
        container.Parent = tabFrame
        container.Size = UDim2.new(1, -10, 0, 80)
        container.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
        local containerCorner = Instance.new("UICorner")
        containerCorner.Parent = container
        containerCorner.CornerRadius = UDim.new(0, 12)
        
        local containerStroke = Instance.new("UIStroke")
        containerStroke.Parent = container
        containerStroke.Color = Color3.fromRGB(60, 60, 80)
        containerStroke.Thickness = 1
        containerStroke.Transparency = 0.7

        local btn = Instance.new("TextButton")
        btn.Parent = container
        btn.Size = UDim2.new(1, -20, 0, 35)
        btn.Position = UDim2.new(0, 10, 0, 10)
        btn.Text = label
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 16
        btn.TextColor3 = Color3.new(1, 1, 1)
        btn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
        local btnCorner = Instance.new("UICorner")
        btnCorner.Parent = btn
        btnCorner.CornerRadius = UDim.new(0, 8)

        local desc = Instance.new("TextLabel")
        desc.Parent = container
        desc.Size = UDim2.new(1, -20, 0, 25)
        desc.Position = UDim2.new(0, 10, 0, 50)
        desc.Text = description or "Funcionalidade avançada"
        desc.TextColor3 = Color3.fromRGB(180, 180, 180)
        desc.Font = Enum.Font.Gotham
        desc.TextSize = 12
        desc.BackgroundTransparency = 1
        desc.TextWrapped = true

        buttonStates[label] = false

        btn.MouseButton1Click:Connect(function()
            buttonStates[label] = not buttonStates[label]
            local targetColor = buttonStates[label] and Color3.fromRGB(255, 80, 80) or Color3.fromRGB(0, 120, 255)
            
            local tween = TweenService:Create(btn, TweenInfo.new(0.3), {BackgroundColor3 = targetColor})
            tween:Play()
            
            pcall(function()
                callback(buttonStates[label])
            end)
        end)
        
        return btn
    end

    local function createSlider(tab, label, min, max, default, callback, description)
        local tabFrame = tabFrames[tab]
        
        local container = Instance.new("Frame")
        container.Parent = tabFrame
        container.Size = UDim2.new(1, -10, 0, 100)
        container.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
        local containerCorner = Instance.new("UICorner")
        containerCorner.Parent = container
        containerCorner.CornerRadius = UDim.new(0, 12)

        local sliderLabel = Instance.new("TextLabel")
        sliderLabel.Parent = container
        sliderLabel.Size = UDim2.new(1, -20, 0, 25)
        sliderLabel.Position = UDim2.new(0, 10, 0, 5)
        sliderLabel.Text = label .. ": " .. default
        sliderLabel.TextColor3 = Color3.new(1, 1, 1)
        sliderLabel.Font = Enum.Font.GothamBold
        sliderLabel.TextSize = 14
        sliderLabel.BackgroundTransparency = 1

        local sliderBg = Instance.new("Frame")
        sliderBg.Parent = container
        sliderBg.Size = UDim2.new(1, -40, 0, 8)
        sliderBg.Position = UDim2.new(0, 20, 0, 35)
        sliderBg.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        local sliderBgCorner = Instance.new("UICorner")
        sliderBgCorner.Parent = sliderBg
        sliderBgCorner.CornerRadius = UDim.new(0, 4)

        local sliderFill = Instance.new("Frame")
        sliderFill.Parent = sliderBg
        sliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
        sliderFill.BackgroundColor3 = Color3.fromRGB(100, 200, 255)
        local sliderFillCorner = Instance.new("UICorner")
        sliderFillCorner.Parent = sliderFill
        sliderFillCorner.CornerRadius = UDim.new(0, 4)

        local desc = Instance.new("TextLabel")
        desc.Parent = container
        desc.Size = UDim2.new(1, -20, 0, 40)
        desc.Position = UDim2.new(0, 10, 0, 55)
        desc.Text = description or "Ajuste este valor"
        desc.TextColor3 = Color3.fromRGB(180, 180, 180)
        desc.Font = Enum.Font.Gotham
        desc.TextSize = 12
        desc.BackgroundTransparency = 1
        desc.TextWrapped = true

        local value = default
        local dragging = false
        
        sliderBg.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 then
                dragging = true
                local connection
                connection = UIS.InputChanged:Connect(function(input2)
                    if input2.UserInputType == Enum.UserInputType.MouseMovement and dragging then
                        local mouse = Mouse
                        local rel = mouse.X - sliderBg.AbsolutePosition.X
                        local percentage = math.clamp(rel / sliderBg.AbsoluteSize.X, 0, 1)
                        value = min + (max - min) * percentage
                        sliderFill.Size = UDim2.new(percentage, 0, 1, 0)
                        sliderLabel.Text = label .. ": " .. math.floor(value)
                        pcall(function()
                            callback(value)
                        end)
                    end
                end)
                
                local endConnection
                endConnection = UIS.InputEnded:Connect(function(input3)
                    if input3.UserInputType == Enum.UserInputType.MouseButton1 then
                        dragging = false
                        connection:Disconnect()
                        endConnection:Disconnect()
                    end
                end)
            end
        end)
    end

    -- Notification System
    local function createNotification(text, color)
        local notif = Instance.new("Frame")
        notif.Parent = gui
        notif.Size = UDim2.new(0, 300, 0, 60)
        notif.Position = UDim2.new(1, 0, 0, 20)
        notif.BackgroundColor3 = color or Color3.fromRGB(30, 30, 30)
        local notifCorner = Instance.new("UICorner")
        notifCorner.Parent = notif
        notifCorner.CornerRadius = UDim.new(0, 12)

        local notifText = Instance.new("TextLabel")
        notifText.Parent = notif
        notifText.Size = UDim2.new(1, -20, 1, 0)
        notifText.Position = UDim2.new(0, 10, 0, 0)
        notifText.Text = text
        notifText.TextColor3 = Color3.new(1, 1, 1)
        notifText.Font = Enum.Font.Gotham
        notifText.TextSize = 14
        notifText.BackgroundTransparency = 1
        notifText.TextWrapped = true

        local tweenIn = TweenService:Create(notif, TweenInfo.new(0.5), {Position = UDim2.new(1, -320, 0, 20)})
        tweenIn:Play()

        spawn(function()
            wait(3)
            local tweenOut = TweenService:Create(notif, TweenInfo.new(0.5), {Position = UDim2.new(1, 0, 0, 20)})
            tweenOut:Play()
            tweenOut.Completed:Connect(function()
                notif:Destroy()
            end)
        end)
    end

    -- === FUNCIONALIDADES ===

    -- COMBAT TAB
    createButton("Combat", "Fly", function(state)
        local hrp = getChar():FindFirstChild("HumanoidRootPart")
        if not hrp then return end

        if state then
            local gyro = Instance.new("BodyGyro")
            gyro.Parent = hrp
            local vel = Instance.new("BodyVelocity")
            vel.Parent = hrp
            gyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
            vel.MaxForce = Vector3.new(9e9, 9e9, 9e9)
            gyro.Name = "FlyGyro"
            vel.Name = "FlyVel"

            flyConnection = RunService.RenderStepped:Connect(function()
                if not buttonStates["Fly"] then 
                    if flyConnection then flyConnection:Disconnect() end
                    return 
                end
                local cam = workspace.CurrentCamera
                local dir = Vector3.new(0, 0, 0)
                if UIS:IsKeyDown(Enum.KeyCode.W) then dir = dir + cam.CFrame.LookVector end
                if UIS:IsKeyDown(Enum.KeyCode.S) then dir = dir - cam.CFrame.LookVector end
                if UIS:IsKeyDown(Enum.KeyCode.A) then dir = dir - cam.CFrame.RightVector end
                if UIS:IsKeyDown(Enum.KeyCode.D) then dir = dir + cam.CFrame.RightVector end
                if UIS:IsKeyDown(Enum.KeyCode.Space) then dir = dir + cam.CFrame.UpVector end
                if UIS:IsKeyDown(Enum.KeyCode.LeftShift) then dir = dir - cam.CFrame.UpVector end
                
                if vel and vel.Parent then
                    vel.Velocity = dir.Magnitude > 0 and dir.Unit * 50 or Vector3.new(0, 0, 0)
                end
                if gyro and gyro.Parent then
                    gyro.CFrame = cam.CFrame
                end
            end)
        else
            if flyConnection then flyConnection:Disconnect() end
            for _, v in pairs(hrp:GetChildren()) do
                if v.Name == "FlyGyro" or v.Name == "FlyVel" then
                    v:Destroy()
                end
            end
        end
    end, "Voe livremente pelo mapa usando WASD")

    createButton("Combat", "Noclip", function(state)
        if noclipConnection then noclipConnection:Disconnect() end
        if state then
            noclipConnection = RunService.Stepped:Connect(function()
                if buttonStates["Noclip"] then
                    pcall(function()
                        for _, part in pairs(getChar():GetDescendants()) do
                            if part:IsA("BasePart") then 
                                part.CanCollide = false 
                            end
                        end
                    end)
                end
            end)
        end
    end, "Atravesse paredes e objetos")

    createButton("Combat", "Godmode", function(state)
        local hum = getHum()
        if hum then
            if state then
                hum.MaxHealth = math.huge
                hum.Health = math.huge
                if godmodeConnection then godmodeConnection:Disconnect() end
                godmodeConnection = hum:GetPropertyChangedSignal("Health"):Connect(function()
                    if buttonStates["Godmode"] and hum.Health < hum.MaxHealth then
                        hum.Health = hum.MaxHealth
                    end
                end)
            else
                if godmodeConnection then godmodeConnection:Disconnect() end
                hum.MaxHealth = 100
                hum.Health = 100
            end
        end
    end, "Torna você invencível contra danos")

    createButton("Combat", "Speed Hack", function(state)
        local hum = getHum()
        if hum then
            hum.WalkSpeed = state and 100 or 16
        end
    end, "Aumenta drasticamente sua velocidade")

    createButton("Combat", "Jump Power", function(state)
        local hum = getHum()
        if hum then
            hum.JumpPower = state and 200 or 50
        end
    end, "Pule muito mais alto que o normal")

    createButton("Combat", "Rapid Fire", function(state)
        local tool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Tool")
        if tool and tool:FindFirstChild("Handle") then
            if state then
                spawn(function()
                    for i = 1, 50 do
                        if buttonStates["Rapid Fire"] then
                            tool:Activate()
                            wait(0.01)
                        end
                    end
                end)
            end
        end
    end, "Disparo rápido com ferramentas/armas")

    createButton("Combat", "Infinite Health", function(state)
        local hum = getHum()
        if hum and state then
            hum.MaxHealth = 999999
            hum.Health = 999999
        elseif hum and not state then
            hum.MaxHealth = 100
            hum.Health = 100
        end
    end, "Vida infinita sem godmode")

    createButton("Combat", "No Fall Damage", function(state)
        local hum = getHum()
        if hum then
            if state then
                hum.StateChanged:Connect(function(old, new)
                    if new == Enum.HumanoidStateType.FallingDown then
                        hum:ChangeState(Enum.HumanoidStateType.Freefall)
                    end
                end)
            end
        end
    end, "Remove dano de queda")

    createButton("Combat", "Teleport Behind Target", function(state)
        if state then
            local target = nil
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    target = player
                    break
                end
            end
            if target then
                local hrp = getChar():FindFirstChild("HumanoidRootPart")
                local targetHrp = target.Character.HumanoidRootPart
                if hrp and targetHrp then
                    hrp.CFrame = targetHrp.CFrame * CFrame.new(0, 0, 3)
                    createNotification("Teleportado atrás de " .. target.Name, Color3.fromRGB(255, 150, 0))
                end
            end
            buttonStates["Teleport Behind Target"] = false
        end
    end, "Teleporta atrás do primeiro player encontrado")

    -- VISUAL TAB
    createButton("Visual", "ESP Players", function(state)
        if espConnection then espConnection:Disconnect() end
        if state then
            espConnection = Players.PlayerAdded:Connect(function(player)
                if buttonStates["ESP Players"] then
                    player.CharacterAdded:Connect(function(char)
                        wait(1)
                        if buttonStates["ESP Players"] and char:FindFirstChild("HumanoidRootPart") then
                            local highlight = Instance.new("Highlight")
                            highlight.Parent = char
                            highlight.Name = "ESP_Highlight"
                            highlight.FillColor = Color3.fromRGB(255, 0, 100)
                            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                            highlight.FillTransparency = 0.5
                        end
                    end)
                end
            end)
            
            for _, p in ipairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    local h = p.Character:FindFirstChild("ESP_Highlight")
                    if not h then
                        local highlight = Instance.new("Highlight")
                        highlight.Parent = p.Character
                        highlight.Name = "ESP_Highlight"
                        highlight.FillColor = Color3.fromRGB(255, 0, 100)
                        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                        highlight.FillTransparency = 0.5
                    end
                end
            end
        else
            for _, p in ipairs(Players:GetPlayers()) do
                if p.Character then
                    local h = p.Character:FindFirstChild("ESP_Highlight")
                    if h then h:Destroy() end
                end
            end
        end
    end, "Destaca todos os players através das paredes")

    createButton("Visual", "Fullbright", function(state)
        local lighting = game:GetService("Lighting")
        if state then
            lighting.Brightness = 2
            lighting.ClockTime = 14
            lighting.FogEnd = 100000
            lighting.GlobalShadows = false
            lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        else
            lighting.Brightness = 1
            lighting.ClockTime = 12
            lighting.FogEnd = 100000
            lighting.GlobalShadows = true
            lighting.OutdoorAmbient = Color3.fromRGB(70, 70, 70)
        end
    end, "Remove sombras e escuridão do jogo")

    createButton("Visual", "Rainbow Character", function(state)
        local connection
        if state then
            connection = RunService.Heartbeat:Connect(function()
                if not buttonStates["Rainbow Character"] then 
                    if connection then connection:Disconnect() end
                    return 
                end
                pcall(function()
                    local char = getChar()
                    local time = tick()
                    for _, part in pairs(char:GetChildren()) do
                        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                            part.Color = Color3.fromHSV((time * 2) % 5 / 5, 1, 1)
                        end
                    end
                end)
            end)
        else
            if connection then connection:Disconnect() end
        end
    end, "Muda a cor do seu personagem em arco-íris")

    createButton("Visual", "Wireframe Mode", function(state)
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("BasePart") then
                if state then
                    obj.Material = Enum.Material.Neon
                    obj.Transparency = 0.7
                    obj.BrickColor = BrickColor.new("Bright blue")
                else
                    obj.Material = Enum.Material.Plastic
                    obj.Transparency = 0
                end
            end
        end
    end, "Modo wireframe para ver estruturas")

    createButton("Visual", "Night Vision", function(state)
        local lighting = game:GetService("Lighting")
        if state then
            lighting.Ambient = Color3.fromRGB(255, 255, 255)
            lighting.ColorShift_Bottom = Color3.fromRGB(255, 255, 255)
            lighting.ColorShift_Top = Color3.fromRGB(255, 255, 255)
        else
            lighting.Ambient = Color3.fromRGB(70, 70, 70)
            lighting.ColorShift_Bottom = Color3.fromRGB(0, 0, 0)
            lighting.ColorShift_Top = Color3.fromRGB(0, 0, 0)
        end
    end, "Visão noturna avançada")

    createButton("Visual", "Player Chams", function(state)
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                for _, part in pairs(player.Character:GetChildren()) do
                    if part:IsA("BasePart") then
                        if state then
                            part.Material = Enum.Material.ForceField
                            part.BrickColor = BrickColor.new("Really red")
                        else
                            part.Material = Enum.Material.Plastic
                        end
                    end
                end
            end
        end
    end, "Destaque especial nos players com efeito chams")

    -- UTILITY TAB
    createButton("Utility", "Auto Jump", function(state)
        RunService.Heartbeat:Connect(function()
            if state and getHum() then
                getHum().Jump = true
            end
        end)
    end, "Pula automaticamente e continuamente")

    createButton("Utility", "Infinite Yield", function(state)
        if state then
            pcall(function()
                loadstring(game:HttpGet('https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source'))()
            end)
        end
    end, "Carrega o famoso script Infinite Yield")

    createButton("Utility", "Anti AFK", function(state)
        if state then
            local VirtualUser = game:GetService("VirtualUser")
            game:GetService("Players").LocalPlayer.Idled:Connect(function()
                if buttonStates["Anti AFK"] then
                    VirtualUser:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
                    wait(1)
                    VirtualUser:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
                end
            end)
        end
    end, "Previne que você seja kickado por inatividade")

    createButton("Utility", "Teleport to Players", function(state)
        if state then
            local teleportGui = Instance.new("Frame")
            teleportGui.Parent = gui
            teleportGui.Size = UDim2.new(0, 250, 0, 300)
            teleportGui.Position = UDim2.new(0, 10, 0, 10)
            teleportGui.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            teleportGui.Active = true
            teleportGui.Draggable = true
            local tpGuiCorner = Instance.new("UICorner")
            tpGuiCorner.Parent = teleportGui
            tpGuiCorner.CornerRadius = UDim.new(0, 12)

            local tpTitle = Instance.new("TextLabel")
            tpTitle.Parent = teleportGui
            tpTitle.Size = UDim2.new(1, 0, 0, 30)
            tpTitle.Text = "📍 TELEPORT MENU"
            tpTitle.TextColor3 = Color3.fromRGB(100, 200, 255)
            tpTitle.Font = Enum.Font.GothamBold
            tpTitle.TextSize = 14
            tpTitle.BackgroundTransparency = 1

            local tpScroll = Instance.new("ScrollingFrame")
            tpScroll.Parent = teleportGui
            tpScroll.Size = UDim2.new(1, -10, 1, -40)
            tpScroll.Position = UDim2.new(0, 5, 0, 35)
            tpScroll.BackgroundTransparency = 1
            tpScroll.AutomaticCanvasSize = Enum.AutomaticSize.Y
            local tpLayout = Instance.new("UIListLayout")
            tpLayout.Parent = tpScroll
            tpLayout.Padding = UDim.new(0, 5)

            for _, player in pairs(Players:GetPlayers()) do
                if player ~= LocalPlayer then
                    local tpBtn = Instance.new("TextButton")
                    tpBtn.Parent = tpScroll
                    tpBtn.Size = UDim2.new(1, -10, 0, 30)
                    tpBtn.Text = "TP: " .. player.Name
                    tpBtn.TextColor3 = Color3.new(1, 1, 1)
                    tpBtn.Font = Enum.Font.Gotham
                    tpBtn.TextSize = 12
                    tpBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 255)
                    local tpBtnCorner = Instance.new("UICorner")
                    tpBtnCorner.Parent = tpBtn
                    tpBtnCorner.CornerRadius = UDim.new(0, 6)

                    tpBtn.MouseButton1Click:Connect(function()
                        pcall(function()
                            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                                local hrp = getChar():FindFirstChild("HumanoidRootPart")
                                if hrp then
                                    hrp.CFrame = player.Character.HumanoidRootPart.CFrame
                                    createNotification("Teleportado para " .. player.Name, Color3.fromRGB(0, 150, 0))
                                end
                            end
                        end)
                    end)
                end
            end

            local closeTP = Instance.new("TextButton")
            closeTP.Parent = teleportGui
            closeTP.Size = UDim2.new(0, 20, 0, 20)
            closeTP.Position = UDim2.new(1, -25, 0, 5)
            closeTP.Text = "X"
            closeTP.TextColor3 = Color3.new(1, 1, 1)
            closeTP.Font = Enum.Font.GothamBold
            closeTP.TextSize = 12
            closeTP.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
            local closeTpCorner = Instance.new("UICorner")
            closeTpCorner.Parent = closeTP
            
            closeTP.MouseButton1Click:Connect(function()
                teleportGui:Destroy()
                buttonStates["Teleport to Players"] = false
            end)
        end
    end, "Abre menu para teleportar para qualquer player")

    createButton("Utility", "Respawn", function(state)
        if state then
            pcall(function()
                LocalPlayer.Character:BreakJoints()
                createNotification("Respawnando...", Color3.fromRGB(255, 200, 0))
            end)
            buttonStates["Respawn"] = false
        end
    end, "Reseta seu personagem instantaneamente")

    createButton("Utility", "Btools", function(state)
        if state then
            local backpack = LocalPlayer.Backpack
            
            -- Clone Tool
            local clone = Instance.new("HopperBin")
            clone.Parent = backpack
            clone.Name = "Clone"
            clone.BinType = Enum.BinType.Clone
            
            -- Delete Tool  
            local delete = Instance.new("HopperBin")
            delete.Parent = backpack
            delete.Name = "Delete"
            delete.BinType = Enum.BinType.Hammer
            
            createNotification("Btools adicionados!", Color3.fromRGB(0, 255, 0))
            buttonStates["Btools"] = false
        end
    end, "Adiciona ferramentas de construção")

    createButton("Utility", "Walkspeed Bypass", function(state)
        local hum = getHum()
        if hum then
            if state then
                hum.WalkSpeed = 500
                -- Bypass anti-cheat detection
                local mt = getrawmetatable(game)
                local old = mt.__index
                setreadonly(mt, false)
                mt.__index = function(t, k)
                    if t == hum and k == "WalkSpeed" then
                        return 16
                    end
                    return old(t, k)
                end
                setreadonly(mt, true)
            else
                hum.WalkSpeed = 16
            end
        end
    end, "Velocidade alta com bypass de detecção")

    createButton("Utility", "Invisible Character", function(state)
        pcall(function()
            local char = getChar()
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") or part:IsA("Decal") then
                    if part.Name ~= "HumanoidRootPart" then
                        part.Transparency = state and 1 or 0
                    end
                elseif part:IsA("Accessory") then
                    local handle = part:FindFirstChild("Handle")
                    if handle then
                        handle.Transparency = state and 1 or 0
                    end
                end
            end
        end)
    end, "Torna seu personagem completamente invisível")

    createButton("Utility", "Chat Logger", function(state)
        if state then
            game:GetService("Players").PlayerChatted:Connect(function(chatType, player, message)
                if buttonStates["Chat Logger"] then
                    print("[CHAT] " .. player.Name .. ": " .. message)
                end
            end)
        end
    end, "Registra todas as mensagens do chat no console")

    createButton("Utility", "Auto Collect Items", function(state)
        RunService.Heartbeat:Connect(function()
            if not state then return end
            pcall(function()
                local hrp = getChar():FindFirstChild("HumanoidRootPart")
                if not hrp then return end
                
                for _, obj in pairs(workspace:GetDescendants()) do
                    if obj.Name:lower():find("coin") or obj.Name:lower():find("cash") or obj.Name:lower():find("money") then
                        if obj:IsA("BasePart") then
                            local distance = (hrp.Position - obj.Position).Magnitude
                            if distance <= 50 then
                                obj.CFrame = hrp.CFrame
                            end
                        end
                    end
                end
            end)
        end)
    end, "Coleta automaticamente itens próximos")

    createButton("Utility", "Auto Farm XP", function(state)
        RunService.Heartbeat:Connect(function()
            if not state then return end
            pcall(function()
                local hum = getHum()
                if hum then
                    -- Simula atividade para ganhar XP
                    hum.Jump = true
                    wait(0.1)
                    hum.Jump = false
                end
            end)
        end)
    end, "Farm automático de experiência")

    createButton("Utility", "Spam Jump", function(state)
        RunService.Heartbeat:Connect(function()
            if state and getHum() then
                getHum().Jump = true
                wait(0.1)
                getHum().Jump = false
            end
        end)
    end, "Pula repetidamente de forma automática")

    -- SETTINGS TAB
    createSlider("Settings", "Fly Speed", 10, 200, 50, function(value)
        -- Fly speed will be updated when fly is active
    end, "Ajusta a velocidade do modo fly")

    createSlider("Settings", "Walk Speed", 16, 500, 100, function(value)
        local hum = getHum()
        if hum and buttonStates["Speed Hack"] then
            hum.WalkSpeed = value
        end
    end, "Ajusta a velocidade de caminhada")

    createSlider("Settings", "Jump Height", 50, 500, 200, function(value)
        local hum = getHum()
        if hum and buttonStates["Jump Power"] then
            hum.JumpPower = value
        end
    end, "Ajusta a altura do pulo")

    -- Info Panel
    local infoPanel = Instance.new("Frame")
    infoPanel.Parent = tabFrames["Settings"]
    infoPanel.Size = UDim2.new(1, -10, 0, 120)
    infoPanel.BackgroundColor3 = Color3.fromRGB(20, 30, 20)
    local infoPanelCorner = Instance.new("UICorner")
    infoPanelCorner.Parent = infoPanel
    infoPanelCorner.CornerRadius = UDim.new(0, 12)

    local infoTitle = Instance.new("TextLabel")
    infoTitle.Parent = infoPanel
    infoTitle.Size = UDim2.new(1, 0, 0, 30)
    infoTitle.Text = "📊 INFORMAÇÕES DO SISTEMA"
    infoTitle.TextColor3 = Color3.fromRGB(100, 255, 100)
    infoTitle.Font = Enum.Font.GothamBlack
    infoTitle.TextSize = 14
    infoTitle.BackgroundTransparency = 1

    local infoText = Instance.new("TextLabel")
    infoText.Parent = infoPanel
    infoText.Size = UDim2.new(1, -20, 1, -35)
    infoText.Position = UDim2.new(0, 10, 0, 30)
    infoText.Text = "Usuário: " .. LocalPlayer.Name .. "\nKey: Autenticada ✅\nVersão: TAF Menu V2.0\nStatus: Online"
    infoText.TextColor3 = Color3.fromRGB(200, 200, 200)
    infoText.Font = Enum.Font.Gotham
    infoText.TextSize = 12
    infoText.BackgroundTransparency = 1
    infoText.TextYAlignment = Enum.TextYAlignment.Top

    -- Hotkeys display
    local hotkeysPanel = Instance.new("Frame")
    hotkeysPanel.Parent = tabFrames["Settings"]
    hotkeysPanel.Size = UDim2.new(1, -10, 0, 150)
    hotkeysPanel.BackgroundColor3 = Color3.fromRGB(30, 20, 30)
    local hotkeysPanelCorner = Instance.new("UICorner")
    hotkeysPanelCorner.Parent = hotkeysPanel
    hotkeysPanelCorner.CornerRadius = UDim.new(0, 12)

    local hotkeysTitle = Instance.new("TextLabel")
    hotkeysTitle.Parent = hotkeysPanel
    hotkeysTitle.Size = UDim2.new(1, 0, 0, 30)
    hotkeysTitle.Text = "⌨️ ATALHOS DO TECLADO"
    hotkeysTitle.TextColor3 = Color3.fromRGB(200, 100, 255)
    hotkeysTitle.Font = Enum.Font.GothamBlack
    hotkeysTitle.TextSize = 14
    hotkeysTitle.BackgroundTransparency = 1

    local hotkeysText = Instance.new("TextLabel")
    hotkeysText.Parent = hotkeysPanel
    hotkeysText.Size = UDim2.new(1, -20, 1, -35)
    hotkeysText.Position = UDim2.new(0, 10, 0, 30)
    hotkeysText.Text = "F4 - Mostrar/Ocultar Menu\nCTRL + F - Ativar/Desativar Fly\nCTRL + N - Ativar/Desativar Noclip\nCTRL + G - Ativar/Desativar Godmode\nALT + S - Speed Boost Rápido"
    hotkeysText.TextColor3 = Color3.fromRGB(200, 200, 200)
    hotkeysText.Font = Enum.Font.Gotham
    hotkeysText.TextSize = 12
    hotkeysText.BackgroundTransparency = 1
    hotkeysText.TextYAlignment = Enum.TextYAlignment.Top

    -- Advanced hotkeys
    UIS.InputBegan:Connect(function(key, gameProcessed)
        if gameProcessed then return end
        
        if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then
            if key.KeyCode == Enum.KeyCode.F then
                -- Toggle Fly
                buttonStates["Fly"] = not buttonStates["Fly"]
                createNotification("Fly: " .. (buttonStates["Fly"] and "ON" or "OFF"))
            elseif key.KeyCode == Enum.KeyCode.N then
                -- Toggle Noclip
                buttonStates["Noclip"] = not buttonStates["Noclip"]
                createNotification("Noclip: " .. (buttonStates["Noclip"] and "ON" or "OFF"))
            elseif key.KeyCode == Enum.KeyCode.G then
                -- Toggle Godmode
                buttonStates["Godmode"] = not buttonStates["Godmode"]
                createNotification("Godmode: " .. (buttonStates["Godmode"] and "ON" or "OFF"))
            end
        elseif UIS:IsKeyDown(Enum.KeyCode.LeftAlt) then
            if key.KeyCode == Enum.KeyCode.S then
                -- Quick speed boost
                local hum = getHum()
                if hum then
                    hum.WalkSpeed = hum.WalkSpeed == 16 and 100 or 16
                    createNotification("Speed: " .. (hum.WalkSpeed == 100 and "BOOST" or "NORMAL"))
                end
            end
        end
    end)

    -- Welcome notification
    spawn(function()
        wait(0.5)
        createNotification("🎉 TAF Menu carregado com sucesso!", Color3.fromRGB(0, 150, 0))
    end)

    -- Keybind to toggle GUI (F4)
    UIS.InputBegan:Connect(function(key)
        if key.KeyCode == Enum.KeyCode.F4 then
            frame.Visible = not frame.Visible
        end
    end)
end

-- Key authentication logic
submitBtn.MouseButton1Click:Connect(function()
    local key = keyInput.Text
    local isValid, message = checkKey(key)
    
    if isValid then
        authenticated = true
        statusLabel.Text = "✅ " .. message .. " Carregando menu..."
        statusLabel.TextColor3 = Color3.fromRGB(100, 255, 100)
        wait(1)
        keyGui:Destroy()
        loadMainMenu()
    else
        statusLabel.Text = "❌ " .. message
        statusLabel.TextColor3 = Color3.fromRGB(255, 100, 100)
        keyInput.Text = ""
    end
end)

-- Enter key support for authentication
keyInput.FocusLost:Connect(function(enterPressed)
    if enterPressed then
        submitBtn.MouseButton1Click:Fire()
    end
end)

-- Start message
print("TAF Menu V2.0 - Sistema de autenticação iniciado")
