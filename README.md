local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local VirtualInputManager = game:GetService("VirtualInputManager")

local Window = Rayfield:CreateWindow({
   Name = "azOoO Fix v2",
   LoadingTitle = "Anti-Back System...",
   ConfigurationSaving = { Enabled = false }
})

local Tab = Window:CreateTab("Auto Farm", 4483362458)
_G.AzoFarm = false

-- تطوير ضغطة الـ E مع تثبيت الموقع
local function PressE(currentPos)
    for i = 1, 15 do
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
        task.wait(0.03)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
    end
    -- أهم خطوة: تثبيت الشخصية في مكانها لمدة بسيطة عشان ما ترجعك اللعبة
    local hrp = game.Players.LocalPlayer.Character:WaitForChild("HumanoidRootPart")
    hrp.Velocity = Vector3.new(0,0,0)
    task.wait(0.4) -- وقت مستقطع للعبة عشان تسجل الأخذ
end

local function Glide(target, isPickup)
    if not _G.AzoFarm then return end
    local char = game.Players.LocalPlayer.Character
    local hrp = char:WaitForChild("HumanoidRootPart")
    
    local nc = game:GetService("RunService").Stepped:Connect(function()
        for _, v in pairs(char:GetDescendants()) do
            if v:IsA("BasePart") then v.CanCollide = false end
        end
    end)

    local bv = Instance.new("BodyVelocity", hrp)
    bv.MaxForce = Vector3.new(1e6, 1e6, 1e6)
    
    -- رفع الارتفاع لـ 88 لتجنب أي عوائق (أعلى من السور)
    while (Vector2.new(hrp.Position.X, hrp.Position.Z) - Vector2.new(target.X, target.Z)).Magnitude > 6 and _G.AzoFarm do
        local dir = (Vector3.new(target.X, 88, target.Z) - hrp.Position).Unit
        bv.Velocity = dir * 70 
        hrp.CFrame = CFrame.new(hrp.Position, Vector3.new(target.X, 88, target.Z))
        task.wait()
    end
    
    bv:Destroy()
    nc:Disconnect()
    
    -- النزول الدقيق
    hrp.CFrame = CFrame.new(target.X, target.Y + 1.2, target.Z)
    task.wait(0.2)
    
    if isPickup then 
        PressE(target) 
    end
end

Tab:CreateToggle({
   Name = "Start Farm (No Back)",
   CurrentValue = false,
   Callback = function(Value)
      _G.AzoFarm = Value
      task.spawn(function()
         while _G.AzoFarm do
            -- أخذ الصندوق
            Glide(Vector3.new(-38, 71, -3082), true)
            
            -- التسليم
            if _G.AzoFarm then
                for _, pos in ipairs({
                    Vector3.new(-392.1, 73, -2660.1),
                    Vector3.new(366.9, 73.3, -2804.5),
                    Vector3.new(-84.1, 73, -2715.4)
                }) do
                    if not _G.AzoFarm then break end
                    Glide(pos, false)
                    task.wait(0.5)
                end
            end
         end
      end)
   end,
})
