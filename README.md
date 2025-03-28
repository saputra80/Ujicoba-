local frame = script.Parent
local closeButton = frame:WaitForChild("CloseButton")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Posisi dan ukuran frame
frame.Size = UDim2.new(0, 300, 0, 300)
frame.Position = UDim2.new(0.5, -150, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 2
frame.BorderColor3 = Color3.fromRGB(255, 255, 255)

-- Close button
closeButton.MouseButton1Click:Connect(function()
    frame.Visible = false
end)

-- Tombol-tombol menu
local buttons = {
    "Home",
    "Main",
    "Other",
    "MoreScript"
}

for i, name in ipairs(buttons) do
    local button = Instance.new("TextButton")
    button.Parent = frame
    button.Size = UDim2.new(0, 280, 0, 40)
    button.Position = UDim2.new(0, 10, 0, 40 * (i - 1) + 40)
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.SourceSansBold
    button.TextSize = 20
    button.Text = name

    button.MouseButton1Click:Connect(function()
        if name == "Main" then
            -- Perintah Speed
            humanoid.WalkSpeed = 100
            print("Speed activated!")
        elseif name == "Other" then
            -- Perintah Invisible
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("BasePart") then
                    part.Transparency = 1
                end
            end
            print("Invisible activated!")
        else
            print(name .. " clicked!")
        end
    end)
end