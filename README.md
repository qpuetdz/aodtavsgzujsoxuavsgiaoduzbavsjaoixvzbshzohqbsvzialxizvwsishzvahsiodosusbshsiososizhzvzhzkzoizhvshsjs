-- ==========================================
-- 驗證機制（上半部）
-- ==========================================

local function req(url, method, headers, body)
    if syn and syn.request then
        local r = syn.request({Url=url, Method=method, Headers=headers, Body=body, Timeout=15})
        return {Success = r.StatusCode >= 200 and r.StatusCode < 300, Body = r.Body}
    end
    if request then
        local s, r = pcall(request, {Url=url, Method=method, Headers=headers, Body=body, Timeout=15})
        if s and r then return {Success = r.StatusCode >= 200 and r.StatusCode < 300, Body = r.Body} end
    end
    if http_request then
        local s, r = pcall(http_request, {Url=url, Method=method, Headers=headers, Body=body, Timeout=15})
        if s and r then return {Success = r.StatusCode >= 200 and r.StatusCode < 300, Body = r.Body} end
    end
    local HttpService = game:GetService("HttpService")
    local s, r = pcall(HttpService.RequestAsync, HttpService, {Url=url, Method=method, Headers=headers, Body=body, Timeout=15})
    if s and r then return {Success = r.Success, Body = r.Body} end
    return nil
end

local LP = game:GetService("Players").LocalPlayer
local HttpService = game:GetService("HttpService")

-- ★★★ 修改這一行：輸入你的卡號 ★★★
local script_key = "你的卡號"

if not script_key or script_key == "" then
    LP:Kick("請輸入有效卡號！")
    return
end

local body = HttpService:JSONEncode({
    key = script_key,
    robloxId = tostring(LP.UserId),
    robloxName = LP.Name
})

local v = req("https://kei-bot.onrender.com/v1/auth", "POST",
    {["Content-Type"]="application/json", ["User-Agent"]="Roblox/WinInet"}, body)

if not v or not v.Success then
    LP:Kick("驗證伺服器連線失敗，請稍後再試")
    return
end

local ok, data = pcall(HttpService.JSONDecode, HttpService, v.Body)
if not ok or not data.success then
    LP:Kick("卡號無效或已過期，請檢查後重新輸入")
    return
end

print("✅ 卡號驗證成功！開始載入星輝 void...")

-- ==========================================
-- 星輝 void lua 完整版 + 目標輪換 + 倒數計時器（可拖動）
-- （下半部：直接貼上全部源碼）
-- ==========================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera
local container = (gethui and gethui()) or game:GetService("CoreGui") or player:WaitForChild("PlayerGui")

local MAX_VAL = 100000

local Config = {
    Ragebot = {
        Enabled = false, AutoShoot = true, HitPart = "Head", HitChance = 100,
        Smoothness = 2, FOV = 360, VisibleOnly = true,
    },
    Voidspam = {
        Enabled = false, ShootDuration = 0.6, HideDuration = 2.5,
    },
    Underground = {
        Height = MAX_VAL, StepSize = 100000000, StepDelay = 0.01,
        HideDriftRadius = MAX_VAL, HideDriftInterval = 0,
    },
    Orbit = {
        Speed = MAX_VAL, Height = 6, Radius = 3,
        RandomFactor = 1, ShootDriftRadius = 2, ShootDriftInterval = 0,
    },
    AntiAim = {
        Enabled = false,
        Yaw = { Enabled = true, Mode = "jitter", Speed = MAX_VAL, Angle = 30 },
        Pitch = { Enabled = true, Mode = "jitter", Speed = MAX_VAL, Angle = 30 },
        Roll = { Enabled = true, Mode = "upside down" },
    },
}

