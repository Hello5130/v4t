
local Nova = loadstring(game:HttpGet("https://raw.githubusercontent.com/Hello5130/v4t/refs/heads/main/v23"))()


--===========================================================
-- KEY SYSTEM
--===========================================================
Nova:VerifyKey({
    Title = "Nova Speed Hub",
    Note = "กรุณากรอก Key เพื่อใช้งาน (ลอง: FREE)",
    Keys = {"FREE", "SPEED", "VIP123"},
    Callback = function()
        loadSpeedUI()
    end
})

--===========================================================
-- MAIN UI
--===========================================================
function loadSpeedUI()
    local Window = Nova:CreateWindow({
        Name = "SpeedGUI",
        Title = "Nova Speed | WalkSpeed",
        ConfigName = "SpeedConfig", -- บันทึกค่าอัตโนมัติ
    })

    -- แจ้งเตือนเข้าใช้งาน
    Window:Notify({
        Title = "พร้อมใช้งาน!",
        Content = "ระบบวิ่งเร็วโหลดเสร็จแล้ว",
        Duration = 3
    })

    local Tab = Window:CreateTab("Movement")

    -- Section วิ่งเร็ว
    Tab:CreateSection("ความเร็วตัวละคร")

    -- ตัวแปรเก็บสถานะ
    local speedEnabled = false
    local defaultSpeed = 16 -- ความเร็วปกติของ Roblox
    local currentSpeed = defaultSpeed

    -- Toggle เปิด/ปิด
    local speedToggle = Tab:CreateToggle({
        Name = "Enable WalkSpeed",
        CurrentValue = false,
        Flag = "SpeedEnabled",
        Callback = function(Value)
            speedEnabled = Value
            local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
            
            if humanoid then
                if speedEnabled then
                    humanoid.WalkSpeed = currentSpeed
                    Window:Notify({
                        Title = "WalkSpeed",
                        Content = "เปิดแล้ว! ความเร็ว: " .. currentSpeed,
                        Duration = 2
                    })
                else
                    humanoid.WalkSpeed = defaultSpeed
                    Window:Notify({
                        Title = "WalkSpeed",
                        Content = "ปิดแล้ว กลับสู่ค่าปกติ",
                        Duration = 2
                    })
                end
            end
        end
    })

    -- Slider ปรับความเร็ว
    Tab:CreateSlider({
        Name = "Speed Value",
        Range = {16, 500},
        Increment = 1,
        CurrentValue = 100,
        Flag = "SpeedValue",
        Callback = function(Value)
            currentSpeed = Value
            if speedEnabled then
                local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid.WalkSpeed = currentSpeed
                end
            end
            Window:Notify({
                Title = "ปรับความเร็ว",
                Content = "ตั้งค่าเป็น: " .. Value,
                Duration = 1.5
            })
        end
    })

    -- Keybind เปิด/ปิดด่วน
    Tab:CreateKeybind({
        Name = "Speed Toggle Key",
        CurrentKeybind = "LeftShift",
        Flag = "SpeedKeybind",
        Callback = function(Key)
            print("ตั้งค่าปุ่มใหม่:", Key)
        end,
        OnPressed = function()
            speedToggle:Set(not speedEnabled)
        end
    })

    -- Section อื่นๆ
    Tab:CreateSection("เพิ่มเติม")

    Tab:CreateButton({
        Name = "รีเซ็ตความเร็ว (16)",
        Callback = function()
            currentSpeed = 16
            speedToggle:Set(false)
            Window:Notify({
                Title = "รีเซ็ต",
                Content = "ความเร็วกลับเป็น 16 แล้ว",
                Duration = 2
            })
        end
    })

    -- จับตัวละครเกิดใหม่ (Respawn)
    game.Players.LocalPlayer.CharacterAdded:Connect(function(char)
        task.wait(0.5)
        local humanoid = char:WaitForChild("Humanoid")
        if speedEnabled then
            humanoid.WalkSpeed = currentSpeed
        end
    end)
end
