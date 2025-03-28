local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MainGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = player.PlayerGui

local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 300, 0, 350)
frame.Position = UDim2.new(0.5, -150, 0.5, -175)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 2
frame.BorderColor3 = Color3.fromRGB(255, 255, 255)
frame.Visible = true

local closeButton = Instance.new("TextButton", frame)
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.Font = Enum.Font.SourceSansBold
closeButton.TextSize = 20

local iconButton = Instance.new("TextButton")
iconButton.Size = UDim2.new(0, 50, 0, 50)
iconButton.Position = UDim2.new(0, 10, 0, 10)
iconButton.BackgroundColor3 = Color3.fromRGB(0, 0, 255)
iconButton.Text = "JKT"
iconButton.TextColor3 = Color3.fromRGB(255, 255, 255)
iconButton.Font = Enum.Font.SourceSansBold
iconButton.TextSize = 20
iconButton.Parent = player.PlayerGui
iconButton.Visible = true

local guiVisible = true

closeButton.MouseButton1Click:Connect(function()
    guiVisible = false
    frame.Visible = false
end)

iconButton.MouseButton1Click:Connect(function()
    guiVisible = not guiVisible
    frame.Visible = guiVisible
end)

local speedActive = false
local invisibleActive = false
local jumpActive = false

local function createToggleButton(parent, name, position, action)
    local toggle = Instance.new("TextButton", parent)
    toggle.Size = UDim2.new(0, 120, 0, 40)
    toggle.Position = UDim2.new(0, position.X, 0, position.Y)
    toggle.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggle.Font = Enum.Font.SourceSansBold
    toggle.TextSize = 20
    toggle.Text = name .. " [OFF]"

    local active = false
    toggle.MouseButton1Click:Connect(function()
        active = not active
        toggle.BackgroundColor3 = active and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(100, 100, 100)
        toggle.Text = name .. (active and " [ON]" or " [OFF]")
        action(active)
    end)
end

createToggleButton(frame, "Speed", Vector2.new(10, 40), function(active)
    speedActive = active
    humanoid.WalkSpeed = active and 100 or 16
end)

createToggleButton(frame, "Invisible", Vector2.new(10, 90), function(active)
    invisibleActive = active
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") or part:IsA("MeshPart") then
            part.Transparency = active and 1 or 0
            if part:IsA("BasePart") then
                part.CanCollide = not active
            end
        end
    end
end)

createToggleButton(frame, "Jump", Vector2.new(10, 140), function(active)
    jumpActive = active
    humanoid.UseJumpPower = true
    humanoid.JumpPower = active and 100 or 50
end)