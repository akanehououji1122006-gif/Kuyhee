-- KU SHOP Script | MM2 Performance & ESP Hub
-- Features: FPS Boost, Anti-Disconnect, Speed, Role ESP

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local Workspace = game:GetService("Workspace")
local VirtualUser = game:GetService("VirtualUser")
local LocalPlayer = Players.LocalPlayer

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local Title = Instance.new("TextLabel")

ScreenGui.Name = "KUSHOP_MM2_HUB"
ScreenGui.Parent = game.CoreGui
ScreenGui.ResetOnSpawn = false

MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
MainFrame.BorderSizePixel = 0
MainFrame.Position = UDim2.new(0.35, 0, 0.2, 0)
MainFrame.Size = UDim2.new(0, 300, 0, 350)
MainFrame.Active = true
MainFrame.Draggable = true

Title.Name = "Title"
Title.Parent = MainFrame
Title.BackgroundColor3 = Color3.fromRGB(140, 20, 220)
Title.Size = UDim2.new(1, 0, 0, 40)
Title.Font = Enum.Font.SourceSansBold
Title.Text = "KU SHOP - MM2 Performance Hub"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 17.000

local function createToggle(name, text, posY)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Parent = MainFrame
    btn.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
    btn.Position = UDim2.new(0.05, 0, posY, 0)
    btn.Size = UDim2.new(0.9, 0, 0, 32)
    btn.Font = Enum.Font.SourceSansBold
    btn.Text = text .. ": OFF"
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.TextSize = 13.000
    return btn
end

local FpsBtn = createToggle("FpsBtn", "1. FPS ลื่นขั้นสุด (Max Optimization)", 0.14)
local SpeedBtn = createToggle("SpeedBtn", "2. วิ่งเร็ว (Speed Boost)", 0.25)
local AllEspBtn = createToggle("AllEspBtn", "3. มองเห็นทุกคน (All Players ESP)", 0.36)
local MurdererEspBtn = createToggle("MurdererEspBtn", "4. มองเห็นฆาตกร (สีแดง)", 0.47)
local SheriffEspBtn = createToggle("SheriffEspBtn", "5. มองเห็นนายอำเภอ (สีฟ้า)", 0.58)
local AntiAfkBtn = createToggle("AntiAfkBtn", "6. ป้องกันหลุดจากเซิร์ฟเวอร์ (Anti-AFK)", 0.69)

-- State Configs
local State = {
    FpsBoost = false,
    Speed = false,
    AllESP = false,
    MurdererESP = false,
    SheriffESP = false,
    AntiAFK = false
}

-- 1. & 6. FPS ลื่นขั้นสุด + ป้องกันการค้าง (Max FPS & Lag Reduction)
local function applyOptimization()
    if setfpscap then setfpscap(999) end
    Lighting.GlobalShadows = false
    Lighting.FogEnd = 9e9
    Lighting.Technology = Enum.Technology.Compatibility
    
    for _, v in pairs(Lighting:GetChildren()) do
        if v:IsA("PostEffect") then v.Enabled = false end
    end
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("BasePart") and not v:IsA("MeshPart") then
            v.Material = Enum.Material.SmoothPlastic
            v.Reflectance = 0
        elseif v:IsA("Decal") or v:IsA("Texture") then
            v:Destroy()
        elseif v:IsA("ParticleEmitter") or v:IsA("Trail") or v:IsA("Smoke") or v:IsA("Fire") or v:IsA("Sparkles") then
            v.Enabled = false
        end
    end
end

FpsBtn.MouseButton1Click:Connect(function()
    State.FpsBoost = not State.FpsBoost
    FpsBtn.Text = "1. FPS ลื่นขั้นสุด: " .. (State.FpsBoost and "ON" or "OFF")
    FpsBtn.BackgroundColor3 = State.FpsBoost and Color3.fromRGB(40, 160, 60) or Color3.fromRGB(35, 35, 45)
    if State.FpsBoost then applyOptimization() end
end)

-- 2. วิ่งเร็ว (Speed Boost)
SpeedBtn.MouseButton1Click:Connect(function()
    State.Speed = not State.Speed
    SpeedBtn.Text = "2. วิ่งเร็ว: " .. (State.Speed and "ON" or "OFF")
    SpeedBtn.BackgroundColor3 = State.Speed and Color3.fromRGB(40, 160, 60) or Color3.fromRGB(35, 35, 45)
end)

RunService.Heartbeat:Connect(function()
    if State.Speed and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
        LocalPlayer.Character.Humanoid.WalkSpeed = 32
    end
end)

