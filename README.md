local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
local VirtualInputManager = game:GetService("VirtualInputManager")

local Window = Rayfield:CreateWindow({
   Name = "azOoO",
   LoadingTitle = "azOoO",
   ConfigurationSaving = { Enabled = false }
})

local Tab = Window:CreateTab("azOoO", 4483362458)
_G.AzoFarm = false

local function PressE()
    for i = 1, 12 do
        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
        task.wait(0.05)
        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
    end
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
    bv.Velocity = Vector3.new(0,0,0)
    while (Vector2.new(hrp.Position.X, hrp.Position.Z) - Vector2.new(target.X, target.Z)).Magnitude > 6 and _G.AzoFarm do
        local dir = (Vector3.new(target.X, 82, target.Z) - hrp.Position).Unit
        bv.Velocity = dir * 60
        hrp.CFrame = CFrame.new(hrp.Position, Vector3.new(target.X, 82, target.Z))
        task.wait()
    end
    bv:Destroy()
    nc:Disconnect()
    hrp.CFrame = CFrame.new(target.X, target.Y + 1.5, target.Z)
    task.wait(0.3)
    if isPickup then PressE() end
end

Tab:CreateToggle({
   Name = "Start Farm",
   CurrentValue = false,
   Callback = function(Value)
      _G.AzoFarm = Value
      task.spawn(function()
         while _G.AzoFarm do
            Glide(Vector3.new(-38, 71, -3082), true)
            for _, pos in ipairs({Vector3.new(-392.1, 73, -2660.1), Vector3.new(366.9, 73.3, -2804.5), Vector3.new(-84.1, 73, -2715.4)}) do
                if not _G.AzoFarm then break end
                Glide(pos, false)
            end
            task.wait(0.5)
         end
      end)
   end,
})

Rayfield:Notify({Title = "azOoO", Content = "azOoO", Duration = 3})