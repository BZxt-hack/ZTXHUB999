

-- สคริปที่ เกี่ยวกับ Arise





repeat wait() until game:IsLoaded() and game.Players and game.Players.LocalPlayer and game.Players.LocalPlayer.Character

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "ZTX HUB " .. Fluent.Version,
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftAlt -- Used when theres no MinimizeKeybind
})
----ABOUT------------------------------------------------------------------------------------------------
local Tabs = {
    pageLegit = Window:AddTab({Title = "ABOUT",Icon = "align-justify"})
}

local Options = Fluent.Options

do
    Fluent:Notify({
        Title = "รันติดแล้วไอ้ควาย",
        Content = "KAJOK HUB 999",
        Duration = 5 -- Set to nil to make the notification not disappear
    })
    Tabs.pageLegit:AddSection("MADE BY เทพบน")
    Tabs.pageLegit:AddSection("สคริบ 20 แจกพ่องตาย นะค้าบ")

        
        

end

local Tabs = {
    Main = Window:AddTab({ Title = "FUNCTION", Icon = "" }),
}
local Options = Fluent.Options
---------WALK---------
do
    local Slider = Tabs.Main:AddSlider("WALK SPEED", {
        Title = "WALK SPEED",
        Description = "CHANGE WALK SPEED",
        Default = 20,
        Min = 0,
        Max = 1000,
        Rounding = 1,
        Callback = function(w)
            print("Slider was changed:", w)
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = w
            
        end
    })

    Slider:OnChanged(function(w)
        print("Slider changed:", w)
    end)
---------JUMP-------------
    local Slider = Tabs.Main:AddSlider("JUMP POWER", {
        Title = "JUMP POWER",
        Description = "CHANGE JUMP POWER",
        Default = 50,
        Min = 0,
        Max = 1000,
        Rounding = 1,
        Callback = function(j)
            print("Slider was changed:", j)
            game.Players.LocalPlayer.Character.Humanoid.JumpPower= j
            
        end
    })

    Slider:OnChanged(function(j)
        print("Slider changed:", j)
    end)
-------FLY----------------

local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local root = character:WaitForChild("HumanoidRootPart")
local flying = false
local flySpeed = 100
local flyConnection

local Slider = Tabs.Main:AddSlider("Slider", {
        Title = "Flyspeed",
        Description = "ลากเพื่อเปลี่ยนความเร็ว",
        Default = 100,
        Min = 10,
        Max = 1000,
        Rounding = 1,
        Callback = function(s)
            flySpeed = s
        end
    })
    Slider:OnChanged(function(s)
        flySpeed = s
    end)


------fly-------------
Tabs.Main:AddButton({
    Title = "PRESS TO FLY",
    Description = "เปิด/ปิด flying",
    Callback = function()
        flying = not flying -- Toggle flying state
        
        if flying then
            print("Flying enabled.")
            flyConnection = RunService.RenderStepped:Connect(function()
                if not flying then return end
                local moveDir = Vector3.new(0, 0, 0)
                local cam = workspace.CurrentCamera
                
                if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                    moveDir = moveDir + cam.CFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                    moveDir = moveDir - cam.CFrame.LookVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                    moveDir = moveDir - cam.CFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                    moveDir = moveDir + cam.CFrame.RightVector
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                    moveDir = moveDir + Vector3.new(0, 1, 0)
                end
                if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                    moveDir = moveDir - Vector3.new(0, 1, 0)
                end
                
                if moveDir.Magnitude > 0 then
                    moveDir = moveDir.Unit * flySpeed
                end
                
                root.Velocity = moveDir
            end)
        else
            print("Flying disabled.")
            if flyConnection then
                flyConnection:Disconnect()
                flyConnection = nil
            end
            root.Velocity = Vector3.new(0, 0, 0)
        end
    end
})

------------NO CLIP----------
    local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "NOCLIP [บินก่อนค่อยกดไอสัส]", Default = false })

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local bodyParts = {"Head", "HumanoidRootPart", "Torso", "LeftLeg", "RightLeg", "LeftArm", "RightArm"}

