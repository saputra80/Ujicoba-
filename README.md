local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local Buttons = {}

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Frame.Size = UDim2.new(0, 400, 0, 300)
Frame.Position = UDim2.new(0.5, -200, 0.5, -150)

Title.Parent = Frame
Title.Text = "xx Jakarta xx"
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 24

local function createButton(name, position)
    local Button = Instance.new("TextButton")
    Button.Parent = Frame
    Button.Text = name
    Button.Size = UDim2.new(0, 100, 0, 30)
    Button.Position = UDim2.new(0, 10, 0, position)
    Button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.SourceSans
    Button.TextSize = 18

    Button.MouseButton1Click:Connect(function()
        print(name .. " Clicked!")
    end)

    table.insert(Buttons, Button)
end

createButton("Home", 40)
createButton("Main", 80)
createButton("GhostScript", 120)
createButton("Other", 160)
createButton("GameHub", 200)
createButton("MoreScript", 240)
createButton("Information", 280)

local CloseButton = Instance.new("TextButton")
CloseButton.Parent = Frame
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -40, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 20

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)