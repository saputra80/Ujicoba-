local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local flyActive = false
local flySpeed = 50
local connection

local userInput = game:GetService("UserInputService")
local runService = game:GetService("RunService")

local function startFlying()
    if connection then connection:Disconnect() end

    local bodyGyro = Instance.new("BodyGyro")
    bodyGyro.CFrame = humanoidRootPart.CFrame
    bodyGyro.Parent = humanoidRootPart
    
    local bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    bodyVelocity.Parent = humanoidRootPart

    connection = runService.RenderStepped:Connect(function()
        local camera = workspace.CurrentCamera
        local moveDirection = Vector3.zero
        
        if userInput:IsKeyDown(Enum.KeyCode.W) then
            moveDirection = moveDirection + camera.CFrame.LookVector
        end
        if userInput:IsKeyDown(Enum.KeyCode.S) then
            moveDirection = moveDirection - camera.CFrame.LookVector
        end
        if userInput:IsKeyDown(Enum.KeyCode.A) then
            moveDirection = moveDirection - camera.CFrame.RightVector
        end
        if userInput:IsKeyDown(Enum.KeyCode.D) then
            moveDirection = moveDirection + camera.CFrame.RightVector
        end
        if userInput:IsKeyDown(Enum.KeyCode.Space) then
            moveDirection = moveDirection + Vector3.new(0, 1, 0)
        end
        if userInput:IsKeyDown(Enum.KeyCode.LeftShift) then
            moveDirection = moveDirection - Vector3.new(0, 1, 0)
        end

        if moveDirection.Magnitude > 0 then
            bodyVelocity.Velocity = moveDirection.Unit * flySpeed
        else
            bodyVelocity.Velocity = Vector3.zero
        end

        bodyGyro.CFrame = camera.CFrame
    end)
end

local function stopFlying()
    if connection then connection:Disconnect() end
    for _, v in pairs(humanoidRootPart:GetChildren()) do
        if v:IsA("BodyGyro") or v:IsA("BodyVelocity") then
            v:Destroy()
        end
    end
end

local function createFlyButton(parent, name, position)
    local toggle = Instance.new("TextButton", parent)
    toggle.Size = UDim2.new(0, 120, 0, 40)
    toggle.Position = UDim2.new(0, position.X, 0, position.Y)
    toggle.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggle.Font = Enum.Font.SourceSansBold
    toggle.TextSize = 20
    toggle.Text = name .. " [OFF]"

    toggle.MouseButton1Click:Connect(function()
        flyActive = not flyActive
        toggle.BackgroundColor3 = flyActive and Color3.fromRGB(0, 255, 0) or Color3.fromRGB(100, 100, 100)
        toggle.Text = name .. (flyActive and " [ON]" or " [OFF]")

        if flyActive then
            startFlying()
        else
            stopFlying()
        end
    end)
end

createFlyButton(frame, "Fly", Vector2.new(150, 40))