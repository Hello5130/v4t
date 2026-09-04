
local Nova = loadstring(game:HttpGet("https://raw.githubusercontent.com/Hello5130/v4t/refs/heads/main/v23"))()

--===========================================================
-- 1. KEY SYSTEM (หน้ากรอก Key ก่อนเข้าใช้งาน)
--===========================================================
Nova:VerifyKey({
    Title = "Nova Hub",
    Note = "กรุณากรอก Key จาก Discord Server",
    Keys = {"FREE-123", "PREMIUM-456", "VIP-789", "TEST"},
    Callback = function()
        -- ถ้า Key ถูกต้อง จะมาทำงานในส่วนนี้
        loadMainUI()
    end
})

--===========================================================
-- 2. MAIN UI (ทำงานหลังจาก Key ถูกต้อง)
--===========================================================
function loadMainUI()
    local Window = Nova:CreateWindow({
        Name = "NovaHubGUI",
        Title = "Nova Hub | Blox Fruits", -- เปลี่ยนเป็นชื่อเกมของคุณ
        ConfigName = "BloxFruitsConfig",   -- ชื่อไฟล์ Config (บันทึกอัตโนมัติ)
        Theme = {
            Background = Color3.fromRGB(15, 15, 20),
            Panel      = Color3.fromRGB(25, 25, 35),
            Element    = Color3.fromRGB(35, 35, 50),
            Accent     = Color3.fromRGB(88, 101, 242),
            Text       = Color3.fromRGB(240, 240, 245),
            SubText    = Color3.fromRGB(160, 160, 175),
            Stroke     = Color3.fromRGB(50, 50, 70),
            Success    = Color3.fromRGB(46, 204, 113),
            Error      = Color3.fromRGB(231, 76, 60),
            Warning    = Color3.fromRGB(241, 196, 15),
        }
    })

    -- แจ้งเตือนต้อนรับ
    Window:Notify({
        Title = "ยินดีต้อนรับ!",
        Content = "Nova Hub โหลดสำเร็จแล้ว",
        Duration = 4
    })

    --===========================================================
    -- TAB 1: Auto Farm
    --===========================================================
    local AutoFarmTab = Window:CreateTab("Auto Farm")

    AutoFarmTab:CreateSection("ฟาร์มหลัก")

    -- Toggle พร้อม Flag (บันทึกค่าอัตโนมัติ)
    local autoFarmToggle = AutoFarmTab:CreateToggle({
        Name = "Auto Farm Level",
        CurrentValue = false,
        Flag = "AutoFarmLevel", -- ชื่อนี้จะถูกบันทึกใน Config
        Callback = function(Value)
            print("Auto Farm:", Value)
            if Value then
                Window:Notify({
                    Title = "Auto Farm",
                    Content = "เริ่มฟาร์มอัตโนมัติแล้ว!",
                    Duration = 2
                })
            end
        end
    })

    AutoFarmTab:CreateToggle({
        Name = "Auto Farm Boss",
        CurrentValue = false,
        Flag = "AutoFarmBoss",
        Callback = function(v) print("Boss Farm:", v) end
    })

    AutoFarmTab:CreateSection("ตั้งค่า")

    -- Slider พร้อม Flag
    AutoFarmTab:CreateSlider({
        Name = "Farm Distance",
        Range = {1, 50},
        Increment = 1,
        CurrentValue = 20,
        Flag = "FarmDistance",
        Callback = function(Value)
            print("ระยะฟาร์ม:", Value)
        end
    })

    AutoFarmTab:CreateSlider({
        Name = "Attack Speed",
        Range = {0.1, 2},
        Increment = 0.1,
        CurrentValue = 1,
        Flag = "AttackSpeed",
        Callback = function(v) print("ความเร็วโจมตี:", v) end
    })

    -- Dropdown พร้อม Flag
    AutoFarmTab:CreateDropdown({
        Name = "เลือกเกาะ",
        Options = {"Starter Island", "Jungle", "Pirate Village", "Desert", "Frozen Village"},
        CurrentOption = "Starter Island",
        Flag = "SelectedIsland",
        Callback = function(Option)
            print("เลือกเกาะ:", Option)
            Window:Notify({
                Title = "เปลี่ยนเกาะ",
                Content = "กำลังไปที่: " .. Option,
                Duration = 2
            })
        end
    })

    --===========================================================
    -- TAB 2: Combat
    --===========================================================
    local CombatTab = Window:CreateTab("Combat")

    CombatTab:CreateSection("การต่อสู้")

    CombatTab:CreateToggle({
        Name = "Auto Aim",
        CurrentValue = false,
        Flag = "AutoAim",
        Callback = function(v) print("Auto Aim:", v) end
    })

    CombatTab:CreateToggle({
        Name = "Kill Aura",
        CurrentValue = false,
        Flag = "KillAura",
        Callback = function(v) print("Kill Aura:", v) end
    })

    CombatTab:CreateKeybind({
        Name = "Panic Key",
        CurrentKeybind = "P",
        Flag = "PanicKey",
        Callback = function(Key)
            print("ตั้งค่าปุ่ม Panic เป็น:", Key)
        end,
        OnPressed = function()
            -- กดปุ่มนี้แล้วจะทำอะไร
            autoFarmToggle:Set(false) -- ปิด Auto Farm ทันที
            Window:Notify({
                Title = "PANIC!",
                Content = "ปิดทุกฟีเจอร์แล้ว",
                Duration = 3
            })
        end
    })

    --===========================================================
    -- TAB 3: Misc
    --===========================================================
    local MiscTab = Window:CreateTab("Misc")

    MiscTab:CreateSection("ทั่วไป")

    -- Input พร้อม Flag
    MiscTab:CreateInput({
        Name = "ชื่อผู้เล่นเป้าหมาย",
        PlaceholderText = "ใส่ชื่อ...",
        CurrentValue = "",
        Flag = "TargetPlayer",
        Callback = function(Text)
            print("เป้าหมาย:", Text)
        end
    })

    -- Button
    MiscTab:CreateButton({
        Name = "เติม HP เต็ม",
        Callback = function()
            print("เติม HP!")
            Window:Notify({
                Title = "สำเร็จ",
                Content = "HP เต็มแล้ว",
                Duration = 2
            })
        end
    })

    MiscTab:CreateButton({
        Name = "วาปกลับ Spawn",
        Callback = function()
            print("วาปกลับ Spawn!")
        end
    })

    -- Label & Paragraph
    MiscTab:CreateLabel("สถานะ: พร้อมใช้งาน")

    MiscTab:CreateParagraph("คำเตือน", "ห้ามใช้ฟีเจอร์รัวๆ อาจโดนแบนได้ ใช้ด้วยความระมัดระวัง")

    --===========================================================
    -- TAB 4: Settings
    --===========================================================
    local SettingsTab = Window:CreateTab("Settings")

    SettingsTab:CreateSection("การตั้งค่า GUI")

    SettingsTab:CreateButton({
        Name = "บันทึก Config ด้วยตนเอง",
        Callback = function()
            Window:SaveConfig()
            Window:Notify({
                Title = "Saved",
                Content = "บันทึกการตั้งค่าแล้ว",
                Duration = 2
            })
        end
    })

    SettingsTab:CreateButton({
        Name = "ปิด UI ทั้งหมด",
        Callback = function()
            Window:Destroy()
        end
    })

    SettingsTab:CreateLabel("กด RightShift เพื่อซ่อน/แสดง UI")
    SettingsTab:CreateLabel("รองรับทั้ง PC และ Mobile")
end
