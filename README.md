--// Services
local player = game:GetService("Players").LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local soundService = game:GetService("SoundService")

--// ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = playerGui
screenGui.Name = "VjrsuuGUI"

--// Main Frame
local mainFrame = Instance.new("Frame")
mainFrame.Size = UDim2.new(0, 400, 0, 250)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -125)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BackgroundTransparency = 0.2
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.Parent = screenGui

local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 12)
uiCorner.Parent = mainFrame

--// Title
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -80, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Vjrsuu Inc - Free Version"
title.Font = Enum.Font.GothamBold
title.TextSize = 24
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Parent = mainFrame

--// Sounds
local function playSound(soundId)
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://"..soundId
    sound.Volume = 1
    sound.Parent = workspace
    sound:Play()
    game:GetService("Debris"):AddItem(sound, 3)
end

--// Top-right buttons
local function createTopRightButton(name,text,posX,func)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,35,0,25)
    btn.Position = UDim2.new(1, posX, 0, 7)
    btn.BackgroundColor3 = Color3.fromRGB(0,0,0)
    btn.BackgroundTransparency = 0.5
    btn.Text = text
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16
    btn.Parent = mainFrame
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0,4)
    corner.Parent = btn
    btn.MouseButton1Click:Connect(func)
end

-- Close
createTopRightButton("Close","X",-40,function() screenGui:Destroy() end)

-- Minimize
local launcherButton = Instance.new("TextButton")
launcherButton.Size = UDim2.new(0,20,0,20)
launcherButton.Position = UDim2.new(0,10,1,-30)
launcherButton.BackgroundColor3 = Color3.fromRGB(0,0,0)
launcherButton.BackgroundTransparency = 0.5
launcherButton.Text = "VJ"
launcherButton.TextColor3 = Color3.fromRGB(255,255,255)
launcherButton.Font = Enum.Font.GothamBold
launcherButton.TextSize = 12
launcherButton.Visible = false
launcherButton.Parent = screenGui
local launcherCorner = Instance.new("UICorner")
launcherCorner.CornerRadius = UDim.new(0,4)
launcherCorner.Parent = launcherButton

createTopRightButton("Minimize","-", -80, function()
    playSound("82845990304289") -- Minimize sound
    mainFrame.Visible = false
    launcherButton.Visible = true
end)

launcherButton.MouseButton1Click:Connect(function()
    playSound("82845990304289") -- Maximize sound
    mainFrame.Visible = true
    launcherButton.Visible = false
end)

--// Bottom-right Avatar + Username
local bottomInfoFrame = Instance.new("Frame")
bottomInfoFrame.Size = UDim2.new(0,100,0,20)
bottomInfoFrame.Position = UDim2.new(1,-110,1,-30)
bottomInfoFrame.BackgroundTransparency = 1
bottomInfoFrame.Parent = mainFrame

local avatar = Instance.new("ImageLabel")
avatar.Size = UDim2.new(0,20,0,20)
avatar.Position = UDim2.new(0,0,0,0)
avatar.BackgroundTransparency = 1
avatar.Image = "https://www.roblox.com/headshot-thumbnail/image?userId="..player.UserId.."&width=420&height=420&format=png"
avatar.Parent = bottomInfoFrame

local bottomUsernameLabel = Instance.new("TextLabel")
bottomUsernameLabel.Size = UDim2.new(1,-25,1,0)
bottomUsernameLabel.Position = UDim2.new(0,25,0,0)
bottomUsernameLabel.BackgroundTransparency = 1
bottomUsernameLabel.Text = player.Name
bottomUsernameLabel.Font = Enum.Font.Gotham
bottomUsernameLabel.TextSize = 12
bottomUsernameLabel.TextColor3 = Color3.fromRGB(255,255,255)
bottomUsernameLabel.TextXAlignment = Enum.TextXAlignment.Left
bottomUsernameLabel.TextTruncate = Enum.TextTruncate.AtEnd
bottomUsernameLabel.Parent = bottomInfoFrame

--// Sidebar
local sidebar = Instance.new("ScrollingFrame")
sidebar.Size = UDim2.new(0,120,1,-40)
sidebar.Position = UDim2.new(0,0,0,40)
sidebar.BackgroundColor3 = Color3.fromRGB(0,0,0)
sidebar.BackgroundTransparency = 0.5
sidebar.BorderSizePixel = 0
sidebar.CanvasSize = UDim2.new(0,0,0,200)
sidebar.ScrollBarThickness = 6
sidebar.Parent = mainFrame

local sidebarCorner = Instance.new("UICorner")
sidebarCorner.CornerRadius = UDim.new(0,8)
sidebarCorner.Parent = sidebar