local RagebotState = { LastAttackTime = 0, AttackCooldown = 0.05, LockedTarget = nil, SmoothCF = nil }
local VoidState = {
    VoidspamPhase = nil, VoidspamLastSwitch = 0, VoidspamConn = nil,
    OrbitConn = nil, OrbitAngle = 0, OrbitTarget = nil,
    AntiAimConn = nil, HideRoutineActive = false,
    LastShootDriftTime = 0, ShootDriftOffset = Vector3.zero,
    LastTarget = nil,
}

-- ========== 武器強化模組 ==========
local weaponModEnabled = false
local weaponModOriginalInput = nil
local function setupWeaponMod()
    local PlayerScripts = player:FindFirstChild("PlayerScripts")
    if not PlayerScripts then return end
    local ClientItem = PlayerScripts:FindFirstChild("Modules") and PlayerScripts.Modules:FindFirstChild("ClientReplicatedClasses") and PlayerScripts.Modules.ClientReplicatedClasses:FindFirstChild("ClientFighter") and PlayerScripts.Modules.ClientReplicatedClasses.ClientFighter:FindFirstChild("ClientItem")
    if not ClientItem then return end
    local m = require(ClientItem)
    weaponModOriginalInput = m.Input
    m.Input = function(...)
        local args = {...}
        if weaponModEnabled and args[1] and args[1].Info then
            args[1].Info.ShootRecoil = 0
            args[1].Info.ShootSpread = 0
            args[1].Info.ProjectileSpeed = 99999999
            args[1].Info.ShootCooldown = 0
            args[1].Info.QuickShotCooldown = 0
        end
        return weaponModOriginalInput(...)
    end
end

-- ========== 近戰攻速x10模組 ==========
local meleeSpeedEnabled = false
local meleeSpeedConn = nil
local originalMeleeValues = {}
local SPEED_KEYS = {
    "AttackSpeed", "SwingCooldown", "AttackCooldown",
    "FireRate", "Cooldown", "SwingSpeed", "AttackRate",
}
local function applyMeleeSpeed()
    pcall(function()
        local ItemLibrary = require(ReplicatedStorage:WaitForChild("Modules"):WaitForChild("ItemLibrary"))
        if not ItemLibrary or not ItemLibrary.Items then return end
        for _, item in pairs(ItemLibrary.Items) do
            if not item.Name then continue end
            local name = item.Name:lower()
            if name:find("melee") or name:find("katana") or name:find("sword") or 
               name:find("knife") or name:find("dagger") or name:find("fist") or
               name:find("spear") or name:find("axe") or name:find("hammer") then
                if not originalMeleeValues[item.Name] then
                    originalMeleeValues[item.Name] = {}
                    for _, key in ipairs(SPEED_KEYS) do
                        pcall(function()
                            local val = item[key]
                            if type(val) == "number" and val > 0 then
                                originalMeleeValues[item.Name][key] = val
                            end
                        end)
                    end
                end
                for _, key in ipairs(SPEED_KEYS) do
                    pcall(function()
                        local orig = originalMeleeValues[item.Name] and originalMeleeValues[item.Name][key]
                        if orig then
                            if key:lower():find("cooldown") or key:lower():find("rate") then
                                item[key] = orig / 10
                            else
                                item[key] = orig * 10
                            end
                        end
                    end)
                end
            end
        end
    end)
end
local function restoreMeleeSpeed()
    pcall(function()
        local ItemLibrary = require(ReplicatedStorage:WaitForChild("Modules"):WaitForChild("ItemLibrary"))
        if not ItemLibrary or not ItemLibrary.Items then return end
        for _, item in pairs(ItemLibrary.Items) do
            if originalMeleeValues[item.Name] then
                for key, val in pairs(originalMeleeValues[item.Name]) do
                    pcall(function() item[key] = val end)
                end
            end
        end
    end)
end
local function startMeleeSpeed()
    if meleeSpeedConn then return end
    applyMeleeSpeed()
    meleeSpeedConn = RunService.Heartbeat:Connect(function()
        if tick() % 0.3 < 0.016 then applyMeleeSpeed() end
    end)
