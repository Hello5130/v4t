
local Nova = loadstring(game:HttpGet("https://raw.githubusercontent.com/Hello5130/v4t/refs/heads/main/v23"))()
ขอระบบ systems ที่มีมากขึ้น เช่น ระบบปิด gui , key systems และอื่นๆ
local Window = Nova:CreateWindow({
	Name = "Nova Hub",
	Title = "Nova Hub | My Map",
})

local Tab = Window:CreateTab("Main")

Tab:CreateSection("Player")

Tab:CreateToggle({
	Name = "Fly",
	CurrentValue = false,
	Callback = function(state)
		print("Fly:", state)
	end,
})

Tab:CreateSlider({
	Name = "WalkSpeed",
	Range = {16, 200},
	Increment = 1,
	CurrentValue = 16,
	Callback = function(v)
		local hum = game.Players.LocalPlayer.Character
			and game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
		if hum then hum.WalkSpeed = v end
	end,
})

Tab:CreateDropdown({
	Name = "Teleport",
	Options = {"Spawn", "Shop", "Arena"},
	CurrentOption = "Spawn",
	Callback = function(opt)
		print("Teleport to:", opt)
	end,
})

Tab:CreateKeybind({
	Name = "Toggle Menu Key",
	CurrentKeybind = "F",
	Callback = function(key) print("Bind set:", key) end,
	OnPressed = function() print("F pressed") end,
})

Tab:CreateButton({
	Name = "Reset Character",
	Callback = function()
		game.Players.LocalPlayer.Character:BreakJoints()
	end,
})
