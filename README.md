repeat wait() until game:IsLoaded()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local code = "TakashiHub553"
local espAtivo = false
local aimbotAtivo = false
local currentTarget = nil

local gui = Instance.new("ScreenGui", PlayerGui)
gui.Name = "TakashiHubUI"
gui.ResetOnSpawn = false

local intro = Instance.new("TextLabel", gui)
intro.Size = UDim2.new(1, 0, 1, 0)
intro.BackgroundTransparency = 1
intro.BackgroundColor3 = Color3.new(0,0,0)
intro.Text = "🔫 Takashi Hub 🔫"
intro.TextColor3 = Color3.new(1,1,1)
intro.TextScaled = true
intro.Font = Enum.Font.GothamBlack
intro.TextTransparency = 1
intro.ZIndex = 100

TweenService:Create(intro, TweenInfo.new(1), {TextTransparency = 0, BackgroundTransparency = 0.5}):Play()
wait(2)
TweenService:Create(intro, TweenInfo.new(1), {TextTransparency = 1, BackgroundTransparency = 1}):Play()
wait(1)
intro:Destroy()

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 320, 0, 220)
frame.Position = UDim2.new(0.5, -160, 0.5, -110)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0

local titulo = Instance.new("TextLabel", frame)
titulo.Size = UDim2.new(1, 0, 0, 50)
titulo.BackgroundTransparency = 1
titulo.Text = "🔫 Takashi Hub 🔫"
titulo.TextColor3 = Color3.new(1,1,1)
titulo.Font = Enum.Font.GothamBold
titulo.TextSize = 22
titulo.Parent = frame

local textbox = Instance.new("TextBox", frame)
textbox.PlaceholderText = "Digite o código aqui..."
textbox.Size = UDim2.new(0.8, 0, 0, 40)
textbox.Position = UDim2.new(0.1, 0, 0.45, -20)
textbox.Font = Enum.Font.Gotham
textbox.TextSize = 18
textbox.Text = ""
textbox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
textbox.TextColor3 = Color3.new(1,1,1)
textbox.Parent = frame

local copiarBtn = Instance.new("TextButton", frame)
copiarBtn.Size = UDim2.new(0.45, -5, 0, 40)
copiarBtn.Position = UDim2.new(0.05, 0, 0.75, 0)
copiarBtn.Text = "👀 Pegar Código"
copiarBtn.Font = Enum.Font.GothamBold
copiarBtn.TextSize = 16
copiarBtn.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
copiarBtn.TextColor3 = Color3.new(1,1,1)
copiarBtn.Parent = frame

local verificarBtn = Instance.new("TextButton", frame)
verificarBtn.Size = UDim2.new(0.45, -5, 0, 40)
verificarBtn.Position = UDim2.new(0.5, 5, 0.75, 0)
verificarBtn.Text = "🔥 Verificar Código"
verificarBtn.Font = Enum.Font.GothamBold
verificarBtn.TextSize = 16
verificarBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 50)
verificarBtn.TextColor3 = Color3.new(1,1,1)
verificarBtn.Parent = frame

copiarBtn.MouseButton1Click:Connect(function()
    setclipboard(code)
    copiarBtn.Text = "✅ Copiado!"
    wait(2)
    copiarBtn.Text = "👀 Pegar Código"
end)

