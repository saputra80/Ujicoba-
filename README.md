local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MainGui"
screenGui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 150)
frame.Position = UDim2.new(0.5, -100, 0.5, -75)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0
frame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 10)
uiCorner.Parent = frame

local function createToggleButton(name, position, onToggle)
    local toggle = Instance.new("TextButton")
    toggle.Size = UDim2.new(0, 120, 0, 40)
    toggle.Position = UDim2.new(0, position.X, 0, position.Y)
    toggle.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggle.Font = Enum.Font.SourceSansBold
    toggle.TextSize = 20
    toggle.Text = name .. " [OFF]"
    toggle.Parent = frame

    local isActive = false

    toggle.MouseButton1Click:Connect(function()
        isActive = not isActive
        toggle.BackgroundColor3 = isActive and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(100, 100, 100)
        toggle.Text = name .. (isActive and " [ON]" or " [OFF]")
        onToggle(isActive)
    end)
end

createToggleButton("Speed", Vector2.new(40, 10), function(isActive)
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")
    humanoid.WalkSpeed = isActive and 50 or 16
end)

createToggleButton("Invisible", Vector2.new(40, 60), function(isActive)
    local character = player.Character or player.CharacterAdded:Wait()
    for _, part in pairs(character:GetChildren()) do
        if part:IsA("BasePart") then
            part.Transparency = isActive and 1 or 0
            part.CanCollide = not isActive
        end
    end
end)

createToggleButton("Jump", Vector2.new(40, 110), function(isActive)
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:WaitForChild("Humanoid")
    humanoid.JumpPower = isActive and 100 or 50
end)