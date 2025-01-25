-- Define the Game Pass ID
local gamePassId = 12345678  -- Replace with your actual Game Pass ID

-- Get the necessary services
local Players = game:GetService("Players")
local MarketplaceService = game:GetService("MarketplaceService")

-- References to UI components
local screenGui = script.Parent
local skipButton = screenGui:WaitForChild("SkipButton")

-- Function to check if a player owns the game pass
local function playerOwnsGamePass(player, gamePassId)
    local success, hasPass = pcall(function()
        return MarketplaceService:UserOwnsGamePassAsync(player.UserId, gamePassId)
    end)
    if success then
        return hasPass
    else
        warn("Error checking game pass ownership:", hasPass)
        return false
    end
end

-- Function to handle skip button click
local function onSkipButtonClick()
    local player = Players.LocalPlayer  -- Get the local player
    if playerOwnsGamePass(player, gamePassId) then
        -- Player owns the game pass, perform the skip action
        -- Example: Skip to the next stage or desired action
        print("Player owns the game pass. Skipping to the next stage...")
        -- Add your skip logic here
    else
        -- Player does not own the game pass, prompt them to buy it
        MarketplaceService:PromptGamePassPurchase(player, gamePassId)
    end
end

-- Connect the button click event to the handler function
skipButton.MouseButton1Click:Connect(onSkipButtonClick)
