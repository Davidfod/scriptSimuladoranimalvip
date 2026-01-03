--[[
    DESENVOLVIDO POR: © CyberNoDry
    AUTO FARM SIMULADOR ANIMAL
]]

if not game:IsLoaded() then game.Loaded:Wait() end

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local VirtualUser = game:GetService("VirtualUser")

local player = Players.LocalPlayer
local active = false

-- ANTI AFK (Adicionado sem apagar nada)
player.Idled:Connect(function()
    VirtualUser:CaptureController()
    VirtualUser:ClickButton2(Vector2.new())
end)

local function getRoot()
    local char = player.Character or player.CharacterAdded:Wait()
    return char:WaitForChild("HumanoidRootPart", 5)
end

local function CreateUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "CyberNoDry_Final_V2"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.Parent = player:WaitForChild("PlayerGui")

    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.new(0, 260, 0, 180)
    MainFrame.Position = UDim2.new(0.5, -130, 0.5, -90)
    MainFrame.BackgroundColor3 = Color3.fromRGB(12, 12, 12)
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true 
    MainFrame.Parent = ScreenGui

    Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 10)
    local stroke = Instance.new("UIStroke", MainFrame)
    stroke.Thickness = 2
    stroke.Color = Color3.fromRGB(0, 170, 255)

    local Title = Instance.new("TextLabel")
    Title.Size = UDim2.new(1, 0, 0, 40)
    Title.Text = "Auto Farm Simulador Animal"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.BackgroundTransparency = 1
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 14
    Title.Parent = MainFrame

    local TeleportBtn = Instance.new("TextButton")
    TeleportBtn.Size = UDim2.new(0.85, 0, 0, 50)
    TeleportBtn.Position = UDim2.new(0.075, 0, 0.4, 0)
    TeleportBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    TeleportBtn.Text = "LIGAR FARM (FAST)"
    TeleportBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
    TeleportBtn.Font = Enum.Font.GothamBold
    TeleportBtn.TextSize = 14
    TeleportBtn.Parent = MainFrame
    Instance.new("UICorner", TeleportBtn)

    local Copy = Instance.new("TextLabel")
    Copy.Size = UDim2.new(1, 0, 0, 20)
    Copy.Position = UDim2.new(0, 0, 1, -25)
    Copy.Text = "Direitos autorais: © CyberNoDry"
    Copy.TextColor3 = Color3.fromRGB(100, 100, 100)
    Copy.BackgroundTransparency = 1
    Copy.Font = Enum.Font.Gotham
    Copy.TextSize = 11
    Copy.Parent = MainFrame

    local Close = Instance.new("TextButton")
    Close.Size = UDim2.new(0, 25, 0, 25)
    Close.Position = UDim2.new(1, -30, 0, 5)
    Close.Text = "X"
    Close.BackgroundColor3 = Color3.fromRGB(150, 0, 0)
    Close.TextColor3 = Color3.fromRGB(255, 255, 255)
    Close.Parent = MainFrame
    Instance.new("UICorner", Close)

    local Min = Instance.new("TextButton")
    Min.Size = UDim2.new(0, 25, 0, 25)
    Min.Position = UDim2.new(1, -60, 0, 5)
    Min.Text = "-"
    Min.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    Min.TextColor3 = Color3.fromRGB(255, 255, 255)
    Min.Parent = MainFrame
    Instance.new("UICorner", Min)

    local function doFarm()
        while active do
            local container = Workspace:FindFirstChild("CoinContainer")
            
            if container then
                local found = false
                for _, obj in ipairs(container:GetDescendants()) do
                    if not active then break end
                    
                    if (obj.Name == "Ches" or obj.Name == "Chest") and obj:IsA("BasePart") then
                        found = true
                        local root = getRoot()
                        
                        if root and root.Parent:FindFirstChild("Humanoid") and root.Parent.Humanoid.Health > 0 then
                            
                            -- ANTI SIT (Adicionado sem apagar nada)
                            if root.Parent.Humanoid.Sit then root.Parent.Humanoid.Jump = true end
                            
                            root.Velocity = Vector3.new(0,0,0)
                            root.CFrame = obj.CFrame
                            task.wait(0.01)
                        end
                    end
                end
                
                if not found then
                    task.wait(0.1) 
                end
            else
                task.wait(0.5)
            end
            task.wait()
        end
    end

    TeleportBtn.MouseButton1Click:Connect(function()
        active = not active
        if active then
            TeleportBtn.Text = "FARM: ATIVO ✅"
            TeleportBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
            task.spawn(doFarm)
        else
            TeleportBtn.Text = "LIGAR FARM (FAST)"
            TeleportBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        end
    end)

    Close.MouseButton1Click:Connect(function() 
        active = false
        ScreenGui:Destroy() 
    end)

    local isMin = false
    Min.MouseButton1Click:Connect(function()
        isMin = not isMin
        local targetSize = isMin and UDim2.new(0, 260, 0, 40) or UDim2.new(0, 260, 0, 180)
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = targetSize}):Play()
        TeleportBtn.Visible = not isMin
        Copy.Visible = not isMin
    end)
end

CreateUI()