end
local function stopMeleeSpeed()
    if meleeSpeedConn then meleeSpeedConn:Disconnect(); meleeSpeedConn = nil end
    restoreMeleeSpeed()
end

-- ========== 輔助函數 ==========
local function isEnemy(p)
    if p == player then return false end
    return not (player:GetAttribute("TeamID") and p:GetAttribute("TeamID") and player:GetAttribute("TeamID") == p:GetAttribute("TeamID"))
end
local function isTargetValid(target)
    if not target then return false end
    local char = target.Character
    if not char then return false end
    local hum = char:FindFirstChildOfClass("Humanoid")
    if not hum or hum.Health <= 0 then return false end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return false end
    return true
end
local function getClosestInFOV()
    local bestTarget, bestDist = nil, math.huge
    for _, plr in ipairs(Players:GetPlayers()) do
        if not isEnemy(plr) then continue end
        local char = plr.Character
        if not char then continue end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum or hum.Health <= 0 then continue end
        local head = char:FindFirstChild("Head")
        if not head then continue end
        local dist = (head.Position - camera.CFrame.Position).Magnitude
        if dist < bestDist then bestDist = dist; bestTarget = head end
    end
    return bestTarget
end

local function findNearestAliveEnemyExcept(excludePlayer)
    local nearest, bestDist = nil, math.huge
    local myHrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not myHrp then return nil end
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr == player or not isEnemy(plr) then continue end
        if excludePlayer and plr == excludePlayer then continue end
        local char = plr.Character
        if not char then continue end
        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum or hum.Health <= 0 then continue end
        local hrp = char:FindFirstChild("HumanoidRootPart")
        if hrp then
            local d = (hrp.Position - myHrp.Position).Magnitude
            if d < bestDist then bestDist = d; nearest = plr end
        end
    end
    return nearest
end

local function findNearestAliveEnemy()
    return findNearestAliveEnemyExcept(nil)
end

-- ========== 虛空系統 ==========
local function hideRoutine()
    if VoidState.HideRoutineActive then return end
    VoidState.HideRoutineActive = true
    local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then VoidState.HideRoutineActive = false; return end
    local targetY = Config.Underground.Height
    local step = Config.Underground.StepSize
    local delay = Config.Underground.StepDelay
    local driftRadius = Config.Underground.HideDriftRadius
    local driftInterval = Config.Underground.HideDriftInterval
    task.spawn(function()
        while VoidState.HideRoutineActive and hrp and hrp.Parent and VoidState.VoidspamPhase == "hide" do
            local currentY = hrp.Position.Y
            if currentY >= targetY - step then break end
            local newY = math.min(currentY + step, targetY)
            hrp.CFrame = CFrame.new(hrp.Position.X, newY, hrp.Position.Z)
            hrp.AssemblyLinearVelocity = Vector3.zero; hrp.AssemblyAngularVelocity = Vector3.zero
            task.wait(delay)
        end
        while VoidState.HideRoutineActive and hrp and hrp.Parent and VoidState.VoidspamPhase == "hide" do
            local angle = math.random() * 2 * math.pi
            local dist = math.random() * driftRadius
            local offset = Vector3.new(math.cos(angle) * dist, 0, math.sin(angle) * dist)
            hrp.CFrame = CFrame.new(offset.X, targetY, offset.Z)
            hrp.AssemblyLinearVelocity = Vector3.zero; hrp.AssemblyAngularVelocity = Vector3.zero
            task.wait(driftInterval)
        end
        VoidState.HideRoutineActive = false
    end)
end

