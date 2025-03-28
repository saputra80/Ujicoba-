```
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer
local mouse = localPlayer:GetMouse()

local function killPlayer(target)
    if target and target:FindFirstChild("Humanoid") then
        target.Humanoid.Health = 0
        print("Pemain " .. target.Name .. " telah dibunuh!")
    end
end

local function startSpectating(target)
    if target and target:FindFirstChild("Humanoid") then
        workspace.CurrentCamera.CameraSubject = target.Humanoid
        print("Sedang menyaksikan: " .. target.Name)
    end
end

local function stopSpectating()
    workspace.CurrentCamera.CameraSubject = localPlayer.Character.Humanoid
    print("Berhenti menyaksikan")
end

mouse.Button1Down:Connect(function()
    local target = mouse.Target and mouse.Target.Parent
    if target and target:IsA("Model") and Players:GetPlayerFromCharacter(target) then
        killPlayer(target)
    end
end)

local spectating = false
mouse.KeyDown:Connect(function(key)
    if key == "e" then
        local target = mouse.Target and mouse.Target.Parent
        if target and target:IsA("Model") and Players:GetPlayerFromCharacter(target) then
            if not spectating then
                startSpectating(target)
                spectating = true
            else
                stopSpectating()
                spectating = false
            end
        end
    end
end)
```# Ujicoba-