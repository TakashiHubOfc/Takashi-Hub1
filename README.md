repeat wait() until game:IsLoaded()

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

local espAtivo = false
local aimbotTarget = nil
local aimbotAtivo = false
local flyAtivo = false
local killAuraAtivo = false

local gui = Instance.new("ScreenGui", PlayerGui)
gui.Name = "TakashiHubUI"
gui.ResetOnSpawn = false

local bolinha = Instance.new("TextButton", gui)
bolinha.Size = UDim2.new(0, 50, 0, 50)
bolinha.Position = UDim2.new(0, 10, 0, 10)
bolinha.Text = ""
bolinha.BorderSizePixel = 0
Instance.new("UICorner", bolinha).CornerRadius = UDim.new(1, 0)

spawn(function()
	while true do
		for h = 0, 1, 0.01 do
			bolinha.BackgroundColor3 = Color3.fromHSV(h, 1, 1)
			wait()
		end
	end
end)

local menu = Instance.new("Frame", gui)
menu.Size = UDim2.new(0, 180, 0, 250)
menu.Position = UDim2.new(0, 70, 0, 10)
menu.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
menu.Visible = false
bolinha.MouseButton1Click:Connect(function()
	menu.Visible = not menu.Visible
end)

local function criarBotao(txt, ordem, fn)
	local b = Instance.new("TextButton", menu)
	b.Size = UDim2.new(1, 0, 0, 40)
	b.Position = UDim2.new(0, 0, 0, (ordem - 1) * 45)
	b.Text = txt
	b.Font = Enum.Font.GothamBold
	b.TextSize = 16
	b.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	b.TextColor3 = Color3.new(1, 1, 1)
	b.MouseButton1Click:Connect(fn)
end

criarBotao("👁️ ESP 👁️", 1, function()
	espAtivo = not espAtivo
end)

criarBotao("🔫 Aimbot 🔫", 2, function()
	if aimbotAtivo then
		aimbotAtivo = false
		aimbotTarget = nil
	else
		local closest = nil
		local shortest = math.huge
		for _, p in pairs(Players:GetPlayers()) do
			if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Head") then
				local pos, vis = Camera:WorldToViewportPoint(p.Character.Head.Position)
				if vis then
					local dist = (Vector2.new(pos.X, pos.Y) - UserInputService:GetMouseLocation()).Magnitude
					if dist < shortest then
						shortest = dist
						closest = p
					end
				end
			end
		end
		if closest then
			aimbotTarget = closest
			aimbotAtivo = true
		end
	end
end)

criarBotao("🚀 Fly 🚀", 3, function()
	flyAtivo = not flyAtivo
end)

criarBotao("🌀 TP Inimigo 🌀", 4, function()
	for _, p in pairs(Players:GetPlayers()) do
		if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
			LocalPlayer.Character:MoveTo(p.Character.HumanoidRootPart.Position + Vector3.new(0, 5, 0))
			break
		end
	end
end)

criarBotao("⚔️ Kill Aura ⚔️", 5, function()
	killAuraAtivo = not killAuraAtivo
end)

RunService.RenderStepped:Connect(function()
	if espAtivo then
		for _, p in pairs(Players:GetPlayers()) do
			if p ~= LocalPlayer and p.Character and not p.Character:FindFirstChild("TakashiESP") then
				local h = Instance.new("Highlight")
				h.Name = "TakashiESP"
				h.FillColor = Color3.fromRGB(255, 0, 0)
				h.OutlineColor = Color3.new(1, 1, 1)
				h.FillTransparency = 0.3
				h.Adornee = p.Character
				h.Parent = p.Character

				local b = Instance.new("BillboardGui", p.Character)
				b.Name = "NameDisplay"
				b.Size = UDim2.new(0, 200, 0, 20)
				b.StudsOffset = Vector3.new(0, 3, 0)
				b.AlwaysOnTop = true

				local l = Instance.new("TextLabel", b)
				l.Size = UDim2.new(1, 0, 1, 0)
				l.BackgroundTransparency = 1
				l.Text = p.Name
				l.TextColor3 = Color3.new(1, 0, 0)
				l.TextStrokeTransparency = 0
				l.TextSize = 12
				l.Font = Enum.Font.GothamBold
			end
		end
	end
end)

RunService.RenderStepped:Connect(function()
	if aimbotAtivo and aimbotTarget and aimbotTarget.Character and aimbotTarget.Character:FindFirstChild("Head") then
		Camera.CFrame = CFrame.new(Camera.CFrame.Position, aimbotTarget.Character.Head.Position)
	end
end)

local vel
RunService.RenderStepped:Connect(function()
	if flyAtivo then
		local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
		if root then
			if not vel then
				vel = Instance.new("BodyVelocity", root)
				vel.Velocity = Vector3.new(0, 50, 0)
				vel.MaxForce = Vector3.new(1, 1, 1) * math.huge
			end
		end
	else
		if vel then
			vel:Destroy()
			vel = nil
		end
	end
end)

RunService.RenderStepped:Connect(function()
	if killAuraAtivo then
		for _, p in pairs(Players:GetPlayers()) do
			if p ~= LocalPlayer and p.Character and p.Character:FindFirstChild("Humanoid") and p.Character:FindFirstChild("HumanoidRootPart") then
				local humanoid = p.Character.Humanoid
				local hrp = p.Character.HumanoidRootPart
				if humanoid.Health > 0 and (hrp.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 then
					local guiChildren = PlayerGui:GetDescendants()
					local isAlly = false
					for _, v in pairs(guiChildren) do
						if v:IsA("TextLabel") and v.Text == p.DisplayName then
							isAlly = true
							break
						end
					end
					if not isAlly then
						humanoid:TakeDamage(5)
					end
				end
			end
		end
	end
end)