local function goToTarget()
    VoidState.HideRoutineActive = false
    VoidState.ShootDriftOffset = Vector3.zero
    VoidState.LastShootDriftTime = tick()

    local nearest = nil
    if VoidState.LastTarget and isTargetValid(VoidState.LastTarget) then
        nearest = findNearestAliveEnemyExcept(VoidState.LastTarget)
        if not nearest then
            nearest = VoidState.LastTarget
        end
    else
        nearest = findNearestAliveEnemy()
    end

    if not nearest then
        VoidState.OrbitTarget = nil; RagebotState.LockedTarget = nil
        VoidState.VoidspamPhase = "hide"; VoidState.VoidspamLastSwitch = tick()
        if VoidState.OrbitConn then VoidState.OrbitConn:Disconnect(); VoidState.OrbitConn = nil end
        hideRoutine()
        return
    end

    VoidState.OrbitTarget = nearest
    local targetHrp = nearest.Character:FindFirstChild("HumanoidRootPart")
    local myHrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not targetHrp or not myHrp then return end

    local baseRadius = Config.Orbit.Radius
    local randFactor = Config.Orbit.RandomFactor
    local minDist = baseRadius * (1 - randFactor * 0.1)
    local maxDist = baseRadius * (1 + randFactor * 0.1)
    local angle = math.random() * 2 * math.pi
    local dist = minDist + math.random() * (maxDist - minDist)
    local offset = Vector3.new(math.cos(angle) * dist, Config.Orbit.Height, math.sin(angle) * dist)
    myHrp.CFrame = CFrame.new(targetHrp.Position + offset)
    myHrp.AssemblyLinearVelocity = Vector3.zero; myHrp.AssemblyAngularVelocity = Vector3.zero

    local targetHead = nearest.Character:FindFirstChild("Head")
    if targetHead then RagebotState.LockedTarget = targetHead end
end

local function startOrbit()
    if VoidState.OrbitConn then VoidState.OrbitConn:Disconnect(); VoidState.OrbitConn = nil end
    if not isTargetValid(VoidState.OrbitTarget) then
        VoidState.OrbitTarget = nil; RagebotState.LockedTarget = nil
        goToTarget(); if not VoidState.OrbitTarget then return end
    end
    VoidState.OrbitAngle = 0
    VoidState.OrbitConn = RunService.Heartbeat:Connect(function(dt)
        if not Config.Voidspam.Enabled or VoidState.VoidspamPhase ~= "shoot" then return end
        if not isTargetValid(VoidState.OrbitTarget) then
            VoidState.OrbitTarget = nil; RagebotState.LockedTarget = nil
            goToTarget(); if not VoidState.OrbitTarget then return end
        end
        local target = VoidState.OrbitTarget
        local targetHrp = target.Character:FindFirstChild("HumanoidRootPart")
        local myHrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not targetHrp or not myHrp then return end

        VoidState.OrbitAngle = (VoidState.OrbitAngle + Config.Orbit.Speed * dt) % 360
        local rad = math.rad(VoidState.OrbitAngle)
        local baseOffset = Vector3.new(math.cos(rad) * Config.Orbit.Radius, Config.Orbit.Height, math.sin(rad) * Config.Orbit.Radius)
        local now = tick()
        if now - VoidState.LastShootDriftTime >= Config.Orbit.ShootDriftInterval then
            VoidState.LastShootDriftTime = now
            local a = math.random() * 2 * math.pi
            local d = math.random() * Config.Orbit.ShootDriftRadius
            VoidState.ShootDriftOffset = Vector3.new(math.cos(a) * d, 0, math.sin(a) * d)
        end
        local targetPos = targetHrp.Position + baseOffset + VoidState.ShootDriftOffset
        local diff = targetPos - myHrp.Position
        local dist = diff.Magnitude
        if dist < 1 then myHrp.CFrame = CFrame.new(targetPos); myHrp.Velocity = Vector3.zero; return end
        myHrp.Velocity = diff.Unit * math.clamp(dist * 8, 0, 2000)
        myHrp.AssemblyAngularVelocity = Vector3.zero
    end)
end
local function stopOrbit()
    if VoidState.OrbitConn then VoidState.OrbitConn:Disconnect(); VoidState.OrbitConn = nil end
end

