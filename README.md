 --// Legends of Speed //--
 
getgenv().collectHoopsToggle = false
getgenv().collectOrbsToggle = false
getgenv().autoRebirth = false

function collectHoopsToggle() -- Auto Collects Hoops
    spawn(function()
        local playerHead = game.Players.LocalPlayer.Character.Head
        while wait() do
            if not getgenv().collectHoopsToggle then break end
            for i, v in pairs(game:GetService("Workspace").Hoops:GetDescendants()) do
                if v.Name == "TouchInterest" and v.Parent then
                    -- Fire the Touch Interest
                    firetouchinterest(playerHead, v.Parent, 0)
                    wait(0.01)
                    firetouchinterest(playerHead, v.Parent, 1)
                end
            end 
        end
    end)
end

function collectOrbsToggle() -- Auto Collects Orbs
    spawn(function()
        local playerHead = game.Players.LocalPlayer.Character.Head
        while wait() do
            if not getgenv().collectOrbsToggle then break end
            for i, v in pairs(game:GetService("Workspace").orbFolder:GetDescendants()) do
                if v.Name == "TouchInterest" and v.Parent then
                    -- Fire the Touch Interest
                    firetouchinterest(playerHead, v.Parent, 1)
                    wait(0.1)
                    firetouchinterest(playerHead, v.Parent, 0)
                end
            end
        end
    end)
end

function autoRebirth() -- Well it says auto rebirth, I'm sure we all know what that is
    spawn(function() 
        while wait(5) do
            if not getgenv().autoRebirth then break end
            local args = {[1] = "rebirthRequest"}
            game:GetService("ReplicatedStorage").rEvents.rebirthEvent:FireServer(unpack(args))
         end
    end)
end

spawn(function() -- Anti AFK
    local vu = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:connect(function()
    vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    wait(1)
    vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end) 
end)


local library = loadstring(game:HttpGet(('https://gist.githubusercontent.com/vlixby/5c55451c3ab57d6d83ce792a26baf409/raw/f014b6256dc4c18c068a79bebefb68bf09f595c2/gistfile1.txt')))()

local w = library:CreateWindow("Legends of Speed")

local b = w:CreateFolder("AutoFarm")

b:Toggle("Auto Hoops",function(bool)
    getgenv().collectHoopsToggle = bool
    print(shared.toggle)
    if bool then
        collectHoopsToggle()
    end
end)

b:Toggle("Auto Collect",function(bool)
    getgenv().collectOrbsToggle = bool
    print(shared.toggle)
    if bool then
        collectOrbsToggle()
    end
end)

b:Toggle("Auto Rebirth",function(bool)
    getgenv().autoRebirth = bool
    print(shared.toggle)
    if bool then
        autoRebirth()
    end
end)

b:DestroyGui()
