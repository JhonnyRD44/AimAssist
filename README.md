local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

local MAX_ANGLE = 15

local function getLookedAtPlayer()
	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Head") then
			local head = player.Character.Head.Position
			local directionToHead = (head - Camera.CFrame.Position).Unit
			local cameraLookVector = Camera.CFrame.LookVector
			local angle = math.deg(math.acos(directionToHead:Dot(cameraLookVector)))
			if angle < MAX_ANGLE then
				return player
			end
		end
	end
	return nil
end

RunService.RenderStepped:Connect(function()
	local target = getLookedAtPlayer()
	if target and target.Character and target.Character:FindFirstChild("Head") then
		local headPos = target.Character.Head.Position
		Camera.CFrame = CFrame.new(Camera.CFrame.Position, headPos)
	end
end)