local function startAntiAim()
    if VoidState.AntiAimConn then return end
    VoidState.AntiAimConn = RunService.Heartbeat:Connect(function()
        if not Config.AntiAim.Enabled then return end
        local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not hrp then return end
        local pos = hrp.Position
        local yawRad = math.rad(math.random(-Config.AntiAim.Yaw.Angle, Config.AntiAim.Yaw.Angle))
        local pitchRad = math.rad(math.random(-Config.AntiAim.Pitch.Angle, Config.AntiAim.Pitch.Angle))
        hrp.CFrame = CFrame.new(pos) * CFrame.Angles(pitchRad, yawRad, math.rad(180))
    end)
end
local function stopAntiAim()
    if VoidState.AntiAimConn then VoidState.AntiAimConn:Disconnect(); VoidState.AntiAimConn = nil end
end

local function updateVoidspamPhase()
    if not Config.Voidspam.Enabled then return end
    local now = tick()
    if VoidState.VoidspamPhase == "shoot" then
        if now - VoidState.VoidspamLastSwitch >= Config.Voidspam.ShootDuration then
            VoidState.LastTarget = VoidState.OrbitTarget
            VoidState.VoidspamPhase = "hide"; VoidState.VoidspamLastSwitch = now
            stopOrbit(); hideRoutine()
        end
    elseif VoidState.VoidspamPhase == "hide" then
        if now - VoidState.VoidspamLastSwitch >= Config.Voidspam.HideDuration then
            if findNearestAliveEnemy() then
                VoidState.VoidspamPhase = "shoot"; VoidState.VoidspamLastSwitch = now
                goToTarget(); startOrbit()
            else
                VoidState.VoidspamLastSwitch = now; hideRoutine()
            end
        end
    else
        if findNearestAliveEnemy() then
            VoidState.VoidspamPhase = "shoot"; VoidState.VoidspamLastSwitch = now
            goToTarget(); startOrbit()
        else
            VoidState.VoidspamPhase = "hide"; VoidState.VoidspamLastSwitch = now; hideRoutine()
        end
    end
end
local function startVoidspam()
    if VoidState.VoidspamConn then VoidState.VoidspamConn:Disconnect() end
    VoidState.VoidspamPhase = nil; VoidState.VoidspamLastSwitch = tick()
    VoidState.LastTarget = nil
    VoidState.VoidspamConn = RunService.Heartbeat:Connect(updateVoidspamPhase)
end
local function stopVoidspam()
    if VoidState.VoidspamConn then VoidState.VoidspamConn:Disconnect(); VoidState.VoidspamConn = nil end
    VoidState.VoidspamPhase = nil; stopOrbit(); VoidState.HideRoutineActive = false
    VoidState.LastTarget = nil
end

-- ========== 自動射擊 ==========
local function autoShoot()
    if not Config.Ragebot.Enabled or not Config.Ragebot.AutoShoot then return end
    if Config.Voidspam.Enabled and VoidState.VoidspamPhase ~= "shoot" then return end
    local now = tick()
    if now - RagebotState.LastAttackTime < RagebotState.AttackCooldown then return end
    if not RagebotState.LockedTarget or not RagebotState.LockedTarget.Parent then
        RagebotState.LockedTarget = getClosestInFOV()
    end
    local targetPart = RagebotState.LockedTarget
    if not targetPart then return end
    local myRoot = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not myRoot then return end
    local eq = nil
    pcall(function()
        local fc = require(player.PlayerScripts.Controllers.FighterController)
        if fc and fc.LocalFighter then eq = fc.LocalFighter.EquippedItem end
    end)
    if not eq then return end
    local data = {
        [utf8.char(1)] = {
            [utf8.char(0)] = require(ReplicatedStorage.Modules.Utility):EncodeCFrame(CFrame.lookAt(myRoot.Position, targetPart.Position)),
            [utf8.char(1)] = require(ReplicatedStorage.Modules.Utility):EncodeCFrame(CFrame.lookAt(myRoot.Position, targetPart.Position)),
            [utf8.char(2)] = targetPart,
            [utf8.char(3)] = require(ReplicatedStorage.Modules.Utility):EncodeCFrame(CFrame.new(0.43, 0.25, 0.42)),
        },
    }
    RagebotState.LastAttackTime = now
    pcall(function()
        local el = require(ReplicatedStorage.Modules.EnumLibrary)
        ReplicatedStorage.Remotes.Replication.Fighter.UseItem:FireServer(eq:Get("ObjectID"), el:ToEnum("StartShooting"), data, nil)
    end)
