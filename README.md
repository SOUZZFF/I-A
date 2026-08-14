--// PLAYER ESP
--// LocalScript - Roblox Studio
--// Coloque em StarterPlayer > StarterPlayerScripts

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer

local ESP_ENABLED = true

local ESP_FOLDER = Instance.new("Folder")
ESP_FOLDER.Name = "PlayerESP"
ESP_FOLDER.Parent = workspace

local espObjects = {}

local function removeESP(player)
	if espObjects[player] then
		for _, object in pairs(espObjects[player]) do
			if object and object.Parent then
				object:Destroy()
			end
		end

		espObjects[player] = nil
	end
end

local function createESP(player)
	if player == LocalPlayer then
		return
	end

	removeESP(player)

	local character = player.Character
	if not character then
		return
	end

	local root = character:FindFirstChild("HumanoidRootPart")
	if not root then
		return
	end

	--// BOX
	local highlight = Instance.new("Highlight")
	highlight.Name = "ESP_Box"
	highlight.Adornee = character
	highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
	highlight.FillTransparency = 1
	highlight.OutlineTransparency = 0
	highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
	highlight.Parent = ESP_FOLDER

	--// GUI
	local billboard = Instance.new("BillboardGui")
	billboard.Name = "ESP_Info"
	billboard.Adornee = root
	billboard.Size = UDim2.new(0, 200, 0, 60)
	billboard.StudsOffset = Vector3.new(0, 3, 0)
	billboard.AlwaysOnTop = true
	billboard.Parent = ESP_FOLDER

	local text = Instance.new("TextLabel")
	text.Name = "Info"
	text.Size = UDim2.fromScale(1, 1)
	text.BackgroundTransparency = 1
	text.TextColor3 = Color3.fromRGB(255, 255, 255)
	text.TextStrokeTransparency = 0
	text.TextScaled = false
	text.TextSize = 14
	text.Font = Enum.Font.GothamBold
	text.Parent = billboard

	espObjects[player] = {
		highlight = highlight,
		billboard = billboard,
		text = text
	}
end

local function setESP(state)
	ESP_ENABLED = state

	for player, objects in pairs(espObjects) do
		if objects then
			for _, object in pairs(objects) do
				if object and object.Parent then
					object.Enabled = state
				end
			end

			if objects.text then
				objects.text.Visible = state
			end
		end
	end

	if state then
		for _, player in ipairs(Players:GetPlayers()) do
			if player ~= LocalPlayer then
				createESP(player)
			end
		end
	else
		for player in pairs(espObjects) do
			removeESP(player)
		end
	end
end

--// Atualiza distância
RunService.RenderStepped:Connect(function()
	if not ESP_ENABLED then
		return
	end

	local localCharacter = LocalPlayer.Character
	if not localCharacter then
		return
	end

	local localRoot = localCharacter:FindFirstChild("HumanoidRootPart")
	if not localRoot then
		return
	end

	for player, objects in pairs(espObjects) do
		if player.Character and objects and objects.text then
			local targetRoot = player.Character:FindFirstChild("HumanoidRootPart")

			if targetRoot then
				local distance = (localRoot.Position - targetRoot.Position).Magnitude

				objects.text.Text =
					player.DisplayName ..
					"\n[" .. math.floor(distance) .. " studs]"
			end
		end
	end
end)

--// Jogador entrou
Players.PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function()
		task.wait(0.5)

		if ESP_ENABLED then
			createESP(player)
		end
	end)
end)

--// Personagem respawnou
for _, player in ipairs(Players:GetPlayers()) do
	if player ~= LocalPlayer then
		player.CharacterAdded:Connect(function()
			task.wait(0.5)

			if ESP_ENABLED then
				createESP(player)
			end
		end)

		if player.Character and ESP_ENABLED then
			createESP(player)
		end
	end
end

--// Jogador saiu
Players.PlayerRemoving:Connect(function(player)
	removeESP(player)
end)

--// =====================================================
--// TOGGLE
--// =====================================================

local function toggleESP()
	setESP(not ESP_ENABLED)
	print("ESP:", ESP_ENABLED and "ATIVADO" or "DESATIVADO")
end

-- Começa ativado
setESP(true)

-- Você pode chamar:
-- toggleESP()
-- para ligar/desligar o ESP.
