-- Services
local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

-- Player
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Variables
local dynamicFurnitureFolder = Workspace:WaitForChild("DynamicFurniture")
local mapFolder = Workspace:WaitForChild("Map")
local bigRoom1 = mapFolder:FindFirstChild("BigRoom1")
local teleportDuration = 0.2 -- Reduced duration for teleport tween
local isActive = false -- Flag to control the process

-- Function to teleport player to a position smoothly
local function teleportToPosition(cframe)
    local tweenInfo = TweenInfo.new(teleportDuration, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(humanoidRootPart, tweenInfo, {CFrame = cframe})
    tween:Play()
    tween.Completed:Wait()
end

-- Function to start the process
local function startProcess()
    isActive = true
    while isActive do
        for _, scrapPile in ipairs(dynamicFurnitureFolder:GetChildren()) do
            if scrapPile:IsA("Model") and scrapPile.Name == "ScrapPile" then
                for _, model in ipairs(scrapPile:GetChildren()) do
                    if model:IsA("Model") then
                        -- Check player health
                        local humanoid = character:FindFirstChildOfClass("Humanoid")
                        if humanoid.Health < 100 then
                            if bigRoom1 then
                                teleportToPosition(bigRoom1.PrimaryPart.CFrame)
                                stopProcess() -- Stop the process after teleporting
                                return
                            else
                                warn("BigRoom1 not found in Map folder.")
                            end
                        end

                        local proximityPrompt = model:FindFirstChildOfClass("ProximityPrompt")
                        if proximityPrompt then
                            -- Set the proximity prompt to 0 second toggle
                            proximityPrompt.HoldDuration = 0
                            proximityPrompt.ActionText = "Scrap"

                            -- Teleport to the proximity prompt's position
                            teleportToPosition(proximityPrompt.Parent.PrimaryPart.CFrame + Vector3.new(0, 3, 0))

                            -- Reduced wait time to ensure the prompt registers the player's presence
                            wait(0.1)

                            -- Activate the proximity prompt
                            proximityPrompt:InputHoldBegin()
                            wait(0.05) -- Reduced delay to simulate hold time
                            proximityPrompt:InputHoldEnd()

                            if not isActive then
                                return -- Exit the loop if process is stopped
                            end
                        end
                    end
                end
            end
        end
        wait(0.5) -- Reduced wait between cycles
    end
end

-- Function to stop the process
local function stopProcess()
    isActive = false
end

-- Toggle function for the process
local function toggleProcess()
    if isActive then
        stopProcess()
    else
        startProcess()
    end
end

-- Key binding to toggle the process (using "P" key)
UserInputService.InputBegan:Connect(function(input, gameProcessedEvent)
    if input.KeyCode == Enum.KeyCode.P and not gameProcessedEvent then
        toggleProcess()
    end
end)