local noClipEnabled = false

-- Function to enable no-clip
local function enableNoClip()
    noClipEnabled = true
    humanoid.PlatformStand = true
    -- Disable collisions for all body parts
    for _, partName in pairs(bodyParts) do
        local part = character:FindFirstChild(partName)
        if part then
            part.CanCollide = false
        end
    end
    -- Disable all collisions in the game world (this is key for no-clip)
    for _, part in ipairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = false
        end
    end
end

-- Function to disable no-clip
local function disableNoClip()
    noClipEnabled = false
    humanoid.PlatformStand = false
    -- Re-enable collisions for all body parts
    for _, partName in pairs(bodyParts) do
        local part = character:FindFirstChild(partName)
        if part then
            part.CanCollide = true
        end
    end
    -- Re-enable all collisions in the game world
    for _, part in ipairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = true
        end
    end
end

-- Listen for toggle changes
Toggle:OnChanged(function()
    if Toggle.Value then
        enableNoClip()  -- Enable no-clip when toggle is on
    else
        disableNoClip() -- Disable no-clip when toggle is off
    end
end)

-- Set default toggle value to false
Toggle:SetValue(false)

end
---------PLAYER---------

local Tabs = {
    Main = Window:AddTab({ Title = "PLAYER", Icon = "" }),
    
}

local Options = Fluent.Options

