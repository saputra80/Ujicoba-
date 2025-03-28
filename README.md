local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local flyActive = false
local flySpeed = 50
local bodyGyro, bodyVelocity

local userInput = game:GetService("UserInputService")
local runService = game:GetService("RunService")

local flyDirection = Vector3.zero

local function updateFly()
    if flyActive and bodyVelocity and bodyGyro then
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
            flyDirection = moveDirection.Unit * flySpeed
        else
            flyDirection = Vector3.zero
        end
        
        bodyVelocity.Velocity = flyDirection
        bodyGyro.CFrame = camera.CFrame
    end
end

local function enableFly()
    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.CFrame = humanoidRootPart.CFrame
    bodyGyro.Parent = humanoidRootPart
    
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
    bodyVelocity.Parent = humanoidRootPart
    
    runService.RenderStepped:Connect(updateFly)
end

local function disableFly()
    if bodyGyro then bodyGyro:Destroy() end
    if bodyVelocity then bodyVelocity:Destroy() end
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
            enableFly()
        else
            disableFly()
        end
    end)
end

createFlyButton(frame, "Fly", Vector2.new(150, 40))