end

-- ========== 瞄準更新 ==========
local function updateAimbot(dt)
    if not Config.Ragebot.Enabled then RagebotState.LockedTarget = nil; RagebotState.SmoothCF = nil; return end
    if Config.Voidspam.Enabled and VoidState.VoidspamPhase == "hide" then return end
    if not RagebotState.LockedTarget or not RagebotState.LockedTarget.Parent then
        local nt = getClosestInFOV()
        if nt then RagebotState.LockedTarget = nt; RagebotState.SmoothCF = camera.CFrame end
    end
    local targetPart = RagebotState.LockedTarget
    if not targetPart or not targetPart.Parent then return end
    local lookCF = CFrame.lookAt(camera.CFrame.Position, targetPart.Position)
    local alpha = 1 - math.exp(-6 * dt)
    RagebotState.SmoothCF = RagebotState.SmoothCF and RagebotState.SmoothCF:Lerp(lookCF, alpha) or lookCF
    pcall(function()
        local cc = require(player.PlayerScripts.Controllers.CameraController)
        if cc and cc.MimicRotation then cc:MimicRotation(RagebotState.SmoothCF)
        else camera.CFrame = RagebotState.SmoothCF end
    end)
end

-- ========== 倒數計時器 UI（可拖動） ==========
local timerGui = Instance.new("ScreenGui", container)
timerGui.Name = "CountdownTimer"
timerGui.ResetOnSpawn = false

local timerFrame = Instance.new("Frame", timerGui)
timerFrame.Size = UDim2.new(0, 160, 0, 70)
timerFrame.Position = UDim2.new(0.5, -80, 0.5, 120)
timerFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
timerFrame.BackgroundTransparency = 0.5
timerFrame.BorderSizePixel = 0
timerFrame.Active = true
timerFrame.Draggable = true
Instance.new("UICorner", timerFrame).CornerRadius = UDim.new(0, 10)

local phaseLabel = Instance.new("TextLabel", timerFrame)
phaseLabel.Size = UDim2.new(1, 0, 0, 28)
phaseLabel.Position = UDim2.new(0, 0, 0, 6)
phaseLabel.BackgroundTransparency = 1
phaseLabel.Text = "..."
phaseLabel.TextColor3 = Color3.new(1, 1, 1)
phaseLabel.Font = Enum.Font.GothamBold
phaseLabel.TextSize = 18

local timeLabel = Instance.new("TextLabel", timerFrame)
timeLabel.Size = UDim2.new(1, 0, 0, 30)
timeLabel.Position = UDim2.new(0, 0, 0, 36)
timeLabel.BackgroundTransparency = 1
timeLabel.Text = "0.0"
timeLabel.TextColor3 = Color3.new(0.8, 0.8, 1)
timeLabel.Font = Enum.Font.Gotham
timeLabel.TextSize = 24

local timerConn = nil

local function updateTimerDisplay()
    if not Config.Voidspam.Enabled or not VoidState.VoidspamPhase then
        timerFrame.Visible = false
        return
    end
    timerFrame.Visible = true

    local phase = VoidState.VoidspamPhase
    local duration = phase == "shoot" and Config.Voidspam.ShootDuration or Config.Voidspam.HideDuration
    local elapsed = tick() - VoidState.VoidspamLastSwitch
    local remaining = math.max(0, duration - elapsed)

    if phase == "shoot" then
        phaseLabel.Text = "ATTACKING"
        phaseLabel.TextColor3 = Color3.new(1, 0.3, 0.3)
    else
        phaseLabel.Text = "VOIDING"
        phaseLabel.TextColor3 = Color3.new(0.4, 0.8, 1)
    end

    timeLabel.Text = string.format("%.1f", remaining)