do
    local TweenService = game:GetService("TweenService")
    local RunService = game:GetService("RunService")
    local Players = game:GetService("Players")

    local player = Players.LocalPlayer
    local humanoidRootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")

    local Plr = {}
    local PlayerTP

    -- 🛠 Update Player List
    local function UpdatePlayerList()
        Plr = {} -- Clear old list
        for i, v in pairs(Players:GetPlayers()) do
            table.insert(Plr, v.Name)
        end
        print("Updated Players List:", table.concat(Plr, ", ")) -- Debugging
    end

    UpdatePlayerList() -- Initial update

    -- 🛠 Add Dropdown to Select Player
    local Drop = Tabs.Main:AddDropdown("SELECT PLAYER", {
        Title = "SELECT PLAYER",
        Values = Plr,
        Multi = false,
        Default = 1,
    })

    Drop:OnChanged(function(t)
        print("Dropdown changed:", t)
        PlayerTP = t
    end)

    -- 🛠 Function to Tween (Smooth TP)
    local function SmoothTeleport(targetPos)
        if humanoidRootPart then
            -- Increased the duration to 6 seconds for slower teleportation
            local tweenInfo = TweenInfo.new(6, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)  -- Slower duration
            local goal = {CFrame = targetPos}
            local tween = TweenService:Create(humanoidRootPart, tweenInfo, goal)
            tween:Play()
        else
            print("Error: HumanoidRootPart not found!")
        end
    end

    -- 🛠 Click to Teleport (Tween)
    Tabs.Main:AddButton({
        Title = "CLICK TO TP",
        Description = "TWEEN",
        Callback = function()
            if PlayerTP then
                local targetPlayer = Players:FindFirstChild(PlayerTP)
                if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local targetPos = targetPlayer.Character.HumanoidRootPart.CFrame
                    SmoothTeleport(targetPos)
                    print("Smoothly teleported to:", PlayerTP)
                else
                    print("Error: Selected player not found or no character.")
                end
            else
                print("Error: No player selected.")
            end
        end
    })

    -- 🛠 Click to Teleport (Warp)
    Tabs.Main:AddButton({
        Title = "CLICK TO TP",
        Description = "WARP",
        Callback = function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players[PlayerTP].Character.HumanoidRootPart.CFrame
        end
    })

    -- 🛠 Auto TP Toggle (Tween)
    local autoTPConnection
    local AutoTweenToggle = Tabs.Main:AddToggle("AutoTweenTP", {Title = "AUTO TWEEN TP", Default = false })

    AutoTweenToggle:OnChanged(function(state)
        print("Auto Tween Toggle changed:", state)
        _G.TPPlayer = state

        if _G.TPPlayer then
            -- Start Auto TP
            autoTPConnection = RunService.RenderStepped:Connect(function()
                if PlayerTP then
                    local targetPlayer = Players:FindFirstChild(PlayerTP)
                    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                        local targetPos = targetPlayer.Character.HumanoidRootPart.CFrame
                        SmoothTeleport(targetPos)
                    else
                        print("Error: Target player not found.")
                    end
                end
            end)
        else
            -- Stop Auto TP
            if autoTPConnection then
                autoTPConnection:Disconnect()
                autoTPConnection = nil
            end
        end
    end)

    -- 🛠 Auto TP Toggle (Warp)
    local AutoWarpToggle = Tabs.Main:AddToggle("AutoWarpTP", {Title = "AUTO WARP TP", Default = false })

    AutoWarpToggle:OnChanged(function(state)
        print("Auto Warp Toggle changed:", state)
        _G.TPPlayer = state

        -- Start Auto Warp TP if toggle is on
        while _G.TPPlayer do
            wait(0.05)  -- Wait to prevent freezing
            if PlayerTP then
                local targetPlayer = Players:FindFirstChild(PlayerTP)
                if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
                else
                    print("Error: Target player not found.")
                end
            end
        end
    end)

    -- 🛠 Refresh Player List
    Tabs.Main:AddButton({
        Title = "REFRESH PLAYER",
        Description = "REFRESH",
        Callback = function()
            Window:Dialog({
                Title = "REFRESH PLAYER",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                            UpdatePlayerList()  -- Update the player list
                            if Drop then
                                Drop:SetValues(Plr) -- Refresh dropdown
                            else
                                print("Error: Drop is nil!")
                            end
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })

end


--------ESP PLAYER------------------    
Tabs.Main:AddSection("ESP")
local ESPEnabled = false -- Track ESP state

local Toggle = Tabs.Main:AddToggle("ESP_Toggle", { Title = "ESP PLAYER", Default = false })

Toggle:OnChanged(function(value)
    ESPEnabled = value -- Set ESP state based on toggle

    if ESPEnabled then
        print("ESP Enabled")

        -- Start ESP Loop
        task.spawn(function()
            while ESPEnabled do
                wait(0.4) -- Refresh every second
                if not ESPEnabled then break end -- Exit loop if ESP is disabled
                
                pcall(function()
                    for _, player in pairs(game.Players:GetPlayers()) do
                        if player.Character and player.Character:FindFirstChild("Head") then
                            local head = player.Character.Head
                            if not head:FindFirstChild("ESP") then
                                local BillboardGui = Instance.new("BillboardGui")
                                local TextLabel = Instance.new("TextLabel")
                                BillboardGui.Parent = head
                                BillboardGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
                                BillboardGui.Active = true
                                BillboardGui.Name = "ESP"
                                BillboardGui.AlwaysOnTop = true
                                BillboardGui.LightInfluence = 1.000
                                BillboardGui.Size = UDim2.new(0, 200, 0, 50)
                                BillboardGui.StudsOffset = Vector3.new(0, 2.5, 0)
                                TextLabel.Parent = BillboardGui
                                TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                                TextLabel.BackgroundTransparency = 1.000
                                TextLabel.Size = UDim2.new(0, 200, 0, 50)
                                TextLabel.Font = Enum.Font.GothamBold
                                TextLabel.Text = player.Name
                                TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
                                TextLabel.TextScaled = true
                                TextLabel.TextSize = 14.000
                                TextLabel.TextStrokeTransparency = 0.000
                                TextLabel.TextWrapped = true
                            end
                        end
                    end
                end)
            end
        end)
    else
        print("ESP Disabled")
        
        -- Remove ESP from all players
        for _, player in pairs(game.Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChild("Head") then
                local head = player.Character.Head
                if head:FindFirstChild("ESP") then
                    head:FindFirstChild("ESP"):Destroy()
                end
            end
        end
    end
end)

Options.ESP_Toggle:SetValue(false) -- Ensure ESP is off by default


-------ARISE CROSSOVER----------



local Tabs = {
    Main = Window:AddTab({ Title = "ARISE CROSSOVER", Icon = "" }),

}

local Options = Fluent.Options

do
Tabs.Main:AddSection("MAIN")

-------AUTO MON----
local TweenService = game:GetService("TweenService")
local player = game.Players.LocalPlayer 
local character = player.Character or player.CharacterAdded:Wait()
local root = character:WaitForChild("HumanoidRootPart")

-- Function to get all enemies
local function getAllEnemies()
    local enemies = {}
    for _, v in next, workspace.__Main.__Enemies.Client:GetChildren() do
        if v:IsA("Model") and v:FindFirstChild("HealthBar") then
            local title = v.HealthBar.Main:FindFirstChild("Title")
            if title and not table.find(enemies, title.Text) then
                table.insert(enemies, title.Text)
            end
        end
    end
    return enemies
end

local enemyList = getAllEnemies()
local selectedEnemy = enemyList[1] or "None"

-- Fluent UI Dropdown
local Dropdown = Tabs.Main:AddDropdown("EnemyDropdown", {
    Title = "Select Enemy",
    Values = enemyList,
    Multi = false,
    Default = 1,
})

Dropdown:OnChanged(function(Value)
    selectedEnemy = Value
    print("📌 Selected Monster:", selectedEnemy)
end)

-- Function to get selected enemy
local function getSelectedEnemy()
    for _, v in next, workspace.__Main.__Enemies.Client:GetChildren() do
        if v:IsA("Model") and v:FindFirstChild("HealthBar") then
            local title = v.HealthBar.Main:FindFirstChild("Title")
            if title and title.Text == selectedEnemy then
                return v
            end
        end
    end
    return nil
end

-- Function to move to selected enemy, adjusted to teleport next to the enemy
local function moveToSelectedEnemy()
    local enemy = getSelectedEnemy()
    if enemy then
        -- Get the enemy's position
        local enemyPosition = enemy.HumanoidRootPart.Position

        -- Calculate a new position next to the enemy
        local offset = Vector3.new(5, 0, 0)  -- 5 units to the right
        local targetPosition = enemyPosition + offset

        -- Create a CFrame based on the calculated position
        local targetCFrame = CFrame.new(targetPosition)

        -- Create a tween to smoothly move the player to the new position
        local tweenInfo = TweenInfo.new(3, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)
        local tween = TweenService:Create(root, tweenInfo, {CFrame = targetCFrame})
        tween:Play()
    else
        warn("Enemy not found:", selectedEnemy)
    end
end

-- Fluent UI Button to teleport to enemy
Tabs.Main:AddButton({
    Title = "Go to Enemy",
    Description = "Click to teleport to the selected monster",
    Callback = function()
        moveToSelectedEnemy()
    end
})

-- Fluent UI Button to refresh enemy list
Tabs.Main:AddButton({
    Title = "Refresh List",
    Description = "Click to update the dropdown list",
    Callback = function()
        enemyList = getAllEnemies()
        Dropdown:SetValues(enemyList)
        print("🔄 Updated monster list:", enemyList)
    end
})





---------------AUTO ATTACK------------------------
local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "AUTO ATTACK", Default = false })

Toggle:OnChanged(function(t)
    print("Toggle changed:", t)
    
    -- Accessing the player’s Settings correctly
    local player = game:GetService("Players").LocalPlayer
    local settings = player:FindFirstChild("Settings")
    
    -- Check if the Settings object exists
    if settings then
        if t then
            -- Enable AutoAttack
            settings:SetAttribute("AutoAttack", true)
            print("AutoAttack Enabled")
        else
            -- Disable AutoAttack
            settings:SetAttribute("AutoAttack", false)
            print("AutoAttack Disabled")
        end
    else
        print("Error: Settings object not found.")
    end
end)

-- Initialize Toggle as disabled
Options.MyToggle:SetValue(false)

----------AUTO ZIRU----------
local TweenService = game:GetService("TweenService")

-- Create the toggle for AUTO ZIRU
local Toggle = Tabs.Main:AddToggle("AUTO ZIRU", {Title = "AUTO ZIRU", Default = false})

Toggle:OnChanged(function(t)
    print("Toggle changed:", t)
    
    -- Accessing the player’s Settings correctly
    local player = game:GetService("Players").LocalPlayer
    local settings = player:FindFirstChild("Settings")
    
    -- Check if the Settings object exists
    if settings then
        if t then
            -- Enable AutoAttack
            settings:SetAttribute("AutoAttack", true)
            print("AutoAttack Enabled")
            
            -- Play the tween when the toggle is enabled
            local Tw = TweenService:Create(game.Players.LocalPlayer.Character.HumanoidRootPart, 
            TweenInfo.new(25, Enum.EasingStyle.Linear, Enum.EasingDirection.Out, 0, false, 0), 
            {CFrame = CFrame.new(3845.51343, 60.1106224, 3056.59473, 0.972756386, -6.78275228e-07, 0.231829762, 5.63077379e-07, 1, 5.63077379e-07, -0.231829762, -4.17199004e-07, 0.972756386)}):Play()
        else
            -- Disable AutoAttack
            settings:SetAttribute("AutoAttack", false)
            print("AutoAttack Disabled")
            
            -- Optionally stop or reset the tween if the toggle is turned off
            print("Tween not playing.")
        end
    else
        print("Error: Settings object not found.")
    end
end)

-- Initialize Toggle as disabled
Toggle:SetValue(false)





-------------------------CODE-------------------------------

    Tabs.Main:AddButton({
        Title = "AUTO REDEEM CODE",
        Description = "AUTO CODE",
        Callback = function()
            Window:Dialog({
                Title = "AUTO REDEEM CODE",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                            local codes = {"RUNES", "DRAGONBLUE", "UPDATE", "BETA"}
for _, autocode in ipairs(codes) do
    local args = {
        [1] = {
            [1] = {
                ["Event"] = "UseCode",
                ["Code"] = autocode
            },
            [2] = "\n"
        }
    }
    game:GetService("ReplicatedStorage"):WaitForChild("BridgeNet2"):WaitForChild("dataRemoteEvent"):FireServer(unpack(args))
    task.wait(0.5)
end

                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })

    Options.MyToggle:SetValue(false)
-----------DUNGEON----------------------

Tabs.Main:AddSection("DUNGEON") 


local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local AutoTween = false
local LastTarget = nil -- Store the last selected monster
local TweenSpeed = 100 -- Speed of the tween (lower = slower, higher = faster)

-- Function to find the nearest monster (not repeating the last one)
local function getNearestMonster()
    local nearestMonster = nil
    local shortestDistance = math.huge

    for _, v in next, workspace.__Main.__Enemies.Client:GetChildren() do
        if v:IsA("Model") and v:FindFirstChild("HealthBar") and v ~= LastTarget then
            local monsterPosition = v:GetPivot().Position
            local distance = (HumanoidRootPart.Position - monsterPosition).Magnitude

            if distance < shortestDistance then
                shortestDistance = distance
                nearestMonster = v
            end
        end
    end

    return nearestMonster
end

-- Function to teleport character to a position next to the monster
local function teleportToMonster(monster)
    if not monster or not monster.PrimaryPart then return end

    -- Calculate a position next to the monster, adjusting the offset (here, +5 units to the right)
    local monsterPosition = monster.PrimaryPart.Position
    local offset = Vector3.new(5, 0, 0)  -- Adjust the offset as needed (can change direction to be random)
    local targetPosition = monsterPosition + offset

    -- Tween the character to the new target position
    local tweenInfo = TweenInfo.new((HumanoidRootPart.Position - targetPosition).Magnitude / TweenSpeed, Enum.EasingStyle.Linear, Enum.EasingDirection.Out)
    local tween = TweenService:Create(HumanoidRootPart, tweenInfo, { CFrame = CFrame.new(targetPosition) })
    tween:Play()

    LastTarget = monster -- Store the most recently selected monster
end

-- Start the AutoTween system
local function startAutoTween()
    while AutoTween do
        local monster = getNearestMonster()
        if monster then
            teleportToMonster(monster)  -- Use teleportToMonster instead of tweenToMonster
        end
        wait(3) -- Wait 3 seconds before looking for another monster
    end
end

-- Create Toggle on Fluent UI
local Toggle = Tabs.Main:AddToggle("AutoTweenToMonster", { Title = "Auto Dungeon", Default = false })

Toggle:OnChanged(function()
    AutoTween = Options.AutoTweenToMonster.Value
    if AutoTween then
        task.spawn(startAutoTween) -- Run in a separate thread
    end
end)

Options.AutoTweenToMonster:SetValue(false)




    ------------สรา้งroom-------------
    Tabs.Main:AddButton({
        Title = "CREATE ROOM",
        Description = "CREAT DUNGEON",
        Callback = function()
            Window:Dialog({
                Title = "CREAT ROOM",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                            local args = {
    [1] = {
        [1] = {
            ["Event"] = "DungeonAction",
            ["Action"] = "Create"
        },
        [2] = "\n"
    }
}

game:GetService("ReplicatedStorage"):WaitForChild("BridgeNet2"):WaitForChild("dataRemoteEvent"):FireServer(unpack(args))
                            
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })

    Tabs.Main:AddButton({
        Title = "LEAVE ROOM",
        Description = "LEAVE DUNGEOUN ROOM",
        Callback = function()
            Window:Dialog({
                Title = "LEAVE ROOM",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                            local args = {
    [1] = {
        [1] = {
            ["Dungeon"] = 6132080989,
            ["Event"] = "DungeonAction",
            ["Action"] = "Leave"
        },
        [2] = "\n"
    }
}

game:GetService("ReplicatedStorage"):WaitForChild("BridgeNet2"):WaitForChild("dataRemoteEvent"):FireServer(unpack(args))
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })

    Tabs.Main:AddButton({
        Title = "ENTER",
        Description = "ENTER DUNGEOUN",
        Callback = function()
            Window:Dialog({
                Title = "ENTER DUNGEON",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                            local args = {
    [1] = {
        [1] = {
            ["Dungeon"] = 6132080989;
            ["Event"] = "DungeonAction";
            ["Action"] = "Start";
        };
        [2] = "\n";
    };
}
game:GetService("ReplicatedStorage"):WaitForChild("BridgeNet2", 9e9):WaitForChild("dataRemoteEvent", 9e9):FireServer(unpack(args))
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })


    

----------teleport arise crossover---------

local Tabs = {
    Main = Window:AddTab({ Title = "TELEPORT", Icon = "" })
}

local Options = Fluent.Options

do
----------------MOUNT------------------------------
    Tabs.Main:AddSection("TELEPORT MOUNT")

    local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

-- Teleport function
local function teleportToMount(mountName)
    local locations = {
        ["Mount 1"] = CFrame.new(4323.5293, 118.99543, -4783.34814),
        ["Mount 2"] = CFrame.new(-712.6026, 147.909653, -3606.24292),
        ["Mount 3"] = CFrame.new(-5439.15381, 107.441559, -5509.52051),
        ["Mount 4"] = CFrame.new(-5872.26514, 81.4962692, 395.888336),
        ["Mount 5"] = CFrame.new(-6140.83252, 78.16008, 5469.28516),
        ["Mount 6"] = CFrame.new(458.317688, 117.564835, 3432.43237),
        ["Mount 7"] = CFrame.new(3328.97119, 83.3834839, 17.4528008),
    }
    
    if not mountName or not locations[mountName] then
        warn("Invalid mount name: " .. tostring(mountName))
        return
    end
    
    local player = Players.LocalPlayer
    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local rootPart = player.Character.HumanoidRootPart
        local tween = TweenService:Create(rootPart, TweenInfo.new(13, Enum.EasingStyle.Linear), {CFrame = locations[mountName]})
        tween:Play()
    else
        warn("Player or HumanoidRootPart not found!")
    end
end

-- Dropdown for Mounts
local MultiDropdown = Tabs.Main:AddDropdown("MultiDropdown", {
    Title = "MOUNT ISLAND",
    Description = "เรียงนะสัส.",
    Values = {"Mount 1", "Mount 2", "Mount 3", "Mount 4", "Mount 5", "Mount 6", "Mount 7"},
    Multi = false,
    Default = "Mount 1",
    Callback = function(selectedMount)
        if type(selectedMount) == "table" then
            selectedMount = selectedMount[1]
        end
        teleportToMount(selectedMount)
    end
})




        
------------------ISLAND--------------------

    Tabs.Main:AddSection("TELEPORT TO ISLAND") 

        local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

-- Teleport function
local function teleportisland(islandName)
    local locations = {
        ["SOLOLEVING ISLAND"] = CFrame.new(575.553345, 28.4345264, 236.949524, -0.962383568, -1.79299803e-07, 0.271694422, -2.59595453e-07, 1, -2.59595311e-07, -0.271694422, -3.20360868e-07, -0.962383568),
        ["NARUTO ISLAND"] = CFrame.new(-3380.23486, 30.326519, 2257.26147, 1, 5.77654705e-19, -1.05042462e-14, -5.77654705e-19, 1, 4.39000519e-19, 1.05042462e-14, -4.39000519e-19, 1),
        ["ONE PIECE ISLAND"] = CFrame.new(-2845.69165, 50.0785561, -2008.58228, -0.789558649, -4.56584992e-08, -0.613675117, -2.59595396e-07, 1, 2.59595481e-07, 0.613675117, 3.64273092e-07, -0.789558649),
        ["BLEACH ISLAND"] = CFrame.new(2642.81836, 46.4265251, -2646.04565, -0.26154536, -3.18455022e-07, -0.965191185, -2.59595453e-07, 1, -2.59595254e-07, 0.965191185, 1.82663328e-07, -0.26154536),
        ["BLACK COVER ISLAND"] = CFrame.new(198.338684, 39.7076759, 4296.10938, 0.993159413, -8.36730089e-24, -0.116766132, 3.3985741e-24, 1, -4.27519066e-23, 0.116766132, 4.20626188e-23, 0.993159413),
        ["ANT ISLAND"] = CFrame.new(3934.61816, 59.1383743, 3200.93066, 0.798636198, -5.10941796e-08, 0.601814091, 2.59595481e-07, 1, -2.59595396e-07, -0.601814091, 3.63550498e-07, 0.798636198),
    }
    
    if not islandName or not locations[islandName] then
        warn("Invalid mount name: " .. tostring(islandName))
        return
    end
    
    local player = Players.LocalPlayer
    if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local rootPart = player.Character.HumanoidRootPart
        local tween = TweenService:Create(rootPart, TweenInfo.new(15, Enum.EasingStyle.Linear), {CFrame = locations[islandName]})
        tween:Play()
    else
        warn("Player or HumanoidRootPart not found!")
    end
end

------TELEPORT ISLAND-----------
local MultiDropdown = Tabs.Main:AddDropdown("MultiDropdown", {
    Title = "TELEPORT TO ISLAND",
    Description = "วาบค้าบน้อนๆ.",
    Values = {"SOLOLEVING ISLAND", "NARUTO ISLAND", "ONE PIECE ICELAND", "BLEACH ISLAND", "BLACK COVER ISLAND", "ANT ISLAND"},
    Multi = false,
    Default = "SOLOLEVING ISLAND",
    Callback = function(selectedisland)
        if type(selectedisland) == "table" then
            selectedisland = selectedisland[1]
        end
        teleportisland(selectedisland)
    end
})
end
end
----------SETTING---------------------------------------
local Tabs = {
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do
   -- Changed Tabs.Main to Tabs.Settings
   Tabs.Settings:AddButton({
        Title = "COPY CFRAME",
        Description = "COPY CFRAME",
        Callback = function()
            Window:Dialog({
                Title = "COPY CFRAME",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            print("Confirmed the dialog.")
                            setclipboard(tostring(game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame))
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })

end

