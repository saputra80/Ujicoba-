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

local function toggleSpeed()
    speedActive = not speedActive
    humanoid.WalkSpeed = speedActive and 100 or 16
    print(speedActive and "Speed ON!" or "Speed OFF!")
end

local function toggleInvisible()
    invisibleActive = not invisibleActive
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") or part:IsA("MeshPart") then
            part.Transparency = invisibleActive and 1 or 0
            if part:IsA("BasePart") then
                part.CanCollide = not invisibleActive
            end
        end
    end
    print(invisibleActive and "Invisible ON!" or "Invisible OFF!")
end

local function toggleJump()
    jumpActive = not jumpActive
    humanoid.UseJumpPower = true
    humanoid.JumpPower = jumpActive and 100 or 50
    print(jumpActive and "Jump Power ON!" or "Jump Power OFF!")
end

local buttons = {
    {name = "Home", action = function()
        print("Home clicked!")
    end},
    {name = "Main", action = toggleSpeed},
    {name = "Other", action = toggleInvisible},
    {name = "Jump", action = toggleJump},
    {name = "MoreScript", action = function()
        print("MoreScript clicked!")
    end}
}

for i, buttonData in ipairs(buttons) do
    local button = Instance.new("TextButton", frame)
    button.Size = UDim2.new(0, 280, 0, 40)
    button.Position = UDim2.new(0, 10, 0, 40 * (i - 1) + 40)
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 20
    button.Text = buttonData.name

    button.MouseButton1Click:Connect(function()
        buttonData.action()
    end)
end