end

-- ========== 主循環 ==========
local MainConnection
local function startAll()
    if MainConnection then return end
    MainConnection = RunService.RenderStepped:Connect(function(dt)
        updateAimbot(dt)
        autoShoot()
    end)
    if timerConn then timerConn:Disconnect() end
    timerConn = RunService.RenderStepped:Connect(updateTimerDisplay)
    if Config.Voidspam.Enabled then startVoidspam() end
    if Config.AntiAim.Enabled then startAntiAim() end
end
local function stopAll()
    if MainConnection then MainConnection:Disconnect(); MainConnection = nil end
    if timerConn then timerConn:Disconnect(); timerConn = nil end
    stopVoidspam(); stopAntiAim()
    RagebotState.LockedTarget = nil; RagebotState.SmoothCF = nil
    timerFrame.Visible = false
end

setupWeaponMod()

-- ========== 控制面板 ==========
local gui = Instance.new("ScreenGui")
gui.Name = "XingHui_SkyVoid"
gui.Parent = container

local panel = Instance.new("Frame", gui)
panel.Size = UDim2.new(0, 300, 0, 330)
panel.Position = UDim2.new(0.5, -150, 0.5, -165)
panel.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
panel.BackgroundTransparency = 0.3; panel.BorderSizePixel = 0
panel.Active = true; panel.Draggable = true
Instance.new("UICorner", panel).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", panel).Thickness = 2
panel.UIStroke.Color = Color3.new(1, 1, 1)

local titleBar = Instance.new("Frame", panel)
titleBar.Size = UDim2.new(1, 0, 0, 26)
titleBar.BackgroundColor3 = Color3.fromRGB(12, 12, 15)
titleBar.BackgroundTransparency = 0.3
local titleLabel = Instance.new("TextLabel", titleBar)
titleLabel.Size = UDim2.new(1, -26, 1, 0); titleLabel.Position = UDim2.new(0, 8, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "星輝 void lua"
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.Font = Enum.Font.GothamBold; titleLabel.TextSize = 16

local closeBtn = Instance.new("TextButton", panel)
closeBtn.Size = UDim2.new(0, 20, 0, 20); closeBtn.Position = UDim2.new(1, -22, 0, 3)
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50); closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.new(1, 1, 1); closeBtn.Font = Enum.Font.GothamBold; closeBtn.TextSize = 12
closeBtn.MouseButton1Click:Connect(function()
    stopAll(); stopMeleeSpeed()
    gui:Destroy()
end)

local scroll = Instance.new("ScrollingFrame", panel)
scroll.Size = UDim2.new(1, -10, 0, 298); scroll.Position = UDim2.new(0, 5, 0, 28)
scroll.BackgroundColor3 = Color3.fromRGB(25, 25, 30); scroll.BackgroundTransparency = 0.6
scroll.ScrollBarThickness = 2; scroll.CanvasSize = UDim2.new(0, 0, 0, 290)
Instance.new("UICorner", scroll).CornerRadius = UDim.new(0, 6)

local list = Instance.new("Frame", scroll)
list.Size = UDim2.new(1, -6, 0, 290); list.Position = UDim2.new(0, 3, 0, 2)
list.BackgroundTransparency = 1
local lay = Instance.new("UIListLayout", list)
lay.Padding = UDim.new(0, 2)

local function addToggle(parent, text, def, cb)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(1, -6, 0, 22)
    btn.BackgroundColor3 = def and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(45, 45, 50)
    btn.Text = (def and "✓ " or "✗ ") .. text
    btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = Enum.Font.GothamBold; btn.TextSize = 12
    btn.MouseButton1Click:Connect(function()
        def = not def
        btn.Text = (def and "✓ " or "✗ ") .. text
        btn.BackgroundColor3 = def and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(45, 45, 50)
        cb(def)
    end)
    return btn
