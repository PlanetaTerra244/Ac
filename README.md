-- Auto Cliker - Otimização + AutoClicker com GUI Quadrada Black
-- from: mario and biel

-- CONFIG OTIMIZAÇÃO
local ToDisable = {
	Textures = true,
	VisualEffects = true,
	Parts = true,
	Particles = true,
	Sky = true
}
local Stuff = {}

for _, v in next, game:GetDescendants() do
	if ToDisable.Parts and (v:IsA("Part") or v:IsA("UnionOperation") or v:IsA("BasePart")) then
		v.Material = Enum.Material.SmoothPlastic
		table.insert(Stuff, v)
	end

	if ToDisable.Particles and (v:IsA("ParticleEmitter") or v:IsA("Smoke") or v:IsA("Explosion") or v:IsA("Sparkles") or v:IsA("Fire")) then
		v.Enabled = false
		table.insert(Stuff, v)
	end

	if ToDisable.VisualEffects and (v:IsA("BloomEffect") or v:IsA("BlurEffect") or v:IsA("DepthOfFieldEffect") or v:IsA("SunRaysEffect")) then
		v.Enabled = false
		table.insert(Stuff, v)
	end

	if ToDisable.Textures and (v:IsA("Decal") or v:IsA("Texture")) then
		v.Texture = ""
		table.insert(Stuff, v)
	end

	if ToDisable.Sky and v:IsA("Sky") then
		v.Parent = nil
		table.insert(Stuff, v)
	end
end

-- AUTO CLICKER COM GUI BONITA E QUADRADA BLACK
local player = game.Players.LocalPlayer
local RunService = game:GetService("RunService")
local clickInterval = 0.05
local autoClicking = false

local gui = Instance.new("ScreenGui")
gui.Name = "AutoClikerGui"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- Main square frame (Black)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 120)
frame.Position = UDim2.new(0.5, -110, 0.5, -60)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Black
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true
frame.Parent = gui

local stroke = Instance.new("UIStroke")
stroke.Thickness = 2
stroke.Color = Color3.fromRGB(180, 80, 255)
stroke.Parent = frame

local title = Instance.new("TextLabel")
title.Text = "Auto Cliker"
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.BackgroundTransparency = 1
title.Position = UDim2.new(0,0,0,8)
title.Size = UDim2.new(1,0,0,32)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Parent = frame

local clickBtn = Instance.new("TextButton")
clickBtn.Text = "Ligar AutoClicker"
clickBtn.Font = Enum.Font.GothamSemibold
clickBtn.TextSize = 18
clickBtn.Position = UDim2.new(0.1,0,0.5,0)
clickBtn.Size = UDim2.new(0.8,0,0.23,0)
clickBtn.BackgroundColor3 = Color3.fromRGB(150, 60, 230)
clickBtn.TextColor3 = Color3.new(1,1,1)
clickBtn.BorderSizePixel = 0
clickBtn.Parent = frame

local btnCorner = Instance.new("UICorner")
btnCorner.CornerRadius = UDim.new(0, 8)
btnCorner.Parent = clickBtn

-- Subtitle at the bottom
local subtitle = Instance.new("TextLabel")
subtitle.Text = "from: mario and biel"
subtitle.Font = Enum.Font.Gotham
subtitle.TextSize = 14
subtitle.BackgroundTransparency = 1
subtitle.Position = UDim2.new(0,0,1,-22)
subtitle.Size = UDim2.new(1,0,0,18)
subtitle.TextColor3 = Color3.fromRGB(200, 200, 200)
subtitle.Parent = frame

local minimizeBtn = Instance.new("TextButton")
minimizeBtn.Text = "-"
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 24
minimizeBtn.Position = UDim2.new(1, -35, 0, -10)
minimizeBtn.Size = UDim2.new(0,28,0,28)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(180, 80, 255)
minimizeBtn.TextColor3 = Color3.fromRGB(255,255,255)
minimizeBtn.BorderSizePixel = 0
minimizeBtn.Parent = frame

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 10)
minCorner.Parent = minimizeBtn

-- Bola (circle) para minimizar (Black)
local miniFrame = Instance.new("Frame")
miniFrame.Size = UDim2.new(0, 50, 0, 50)
miniFrame.Position = UDim2.new(0.5, -25, 0.5, -25)
miniFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- Black
miniFrame.Visible = false
miniFrame.Active = true
miniFrame.Draggable = true
miniFrame.Parent = gui

local ballCorner = Instance.new("UICorner")
ballCorner.CornerRadius = UDim.new(1, 0)
ballCorner.Parent = miniFrame

local acLabel = Instance.new("TextLabel")
acLabel.Text = "AC"
acLabel.Font = Enum.Font.GothamBold
acLabel.TextSize = 20
acLabel.BackgroundTransparency = 1
acLabel.Size = UDim2.new(1,0,1,0)
acLabel.Position = UDim2.new(0,0,0,0)
acLabel.TextColor3 = Color3.fromRGB(255,255,255) -- White
acLabel.Parent = miniFrame

-- Invisible button for restoring GUI
local restoreBtn = Instance.new("TextButton")
restoreBtn.Size = UDim2.new(1,0,1,0)
restoreBtn.Position = UDim2.new(0,0,0,0)
restoreBtn.BackgroundTransparency = 1
restoreBtn.Text = ""
restoreBtn.Parent = miniFrame

-- Minimizar/maximizar lógica
local isMinimized = false

minimizeBtn.MouseButton1Click:Connect(function()
	isMinimized = true
	frame.Visible = false
	miniFrame.Visible = true
end)

restoreBtn.MouseButton1Click:Connect(function()
	isMinimized = false
	frame.Visible = true
	miniFrame.Visible = false
end)

clickBtn.MouseButton1Click:Connect(function()
	autoClicking = not autoClicking
	clickBtn.Text = autoClicking and "Desligar AutoClicker" or "Ligar AutoClicker"
	clickBtn.BackgroundColor3 = autoClicking and Color3.fromRGB(255, 80, 120) or Color3.fromRGB(150, 60, 230)
end)

-- AUTO CLICKER LOOP
RunService.RenderStepped:Connect(function()
	if autoClicking then
		local char = player.Character
		if char then
			local tool = char:FindFirstChildOfClass("Tool")
			if tool then
				tool:Activate()
			end
		end
		wait(clickInterval)
	end
end)