-- 3., 4. & 5. ระบบ ESP (มองทุกคน / ฆาตกรสีแดง / นายอำเภอสีฟ้า)
local function applyHighlight(player, color, tag)
    if not player.Character then return end
    local char = player.Character
    local hl = char:FindFirstChild(tag)
    if not hl then
        hl = Instance.new("Highlight")
        hl.Name = tag
        hl.Parent = char
        hl.FillColor = color
        hl.OutlineColor = Color3.fromRGB(255, 255, 255)
        hl.FillTransparency = 0.4
        hl.OutlineTransparency = 0
    end
end

local function removeHighlight(player, tag)
    if player.Character and player.Character:FindFirstChild(tag) then
        player.Character[tag]:Destroy()
    end
end

RunService.RenderStepped:Connect(function()
    for _, p in pairs(Players:GetPlayers()) do
        if p ~= LocalPlayer and p.Character then
            local hasKnife = p.Backpack:FindFirstChild("Knife") or p.Character:FindFirstChild("Knife")
            local hasGun = p.Backpack:FindFirstChild("Gun") or p.Character:FindFirstChild("Gun")
            
            -- มองเห็นทุกคน
            if State.AllESP and not hasKnife and not hasGun then
                applyHighlight(p, Color3.fromRGB(0, 255, 120), "KU_InnocentESP")
            else
                removeHighlight(p, "KU_InnocentESP")
            end
            
            -- มองเห็นฆาตกร (สีแดง)
            if State.MurdererESP and hasKnife then
                applyHighlight(p, Color3.fromRGB(255, 0, 0), "KU_MurdererESP")
            else
                removeHighlight(p, "KU_MurdererESP")
            end
            
            -- มองเห็นนายอำเภอ (สีฟ้า)
            if State.SheriffESP and hasGun then
                applyHighlight(p, Color3.fromRGB(0, 150, 255), "KU_SheriffESP")
            else
                removeHighlight(p, "KU_SheriffESP")
            end
        end
    end
end)

AllEspBtn.MouseButton1Click:Connect(function()
    State.AllESP = not State.AllESP
    AllEspBtn.Text = "3. มองเห็นทุกคน: " .. (State.AllESP and "ON" or "OFF")
    AllEspBtn.BackgroundColor3 = State.AllESP and Color3.fromRGB(40, 160, 60) or Color3.fromRGB(35, 35, 45)
end)

MurdererEspBtn.MouseButton1Click:Connect(function()
    State.MurdererESP = not State.MurdererESP
    MurdererEspBtn.Text = "4. มองเห็นฆาตกร (สีแดง): " .. (State.MurdererESP and "ON" or "OFF")
    MurdererEspBtn.BackgroundColor3 = State.MurdererESP and Color3.fromRGB(40, 160, 60) or Color3.fromRGB(35, 35, 45)
end)

SheriffEspBtn.MouseButton1Click:Connect(function()
    State.SheriffESP = not State.SheriffESP
    SheriffEspBtn.Text = "5. มองเห็นนายอำเภอ (สีฟ้า): " .. (State.SheriffESP and "ON" or "OFF")
    SheriffEspBtn.BackgroundColor3 = State.SheriffESP and Color3.fromRGB(40, 160, 60) or Color3.fromRGB(35, 35, 45)
end)

-- 7. ป้องกันหลุดจากเซิร์ฟเวอร์ (Anti-AFK Kick Prevention)
AntiAfkBtn.MouseButton1Click:Connect(function()
    State.AntiAFK = not State.AntiAFK
    AntiAfkBtn.Text = "6. ป้องกันหลุดจากเซิร์ฟเวอร์: " .. (State.AntiAFK and "ON" or "OFF")
    AntiAfkBtn.BackgroundColor3 = State.AntiAFK and Color3.fromRGB(40, 160, 60) or Color3.fromRGB(35, 35, 45)
end)

LocalPlayer.Idled:Connect(function()
    if State.AntiAFK then
        VirtualUser:Button2Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
        task.wait(1)
        VirtualUser:Button2Up(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
    end
end)

-- Toggle UI Button
local ToggleUIBtn = Instance.new("TextButton")
ToggleUIBtn.Name = "ToggleUIBtn"
ToggleUIBtn.Parent = ScreenGui
ToggleUIBtn.BackgroundColor3 = Color3.fromRGB(140, 20, 220)
ToggleUIBtn.Position = UDim2.new(0.01, 0, 0.4, 0)
ToggleUIBtn.Size = UDim2.new(0, 80, 0, 32)
ToggleUIBtn.Font = Enum.Font.SourceSansBold
ToggleUIBtn.Text = "KU SHOP"
ToggleUIBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleUIBtn.TextSize = 12.000

ToggleUIBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)
