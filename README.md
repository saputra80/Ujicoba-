local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local rs = game:GetService("RunService")

local gui = Instance.new("ScreenGui")
gui.Parent = player.PlayerGui
gui.Name = "CustomGUI"

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.5, -150, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
frame.BorderSizePixel = 2
frame.Parent = gui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.Text = "xx Jakarta xx"
title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 24
title.Parent = frame

local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -30, 0, 0)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 0, 0)
closeButton.Parent = frame
closeButton.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

local function createButton(name, position, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(1, 0, 0, 40)
    button.Position = UDim2.new(0, 0, 0, position)
    button.Text = name
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Parent = frame
    button.MouseButton1Click:Connect(callback)
end

-- Home Button
createButton("Home", 40, function()
    print("Home Button Clicked")
end)

-- Main Button
createButton("Main", 80, function()
    local mainFrame = Instance.new("Frame")
    mainFrame.Size = UDim2.new(0, 200, 0, 300)
    mainFrame.Position = UDim2.new(1, 10, 0, 0)
    mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    mainFrame.Parent = frame
    
    -- Invisible
    createButton("Invisible", 0, function()
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.Transparency = 1
            end
        end
    end).Parent = mainFrame

    -- Lempar Player
    createButton("Throw Player", 40, function()
        if humanoid then
            humanoid:Move(Vector3.new(0, 50, 0), true)
        end
    end).Parent = mainFrame

    -- Speed
    createButton("Speed", 80, function()
        humanoid.WalkSpeed = 100
    end).Parent = mainFrame
    
    -- Jump Power
    createButton("Jump Power", 120, function()
        humanoid.JumpPower = 150
    end).Parent = mainFrame
    
    -- Spectator Mode
    createButton("Spectator Mode", 160, function()
        local cam = workspace.CurrentCamera
        cam.CameraSubject = nil
    end).Parent = mainFrame
    
    -- Teleport
    createButton("Teleport", 200, function()
        character:SetPrimaryPartCFrame(CFrame.new(0, 50, 0))
    end).Parent = mainFrame
    
    -- Close Main Frame
    createButton("Close", 240, function()
        mainFrame:Destroy()
    end).Parent = mainFrame
end)

-- Other Button
createButton("Other", 120, function()
    print("Other Button Clicked")
end)