end

local function addSlider(parent, name, min, max, def, step, cb)
    local f = Instance.new("Frame", parent)
    f.Size = UDim2.new(1, -6, 0, 26); f.BackgroundTransparency = 1
    local lb = Instance.new("TextLabel", f)
    lb.Size = UDim2.new(0, 90, 0, 14); lb.BackgroundTransparency = 1
    lb.Text = name; lb.TextColor3 = Color3.new(0.8, 0.8, 0.8); lb.Font = Enum.Font.Gotham; lb.TextSize = 12
    local vl = Instance.new("TextLabel", f)
    vl.Size = UDim2.new(0, 50, 0, 14); vl.Position = UDim2.new(1, -50, 0, 0)
    vl.BackgroundTransparency = 1; vl.Text = string.format("%.1f", def)
    vl.TextColor3 = Color3.new(1, 1, 1); vl.Font = Enum.Font.Gotham; vl.TextSize = 12
    local bar = Instance.new("Frame", f)
    bar.Size = UDim2.new(1, -10, 0, 4); bar.Position = UDim2.new(0, 5, 0, 17)
    bar.BackgroundColor3 = Color3.fromRGB(50, 50, 55)
    Instance.new("UICorner", bar).CornerRadius = UDim.new(0, 2)
    local fill = Instance.new("Frame", bar)
    fill.Size = UDim2.new((def - min) / (max - min), 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
    Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 2)
    local function upd(pos)
        local x = math.clamp((pos.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
        local val = min + x * (max - min)
        val = math.floor(val / step + 0.5) * step
        val = math.max(min, math.min(max, val))
        fill.Size = UDim2.new(x, 0, 1, 0)
        vl.Text = string.format("%.1f", val)
        cb(val)
    end
    bar.InputBegan:Connect(function(inp)
        if inp.UserInputType == Enum.UserInputType.MouseButton1 or inp.UserInputType == Enum.UserInputType.Touch then
            upd(inp.Position)
            local c = UserInputService.InputChanged:Connect(function(ch)
                if ch.UserInputType == Enum.UserInputType.MouseMovement or ch.UserInputType == Enum.UserInputType.Touch then upd(ch.Position) end
            end)
            local e = UserInputService.InputEnded:Connect(function() c:Disconnect() e:Disconnect() end)
        end
    end)
    return f
end

-- 開關
addToggle(list, "虛空 Spam", Config.Voidspam.Enabled, function(v)
    Config.Voidspam.Enabled = v; Config.Ragebot.Enabled = v
    if v then startAll() else stopAll() end
end)
addToggle(list, "反自瞄", Config.AntiAim.Enabled, function(v)
    Config.AntiAim.Enabled = v
    if v then startAntiAim() else stopAntiAim() end
end)
addToggle(list, "武器強化", false, function(v) weaponModEnabled = v end)
addToggle(list, "近戰攻速x10", false, function(v)
    meleeSpeedEnabled = v
    if v then startMeleeSpeed() else stopMeleeSpeed() end
end)

-- 滑塊
addSlider(list, "環繞半徑", 1, 10, Config.Orbit.Radius, 0.5, function(v) Config.Orbit.Radius = v end)
addSlider(list, "環繞高度", 0, 10, Config.Orbit.Height, 0.5, function(v) Config.Orbit.Height = v end)
addSlider(list, "shoot偏移半徑", 0, 10, Config.Orbit.ShootDriftRadius, 0.5, function(v) Config.Orbit.ShootDriftRadius = v end)
addSlider(list, "現身時間", 0, 10, Config.Voidspam.ShootDuration, 0.1, function(v) Config.Voidspam.ShootDuration = v end)
addSlider(list, "躲藏時間", 0, 10, Config.Voidspam.HideDuration, 0.1, function(v) Config.Voidspam.HideDuration = v end)

print("✅ 星輝 void lua 目標輪換版已啟動｜每次出現鎖定不同敵人")