--// Main content frame
local contentFrame = Instance.new("ScrollingFrame")
contentFrame.Size = UDim2.new(1,-120,1,-40)
contentFrame.Position = UDim2.new(0,120,0,40)
contentFrame.BackgroundColor3 = Color3.fromRGB(0,0,0)
contentFrame.BackgroundTransparency = 0.5
contentFrame.BorderSizePixel = 0
contentFrame.CanvasSize = UDim2.new(0,0,2,0)
contentFrame.ScrollBarThickness = 6
contentFrame.Parent = mainFrame

local contentCorner = Instance.new("UICorner")
contentCorner.CornerRadius = UDim.new(0,8)
contentCorner.Parent = contentFrame

--// Sidebar buttons
local sidebarOptions = {
    ["Kill-Parts"] = {"Sabre","Officer's Sabre","Heavy Sabre"},
}

local function addHoverGlow(button)
    local originalTransparency = button.BackgroundTransparency
    button.MouseEnter:Connect(function()
        button.BackgroundTransparency = 0.2
    end)
    button.MouseLeave:Connect(function()
        button.BackgroundTransparency = originalTransparency
    end)
end

--// Create sidebar and content buttons
local function createSidebar()
    for i, optionName in ipairs({"Kill-Parts"}) do
        local btn = Instance.new("TextButton")
        btn.Size = UDim2.new(1,-10,0,40)
        btn.Position = UDim2.new(0,5,0,(i-1)*50 + 5)
        btn.BackgroundColor3 = Color3.fromRGB(0,0,0)
        btn.BackgroundTransparency = 0.5
        btn.Text = optionName
        btn.TextColor3 = Color3.fromRGB(255,255,255)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 18
        btn.Parent = sidebar

        local btnCorner = Instance.new("UICorner")
        btnCorner.CornerRadius = UDim.new(0,6)
        btnCorner.Parent = btn

        addHoverGlow(btn)

        btn.MouseButton1Click:Connect(function()
            playSound("15675032796") -- Folder click sound

            -- Clear previous content
            for _, child in ipairs(contentFrame:GetChildren()) do
                if child:IsA("TextButton") then
                    child:Destroy()
                end
            end
            local buttons = sidebarOptions[optionName]
            for j, bName in ipairs(buttons) do
                local b = Instance.new("TextButton")
                b.Size = UDim2.new(0,200,0,40)
                b.Position = UDim2.new(0,20,0,(j-1)*50 + 10)
                b.BackgroundColor3 = Color3.fromRGB(0,0,0)
                b.BackgroundTransparency = 0.5
                b.Text = bName
                b.TextColor3 = Color3.fromRGB(255,255,255)
                b.Font = Enum.Font.Gotham
                b.TextSize = 18
                b.Parent = contentFrame

                local corner = Instance.new("UICorner")
                corner.CornerRadius = UDim.new(0,6)
                corner.Parent = b

                addHoverGlow(b)

                -- Connect individual button functions
                b.MouseButton1Click:Connect(function()
                    playSound("15675032796") -- Button click sound
                    KillPartButtons[bName]() -- Call function at bottom
                end)
            end
            contentFrame.CanvasSize = UDim2.new(0,0,#buttons*50 + 20,0)
        end)
    end
end

-- Initialize sidebar
createSidebar()

--// Kill-Parts Button Functions
KillPartButtons = {
    ["Sabre"] = function()
        local LocalPlayer = game:GetService("Players").LocalPlayer local raycastParams = RaycastParams.new() raycastParams.FilterDescendantsInstances = {LocalPlayer.Character} raycastParams.FilterType = Enum.RaycastFilterType.Exclude local params = OverlapParams.new() params.FilterDescendantsInstances = {LocalPlayer.Character} function detectEnemy(hitbox, hrp) 	while true do 		local parts = workspace:GetPartsInPart(hitbox, params) 		for i, part in pairs(parts) do 			if part.Parent.Name == "m_Zombie" then 				local Origin = part.Parent:WaitForChild("Orig") 				if Origin.Value ~= nil then 					local zombie = Origin.Value:WaitForChild("Zombie") 					if LocalPlayer.Character:FindFirstChild("Sabre") and LocalPlayer.Character["Sabre"]:IsA("Tool") then 						local hit = Origin.Value 						local zombieHead = workspace:Raycast(hrp.CFrame.Position, hit.Head.CFrame.Position - hrp.CFrame.Position) 						local calc = (zombieHead.Position - hrp.CFrame.Position) 						if calc:Dot(calc) > 1 then 							calc = calc.Unit 			end 						if LocalPlayer:DistanceFromCharacter(Origin.Value:FindFirstChild("HumanoidRootPart").CFrame.Position) < 13 then 							if zombie.WalkSpeed > 16 then 								game:GetService("ReplicatedStorage").Remotes.Gib:FireServer(hit, "Head", hit.Head.CFrame.Position, zombieHead.Normal, true) 								game:GetService("Workspace").Players[LocalPlayer.Name]["Sabre"].RemoteEvent:FireServer("Swing", "Thrust") 					game:GetService("Workspace").Players[LocalPlayer.Name]["Sabre"].RemoteEvent:FireServer("HitZombie", hit, hit.Head.CFrame.Position, true, calc * 25, "Head", zombieHead.Normal) 					else 						if part.Parent:FindFirstChild("Barrel") == nil then 							game:GetService("ReplicatedStorage").Remotes.Gib:FireServer(hit, "Head", hit.Head.CFrame.Position, zombieHead.Normal, true) 									game:GetService("Workspace").Players[LocalPlayer.Name]["Sabre"].RemoteEvent:FireServer("Swing", "Thrust") 						game:GetService("Workspace").Players[LocalPlayer.Name]["Sabre"].RemoteEvent:FireServer("HitZombie", hit, hit.Head.CFrame.Position, true, calc * 25, "Head", zombieHead.Normal) 								end 							end 						end 					end 				end	 			end 		end 	task.wait(0.1) 	end end workspace.Players.ChildAdded:Connect(function(child) 	if child.Name == LocalPlayer.Name then 		local torso = child:WaitForChild("HumanoidRootPart") 		local hitbox = Instance.new("Part", torso) 		local weld = Instance.new("WeldConstraint", torso) 		weld.Part0 = hitbox 		weld.Part1 = child.HumanoidRootPart 		hitbox.Name = "Hitbox" 		hitbox.Anchored = false 		hitbox.Massless = true 		hitbox.CanCollide = false 		hitbox.CanTouch = true 		hitbox.Transparency = 0.95 		hitbox.Size = Vector3.new(1, 4, 1) 		hitbox.Position = Vector3.new(torso.Position.X, torso.Position.Y, torso.Position.Z-5.8) 		detectEnemy(LocalPlayer.Character.HumanoidRootPart:WaitForChild("Hitbox"), LocalPlayer.Character.HumanoidRootPart) 	end end) 

    end,
    ["Officer's Sabre"] = function()
        local LocalPlayer = game:GetService("Players").LocalPlayer local raycastParams = RaycastParams.new() raycastParams.FilterDescendantsInstances = {LocalPlayer.Character} raycastParams.FilterType = Enum.RaycastFilterType.Exclude local params = OverlapParams.new() params.FilterDescendantsInstances = {LocalPlayer.Character} function detectEnemy(hitbox, hrp) 	while true do 		local parts = workspace:GetPartsInPart(hitbox, params) 		for i, part in pairs(parts) do 			if part.Parent.Name == "m_Zombie" then 				local Origin = part.Parent:WaitForChild("Orig") 				if Origin.Value ~= nil then 					local zombie = Origin.Value:WaitForChild("Zombie") 					if LocalPlayer.Character:FindFirstChild("Officer's Sabre") and LocalPlayer.Character["Officer's Sabre"]:IsA("Tool") then 						local hit = Origin.Value 						local zombieHead = workspace:Raycast(hrp.CFrame.Position, hit.Head.CFrame.Position - hrp.CFrame.Position) 						local calc = (zombieHead.Position - hrp.CFrame.Position) 						if calc:Dot(calc) > 1 then 							calc = calc.Unit 			end 						if LocalPlayer:DistanceFromCharacter(Origin.Value:FindFirstChild("HumanoidRootPart").CFrame.Position) < 13 then 							if zombie.WalkSpeed > 16 then 								game:GetService("ReplicatedStorage").Remotes.Gib:FireServer(hit, "Head", hit.Head.CFrame.Position, zombieHead.Normal, true) 								game:GetService("Workspace").Players[LocalPlayer.Name]["Officer's Sabre"].RemoteEvent:FireServer("Swing", "Thrust") 					game:GetService("Workspace").Players[LocalPlayer.Name]["Officer's Sabre"].RemoteEvent:FireServer("HitZombie", hit, hit.Head.CFrame.Position, true, calc * 25, "Head", zombieHead.Normal) 					else 						if part.Parent:FindFirstChild("Barrel") == nil then 							game:GetService("ReplicatedStorage").Remotes.Gib:FireServer(hit, "Head", hit.Head.CFrame.Position, zombieHead.Normal, true) 									game:GetService("Workspace").Players[LocalPlayer.Name]["Officer's Sabre"].RemoteEvent:FireServer("Swing", "Thrust") 						game:GetService("Workspace").Players[LocalPlayer.Name]["Officer's Sabre"].RemoteEvent:FireServer("HitZombie", hit, hit.Head.CFrame.Position, true, calc * 25, "Head", zombieHead.Normal) 								end 							end 						end 					end 				end	 			end 		end 	task.wait(0.1) 	end end workspace.Players.ChildAdded:Connect(function(child) 	if child.Name == LocalPlayer.Name then 		local torso = child:WaitForChild("HumanoidRootPart") 		local hitbox = Instance.new("Part", torso) 		local weld = Instance.new("WeldConstraint", torso) 		weld.Part0 = hitbox 		weld.Part1 = child.HumanoidRootPart 		hitbox.Name = "Hitbox" 		hitbox.Anchored = false 		hitbox.Massless = true 		hitbox.CanCollide = false 		hitbox.CanTouch = true 		hitbox.Transparency = 0.95 		hitbox.Size = Vector3.new(1, 4, 1) 		hitbox.Position = Vector3.new(torso.Position.X, torso.Position.Y, torso.Position.Z-5.8) 		detectEnemy(LocalPlayer.Character.HumanoidRootPart:WaitForChild("Hitbox"), LocalPlayer.Character.HumanoidRootPart) 	end end) 

    end,
    ["Heavy Sabre"] = function()
        local LocalPlayer = game:GetService("Players").LocalPlayer local raycastParams = RaycastParams.new() raycastParams.FilterDescendantsInstances = {LocalPlayer.Character} raycastParams.FilterType = Enum.RaycastFilterType.Exclude local params = OverlapParams.new() params.FilterDescendantsInstances = {LocalPlayer.Character} function detectEnemy(hitbox, hrp) 	while true do 		local parts = workspace:GetPartsInPart(hitbox, params) 		for i, part in pairs(parts) do 			if part.Parent.Name == "m_Zombie" then 				local Origin = part.Parent:WaitForChild("Orig") 				if Origin.Value ~= nil then 					local zombie = Origin.Value:WaitForChild("Zombie") 					if LocalPlayer.Character:FindFirstChild("Heavy Sabre") and LocalPlayer.Character["Heavy Sabre"]:IsA("Tool") then 						local hit = Origin.Value 						local zombieHead = workspace:Raycast(hrp.CFrame.Position, hit.Head.CFrame.Position - hrp.CFrame.Position) 						local calc = (zombieHead.Position - hrp.CFrame.Position) 						if calc:Dot(calc) > 1 then 							calc = calc.Unit 			end 						if LocalPlayer:DistanceFromCharacter(Origin.Value:FindFirstChild("HumanoidRootPart").CFrame.Position) < 13 then 							if zombie.WalkSpeed > 16 then 								game:GetService("ReplicatedStorage").Remotes.Gib:FireServer(hit, "Head", hit.Head.CFrame.Position, zombieHead.Normal, true) 								game:GetService("Workspace").Players[LocalPlayer.Name]["Heavy Sabre"].RemoteEvent:FireServer("Swing", "Thrust") 					game:GetService("Workspace").Players[LocalPlayer.Name]["Heavy Sabre"].RemoteEvent:FireServer("HitZombie", hit, hit.Head.CFrame.Position, true, calc * 25, "Head", zombieHead.Normal) 					else 						if part.Parent:FindFirstChild("Barrel") == nil then 							game:GetService("ReplicatedStorage").Remotes.Gib:FireServer(hit, "Head", hit.Head.CFrame.Position, zombieHead.Normal, true) 									game:GetService("Workspace").Players[LocalPlayer.Name]["Heavy Sabre"].RemoteEvent:FireServer("Swing", "Thrust") 						game:GetService("Workspace").Players[LocalPlayer.Name]["Heavy Sabre"].RemoteEvent:FireServer("HitZombie", hit, hit.Head.CFrame.Position, true, calc * 25, "Head", zombieHead.Normal) 								end 							end 						end 					end 				end	 			end 		end 	task.wait(0.1) 	end end workspace.Players.ChildAdded:Connect(function(child) 	if child.Name == LocalPlayer.Name then 		local torso = child:WaitForChild("HumanoidRootPart") 		local hitbox = Instance.new("Part", torso) 		local weld = Instance.new("WeldConstraint", torso) 		weld.Part0 = hitbox 		weld.Part1 = child.HumanoidRootPart 		hitbox.Name = "Hitbox" 		hitbox.Anchored = false 		hitbox.Massless = true 		hitbox.CanCollide = false 		hitbox.CanTouch = true 		hitbox.Transparency = 0.95 		hitbox.Size = Vector3.new(1, 4, 1) 		hitbox.Position = Vector3.new(torso.Position.X, torso.Position.Y, torso.Position.Z-7.8) 		detectEnemy(LocalPlayer.Character.HumanoidRootPart:WaitForChild("Hitbox"), LocalPlayer.Character.HumanoidRootPart) 	end end)
    end
}
