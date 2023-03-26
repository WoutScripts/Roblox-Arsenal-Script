-- Snuffel's Arsenal Script --

-- ESP colors --
local espColors = {
  Color3.fromRGB(255, 0, 0), -- Red
  Color3.fromRGB(0, 255, 0), -- Green
  Color3.fromRGB(0, 0, 255), -- Blue
  Color3.fromRGB(255, 255, 255), -- White
  Color3.fromRGB(255, 255, 0) -- Yellow
}

-- Rainbow ESP color function --
local function rainbowColor()
  local frequency = 0.1
  local r = math.sin(frequency*tick() + 0) * 127 + 128
  local g = math.sin(frequency*tick() + 2) * 127 + 128
  local b = math.sin(frequency*tick() + 4) * 127 + 128
  return Color3.fromRGB(r, g, b)
end

-- ESP function --
local function esp(part, color)
  local espBox = Instance.new("BoxHandleAdornment", part)
  espBox.Size = part.Size
  espBox.Adornee = part
  espBox.AlwaysOnTop = true
  espBox.Transparency = 0.5
  espBox.Color3 = color
end

-- Aimbot function --
local function aimbot(enemy)
  local player = game.Players.LocalPlayer.Character
  local headPos = enemy.Head.Position
  local torsoPos = enemy.Torso.Position
  local aimPos = headPos - torsoPos
  player.HumanoidRootPart.CFrame = CFrame.new(headPos + aimPos.Unit * 5)
end

-- Main loop --
while true do
  for _, enemy in pairs(game.Workspace.Characters:GetChildren()) do
    if enemy ~= game.Players.LocalPlayer.Character and enemy.Humanoid.Health > 0 then
      local espColor = espColors[math.random(1, #espColors)]
      esp(enemy.Head, espColor)
      esp(enemy.Torso, espColor)
      esp(enemy.LeftFoot, espColor)
      esp(enemy.RightFoot, espColor)
      esp(enemy.LeftHand, espColor)
      esp(enemy.RightHand, espColor)
      esp(enemy:FindFirstChild("HumanoidRootPart"), espColor)
      esp(enemy:FindFirstChild("HumanoidRootPart").DescendantAdded:Connect(function(descendant)
        if descendant:IsA("BasePart") then
          esp(descendant, espColor)
        end
      end))
      aimbot(enemy)
    end
  end
  wait()
end

-- GUI creation --
local gui = Instance.new("ScreenGui")
gui.Name = "Snuffel's GUI"
gui.Parent = game.Players.LocalPlayer.PlayerGui
