local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

local screenGui = Instance.new("ScreenGui", player.PlayerGui)
local frame = Instance.new("Frame", screenGui)
frame.Size = UDim2.new(0, 300, 0, 300)
frame.Position = UDim2.new(0.5, -150, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 2
frame.BorderColor3 = Color3.fromRGB(255, 255, 255)

local closeButton = Instance.new("TextButton", frame)
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.Font = Enum.Font.SourceSansBold
closeButton.TextSize = 20

closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)

local speedActive = false
local invisibleActive = false

local function toggleSpeed()
    speedActive = not speedActive
    if speedActive then
        humanoid.WalkSpeed = 100
        print("Speed ON!")
    else
        humanoid.WalkSpeed = 16
        print("Speed OFF!")
    end
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
    if invisibleActive then
        print("Invisible ON!")
    else
        print("Invisible OFF!")
    end
end

local buttons = {
    {name = "Home", action = function()
        print("Home clicked!")
    end},
    {name = "Main", action = toggleSpeed},
    {name = "Other", action = toggleInvisible},
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