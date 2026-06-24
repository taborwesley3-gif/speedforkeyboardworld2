local Players = game:GetService("Players")

local player = Players.LocalPlayer
local teleportEnabled = false

local POS1 = Vector3.new(-397.6, 506.6, -177.6)
local POS2 = Vector3.new(-416.8, 607.5, 1265)

-- GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "TeleportToggleGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local button = Instance.new("TextButton")
button.Size = UDim2.new(0, 180, 0, 50)
button.Position = UDim2.new(0.5, -90, 0, 20)
button.TextScaled = true
button.TextColor3 = Color3.new(1, 1, 1)
button.Parent = screenGui

local corner = Instance.new("UICorner")
corner.Parent = button

local function updateButton()
    if teleportEnabled then
        button.Text = "Teleport Loop: ON"
        button.BackgroundColor3 = Color3.fromRGB(80, 255, 80)
    else
        button.Text = "Teleport Loop: OFF"
        button.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
    end
end

button.MouseButton1Click:Connect(function()
    teleportEnabled = not teleportEnabled
    updateButton()
end)

task.spawn(function()
    local useFirstPosition = true

    while true do
        if teleportEnabled then
            local character = player.Character
            local hrp = character and character:FindFirstChild("HumanoidRootPart")

            if hrp then
                if useFirstPosition then
                    hrp.CFrame = CFrame.new(POS1)
                else
                    hrp.CFrame = CFrame.new(POS2)
                end

                useFirstPosition = not useFirstPosition
            end
        end

        task.wait(0.1)
    end
end)

updateButton()