verificarBtn.MouseButton1Click:Connect(function()
    if textbox.Text == code then
        frame:Destroy()

        local bolinha = Instance.new("TextButton", gui)
        bolinha.Size = UDim2.new(0, 50, 0, 50)
        bolinha.Position = UDim2.new(0, 10, 0, 10)
        bolinha.BackgroundColor3 = Color3.new(1, 0, 0)
        bolinha.Text = ""
        bolinha.BorderSizePixel = 0
        bolinha.Draggable = true

        local corner = Instance.new("UICorner", bolinha)
        corner.CornerRadius = UDim.new(1, 0)

        spawn(function()
            while true do
                for h = 0, 1, 0.01 do
                    bolinha.BackgroundColor3 = Color3.fromHSV(h, 1, 1)
                    wait()
                end
            end
        end)

        local menu = Instance.new("Frame", gui)
        menu.Size = UDim2.new(0, 180, 0, 100)
        menu.Position = UDim2.new(0, 70, 0, 10)
        menu.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        menu.Visible = false

        local aimbotBtn = Instance.new("TextButton", menu)
        aimbotBtn.Size = UDim2.new(1, 0, 0, 40)
        aimbotBtn.Position = UDim2.new(0, 0, 0, 0)
        aimbotBtn.Text = "🔫 Aimbot 🔫"
        aimbotBtn.Font = Enum.Font.GothamBold
        aimbotBtn.TextSize = 16
        aimbotBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
        aimbotBtn.TextColor3 = Color3.new(1,1,1)
        aimbotBtn.Parent = menu

        local espBtn = Instance.new("TextButton", menu)
        espBtn.Size = UDim2.new(1, 0, 0, 40)
        espBtn.Position = UDim2.new(0, 0, 0, 50)
        espBtn.Text = "👁️ ESP 👁️"
        espBtn.Font = Enum.Font.GothamBold
        espBtn.TextSize = 16
        espBtn.BackgroundColor3 = Color3.fromRGB(100, 0, 0)
        espBtn.TextColor3 = Color3.new(1,1,1)
        espBtn.Parent = menu

        bolinha.MouseButton1Click:Connect(function()
            menu.Visible = not menu.Visible
        end)

        local aimbotAlvoBtn = nil

        aimbotBtn.MouseButton1Click:Connect(function()
            if not aimbotAlvoBtn then
                aimbotAlvoBtn = Instance.new("TextButton", gui)
                aimbotAlvoBtn.Size = UDim2.new(0, 160, 0, 40)
                aimbotAlvoBtn.Position = UDim2.new(1, -170, 0, 10)
                aimbotAlvoBtn.Text = "🔫 Aimbot 🔫"
                aimbotAlvoBtn.Font = Enum.Font.GothamBold
                aimbotAlvoBtn.TextSize = 16
                aimbotAlvoBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
                aimbotAlvoBtn.TextColor3 = Color3.new(1,1,1)

                aimbotAlvoBtn.MouseButton1Click:Connect(function()
                    local closest = nil
                    local shortest = math.huge
                    for _, p in pairs(Players:GetPlayers()) do
                        if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
                            local screenPos, onScreen = Camera:WorldToViewportPoint(p.Character.Head.Position)
                            if onScreen then
                                local dist = (Vector2.new(screenPos.X, screenPos.Y) - UserInputService:GetMouseLocation()).Magnitude
                                if dist < shortest then
                                    shortest = dist
                                    closest = p
                                end
                            end
                        end
                    end
                    currentTarget = closest
                    aimbotAtivo = not aimbotAtivo
                end)
            end
        end)

        espBtn.MouseButton1Click:Connect(function()
            for _, p in pairs(Players:GetPlayers()) do
                if p ~= LocalPlayer and p.Character then
                    local char = p.Character
                    if not char:FindFirstChild("TakashiESP") then
                        local hl = Instance.new("Highlight")
                        hl.Name = "TakashiESP"
                        hl.FillColor = Color3.fromRGB(255, 0, 0)
                        hl.OutlineColor = Color3.new(1, 1, 1)
                        hl.FillTransparency = 0.3
                        hl.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                        hl.Adornee = char
                        hl.Parent = char
                    end
                end
            end
        end)

        RunService.RenderStepped:Connect(function()
            if aimbotAtivo and currentTarget and currentTarget.Character and currentTarget.Character:FindFirstChild("Head") then
                local head = currentTarget.Character.Head
                Camera.CFrame = CFrame.new(Camera.CFrame.Position, head.Position)
            end
        end)

    else
        textbox.Text = "❌ Código Incorreto"
    end
end)
