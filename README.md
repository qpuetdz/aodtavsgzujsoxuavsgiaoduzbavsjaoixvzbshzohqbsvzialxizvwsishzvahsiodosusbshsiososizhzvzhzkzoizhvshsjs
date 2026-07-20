-- ==========================================
-- 自包含驗證腳本（無需外部下載功能腳本）
-- ==========================================

-- 使用者只需修改這一行卡號
local script_key = "你的卡號"

-- 將你的真實功能腳本完整貼在下面這對 [[ ]] 之間
local realScript = [[
 local repo = "https://raw.githubusercontent.com/deividcomsono/Obsidian/main/"
local Library = loadstring(game:HttpGet(repo .. "Library.lua"))()
local ThemeManager = loadstring(game:HttpGet(repo .. "addons/ThemeManager.lua"))()
local SaveManager = loadstring(game:HttpGet(repo .. "addons/SaveManager.lua"))()

local Options = Library.Options
local Toggles = Library.Toggles
local Players = game:GetService("Players")
local LP = Players.LocalPlayer

local StatesModule
local function getStatesModule()
    if StatesModule then return StatesModule end
    local statesScript = game:GetService("ReplicatedStorage"):FindFirstChild("States", true)
    if statesScript then
        local success, err = pcall(require, statesScript)
        if success then
            StatesModule = err
            return StatesModule
        end
    end
    return nil
end

local NeuronModule
local function getNeuronModule()
    if NeuronModule then return NeuronModule end
    local neuronScript = game:GetService("ReplicatedFirst"):FindFirstChild("neuron", true)
    if neuronScript then
        local success, err = pcall(require, neuronScript)
        if success then
            NeuronModule = err
            return NeuronModule
        end
    end
    return nil
end

Library.ForceCheckbox = false
Library.ShowToggleFrameInKeybinds = true

do
local Window = Library:CreateWindow({
	Title = " MSS_Hub",
	Footer = "[\240\159\145\145NEW]OVERKILL",
	NotifySide = "Right",
	ShowCustomCursor = true,
})

local Tabs = {
	Aimbot = Window:AddTab("Aimbot", "crosshair"),
	ESP = Window:AddTab("ESP", "eye"),
	World = Window:AddTab("World", "sun"),
	Visuals = Window:AddTab("Visuals", "image"),
	Fun = Window:AddTab("Fun", "star"),
	["UI Settings"] = Window:AddTab("UI Settings", "settings"),
}

-- ===== AIMBOT TAB =====
local AimbotGroup = Tabs.Aimbot:AddLeftGroupbox("Aim Assist")
AimbotGroup:AddToggle("AimbotEnabled", { Text = "Enabled", Default = false })
AimbotGroup:AddSlider("AimbotFOV", { Text = "FOV", Default = 10, Min = 1, Max = 180, Suffix = "\194\176", Rounding = 1 })
AimbotGroup:AddSlider("AimbotSmooth", { Text = "Smoothing", Default = 10, Min = 1, Max = 50, Rounding = 0 })
AimbotGroup:AddDropdown("AimbotPriority", { Values = { "Head", "Body", "Nearest" }, Default = 1, Text = "Priority" })
AimbotGroup:AddToggle("SilentAim", { Text = "Silent Aim", Default = false })
AimbotGroup:AddDropdown("SilentAimHitbox", { Values = { "Head", "Body" }, Default = 1, Text = "Silent Hitbox" })
AimbotGroup:AddLabel("Keybind"):AddKeyPicker("AimbotKey", { Default = "MB2", Mode = "Hold", Text = "Aim Key", NoUI = false })

local TriggerGroup = Tabs.Aimbot:AddRightGroupbox("Triggerbot")
TriggerGroup:AddToggle("TriggerbotEnabled", { Text = "Enabled", Default = false })
TriggerGroup:AddSlider("TriggerbotDelay", { Text = "Delay", Default = 0, Min = 0, Max = 500, Suffix = "ms", Rounding = 0 })
TriggerGroup:AddDropdown("TriggerbotHitbox", { Values = { "Head", "Body" }, Default = 1, Text = "Hitbox" })
TriggerGroup:AddLabel("Keybind"):AddKeyPicker("TriggerbotKey", { Default = "Q", Mode = "Hold", Text = "Trigger Key", NoUI = false })

-- ===== ESP TAB =====
local ESPGroup = Tabs.ESP:AddLeftGroupbox("Player ESP")
ESPGroup:AddToggle("ESPEnabled", { Text = "Enabled", Default = true })
local espBoxToggle = ESPGroup:AddToggle("ESPBox", { Text = "Box", Default = true })
espBoxToggle:AddColorPicker("ESPBoxColor", { Default = Color3.fromRGB(255, 50, 50), Title = "Box Color" })
local espNameToggle = ESPGroup:AddToggle("ESPName", { Text = "Name", Default = true })
espNameToggle:AddColorPicker("ESPNameColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Name Color" })
ESPGroup:AddToggle("ESPHealth", { Text = "Health Bar", Default = true })
ESPGroup:AddToggle("ESPShowWeapon", { Text = "Show Weapon", Default = false })
ESPGroup:AddToggle("ESPDistance", { Text = "Distance", Default = true })
-- local espHeadToggle = ESPGroup:AddToggle("ESPHeadDot", { Text = "Head Dot", Default = true })
-- espHeadToggle:AddColorPicker("ESPHeadDotColor", { Default = Color3.fromRGB(255, 255, 50), Title = "Head Dot Color" })
local espTracerToggle = ESPGroup:AddToggle("ESPTracer", { Text = "Tracer", Default = false })
espTracerToggle:AddColorPicker("ESPTracerColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Tracer Color" })
local espSkelToggle = ESPGroup:AddToggle("ESPSkeleton", { Text = "Skeleton", Default = false })
espSkelToggle:AddColorPicker("ESPSkeletonColor", { Default = Color3.fromRGB(255, 255, 255), Title = "Skeleton Color" })
local espOffscreenToggle = ESPGroup:AddToggle("ESPOffscreen", { Text = "Offscreen Indicators", Default = false })
espOffscreenToggle:AddColorPicker("ESPOffscreenColor", { Default = Color3.fromRGB(255, 50, 50), Title = "Indicator Color" })
ESPGroup:AddDivider()
ESPGroup:AddToggle("ESPTeamCheck", { Text = "Team Check", Default = false })
local chamsToggle = ESPGroup:AddToggle("Chams", { Text = "Chams", Default = false })
chamsToggle:AddColorPicker("ChamsColor", { Default = Color3.fromRGB(0, 200, 255), Title = "Chams Color" })
ESPGroup:AddDropdown("ChamsMode", { Values = { "Always", "Wall Only" }, Default = 1, Text = "Chams Mode" })

local FOVGroup = Tabs.ESP:AddRightGroupbox("FOV Circle")
local fovToggle = FOVGroup:AddToggle("FOVCircle", { Text = "Show FOV", Default = true })
fovToggle:AddColorPicker("FOVCircleColor", { Default = Color3.fromRGB(127, 72, 163), Title = "FOV Color" })
FOVGroup:AddSlider("FOVCircleSize", { Text = "FOV Size", Default = 10, Min = 1, Max = 180, Suffix = "\194\176", Rounding = 1 })

-- ===== WORLD TAB =====
local LightingGroup = Tabs.World:AddLeftGroupbox("Lighting")
LightingGroup:AddToggle("NightMode", { Text = "Night Mode", Default = false })
LightingGroup:AddToggle("CustomTime", { Text = "Custom Time", Default = false })
LightingGroup:AddSlider("TimeOfDay", { Text = "Time Of Day", Default = 12, Min = 0, Max = 24, Rounding = 1, Suffix = ":00", HideMax = true })
LightingGroup:AddToggle("CustomFog", { Text = "Custom Fog", Default = false })
LightingGroup:AddSlider("FogEnd", { Text = "Fog End", Default = 99999, Min = 1, Max = 99999, Rounding = 0 })
LightingGroup:AddSlider("FogStart", { Text = "Fog Start", Default = 0, Min = 0, Max = 500, Rounding = 0 })
local ambientToggle = LightingGroup:AddToggle("CustomAmbient", { Text = "Custom Ambient", Default = false })
ambientToggle:AddColorPicker("AmbientColor", { Default = Color3.fromRGB(127, 127, 127), Title = "Ambient Color" })
LightingGroup:AddSlider("Brightness", { Text = "Brightness", Default = 2, Min = 0, Max = 10, Rounding = 1 })

local EffectsGroup = Tabs.World:AddRightGroupbox("Effects")
EffectsGroup:AddToggle("Fullbright", { Text = "Fullbright", Default = false })
EffectsGroup:AddToggle("RemoveFog", { Text = "Remove Fog", Default = false })
EffectsGroup:AddToggle("XRay", { Text = "X-Ray", Default = false })
EffectsGroup:AddToggle("NoSky", { Text = "No Sky", Default = false })
EffectsGroup:AddDivider()
EffectsGroup:AddToggle("RainEffect", { Text = "\240\159\140\167 Rain", Default = false })
EffectsGroup:AddSlider("RainIntensity", { Text = "Rain Intensity", Default = 200, Min = 50, Max = 800, Rounding = 0 })
EffectsGroup:AddToggle("SnowEffect", { Text = "\226\157\132 Snow", Default = false })
EffectsGroup:AddSlider("SnowIntensity", { Text = "Snow Intensity", Default = 150, Min = 30, Max = 500, Rounding = 0 })

local SkyboxGroup = Tabs.World:AddRightGroupbox("Skybox Changer")
SkyboxGroup:AddToggle("SkyboxEnabled", { Text = "Custom Skybox", Default = false })
SkyboxGroup:AddDropdown("SkyboxPreset", { Values = { "Purple Nebula", "Warm Sunset", "Space", "Blue Clouds", "Aesthetic Sakura", "Starry Night", "Red Aurora", "Custom" }, Default = 1, Text = "Skybox Preset" })
SkyboxGroup:AddInput("SkyboxCustomID", { Text = "Custom ID", Default = "", Placeholder = "rbxassetid://..." })

-- ===== VISUALS TAB =====


local TracerGroup = Tabs.Visuals:AddLeftGroupbox("Bullet Tracers")
local TracersEnabled = TracerGroup:AddToggle("TracersEnabled", { Text = "Enabled", Default = false })
TracersEnabled:AddColorPicker("TracersColor", { Default = Color3.fromRGB(0, 200, 255), Title = "Tracer Color" })
TracerGroup:AddDropdown("TracersType", { Values = { "Beam", "Part", "Wireframe" }, Default = 1, Text = "Tracer Type" })
TracerGroup:AddSlider("TracersThickness", { Text = "Thickness", Default = 0.05, Min = 0.01, Max = 2, Rounding = 2 })
TracerGroup:AddSlider("TracersDuration", { Text = "Duration (s)", Default = 0.5, Min = 0.1, Max = 3.0, Rounding = 1 })
TracerGroup:AddSlider("TracersTransparency", { Text = "Start Transparency", Default = 0, Min = 0, Max = 1.0, Rounding = 2 })

local SoundGroup = Tabs.Visuals:AddRightGroupbox("Sound")
SoundGroup:AddToggle("CustomShootSound", { Text = "Custom Shoot Sound", Default = false })
SoundGroup:AddToggle("CustomHitSound", { Text = "Custom Hit Sound", Default = false })
SoundGroup:AddInput("ShootSoundID", { Text = "Shoot Sound ID", Default = "", Placeholder = "rbxassetid://..." })
SoundGroup:AddInput("HitSoundID", { Text = "Hit Sound ID", Default = "", Placeholder = "rbxassetid://..." })
SoundGroup:AddSlider("HitSoundVolume", { Text = "Hit Volume", Default = 1, Min = 0, Max = 5, Rounding = 1, Suffix = "x" })

local HitGroup = Tabs.Visuals:AddLeftGroupbox("Hit Overlay")
local hitOverlayToggle = HitGroup:AddToggle("HitOverlay", { Text = "Hit Overlay", Default = false })
hitOverlayToggle:AddColorPicker("HitOverlayColor", { Default = Color3.fromRGB(255, 0, 0), Title = "Overlay Color" })
HitGroup:AddSlider("HitOverlayAlpha", { Text = "Alpha", Default = 0.3, Min = 0, Max = 1, Rounding = 2 })
HitGroup:AddSlider("HitOverlayDuration", { Text = "Duration", Default = 0.3, Min = 0.1, Max = 2, Rounding = 1, Suffix = "s" })
local hitNotifToggle = HitGroup:AddToggle("HitNotification", { Text = "Hit Notification", Default = false })
hitNotifToggle:AddColorPicker("HitNotifColor", { Default = Color3.fromRGB(255, 200, 50), Title = "Notif Color" })

local hitVisualToggle = HitGroup:AddToggle("HitVisualEffect", { Text = "Hit Visual Effect", Default = false })
hitVisualToggle:AddColorPicker("HitVisualColor", { Default = Color3.fromRGB(0, 255, 120), Title = "Effect Color" })
local dmgIndicatorToggle = HitGroup:AddToggle("DamageIndicators", { Text = "Damage Indicators", Default = true })
dmgIndicatorToggle:AddColorPicker("DamageIndicatorsColor", { Default = Color3.fromRGB(255, 100, 0), Title = "Damage Color" })
HitGroup:AddDropdown("DamageIndicatorsStyle", { Values = { "Default", "Minecraft", "Cartoon", "Comic", "Arcade" }, Default = 3, Text = "Damage Font" })
local VMChamsGroup = Tabs.Visuals:AddRightGroupbox("Viewmodel / Gun Chams")
local vmChamsToggle = VMChamsGroup:AddToggle("VMChams", { Text = "VM Weapon Chams", Default = false })
vmChamsToggle:AddColorPicker("VMChamsColor", { Default = Color3.fromRGB(0, 200, 255), Title = "Weapon Color" })
VMChamsGroup:AddDropdown("VMChamsStyle", { Values = { "ForceField", "Neon", "Plastic", "Glass", "Wireframe" }, Default = 1, Text = "Weapon Style" })
VMChamsGroup:AddSlider("VMChamsTransparency", { Text = "Weapon Transparency", Default = 0.5, Min = 0, Max = 1, Rounding = 2 })

local vmArmChamsToggle = VMChamsGroup:AddToggle("VMArmChams", { Text = "VM Arm Chams", Default = false })
vmArmChamsToggle:AddColorPicker("VMArmChamsColor", { Default = Color3.fromRGB(0, 200, 255), Title = "Arm Color" })
VMChamsGroup:AddDropdown("VMArmChamsStyle", { Values = { "Plastic", "Neon", "ForceField", "Glass", "Wireframe" }, Default = 1, Text = "Arm Style" })
VMChamsGroup:AddSlider("VMArmChamsTransparency", { Text = "Arm Transparency", Default = 0, Min = 0, Max = 1, Rounding = 2 })
VMChamsGroup:AddToggle("VMArmHideADS", { Text = "Hide Arms when Aiming", Default = false })

local NameGroup = Tabs.Fun:AddLeftGroupbox("Name Changer")
NameGroup:AddToggle("NameChanger", { Text = "Enabled", Default = false })
NameGroup:AddButton("Open Name Editor", function()
	if nameChangerGui then
		nameChangerGui.Enabled = not nameChangerGui.Enabled
		if nameChangerGui.Enabled then nameChangerGui.Frame.Visible = true end
	end
end)

local nameStatusLabel = NameGroup:AddLabel("Display: " .. LP.DisplayName)
local usernameStatusLabel = NameGroup:AddLabel("Username: " .. LP.Name)
local gunChamsToggle = VMChamsGroup:AddToggle("GunChams", { Text = "Gun Chams (World)", Default = false })
gunChamsToggle:AddColorPicker("GunChamsColor", { Default = Color3.fromRGB(0, 200, 255), Title = "Gun Color" })
VMChamsGroup:AddDropdown("GunChamsStyle", { Values = { "Highlight", "ForceField", "Neon", "Plastic", "Glass", "Wireframe" }, Default = 1, Text = "Gun Style" })
VMChamsGroup:AddSlider("GunChamsTransparency", { Text = "Gun Transparency", Default = 0.5, Min = 0, Max = 1, Rounding = 2 })
VMChamsGroup:AddDropdown("GunChamsMode", { Values = { "Always", "Wall Only" }, Default = 1, Text = "Gun Depth Mode" })

local UnlockGroup = Tabs.Visuals:AddLeftGroupbox("Unlock All")
UnlockGroup:AddToggle("UnlockAll", { Text = "Unlock All Items", Default = false })
UnlockGroup:AddButton("Refresh Unlocks", function() setupUnlockAll() end)
-- ===== THIRD PERSON =====
local ThirdPersonGroup = Tabs.Visuals:AddRightGroupbox("Third Person")
ThirdPersonGroup:AddToggle("ThirdPersonEnabled", {
	Text = "Enabled",
	Default = false,
	Callback = function(v)
		if v then
			setupThirdPersonHook()
		else
			if CameraHandler and CameraHandler.thirdPersonDebug then
				pcall(function() CameraHandler:exitThirdPersonDebug() end)
			end
			
			-- Restore first-person character transparency when disabling third person
			local nMod = getNeuronModule()
			local char = LP.Character or (nMod and nMod:get_character(LP))
			if char then
				local GameplayHandler = game:GetService("ReplicatedStorage"):FindFirstChild("GameplayHandler", true)
				if GameplayHandler then
					pcall(function()
						require(GameplayHandler):SetCharacterTransparency(char, 1)
					end)
				else
					for _, part in ipairs(char:GetDescendants()) do
						if part:IsA("BasePart") or part:IsA("Decal") then
							if part.Name ~= "HumanoidRootPart" and part.Name ~= "HeadCollider" and part.Name ~= "BoundingBody" and not part.Name:find("Hitbox") then
								part.Transparency = 1
							end
						end
					end
				end
			end
		end
	end
})
ThirdPersonGroup:AddSlider("ThirdPersonDistance", { Text = "Distance", Default = 10, Min = 2, Max = 30, Suffix = " studs", Rounding = 1 })
ThirdPersonGroup:AddSlider("ThirdPersonOffsetX", { Text = "Offset X (\229\183\166\229\143\179)", Default = 0, Min = -10, Max = 10, Suffix = " studs", Rounding = 1 })
-- ===== FUN TAB =====
local minesState = { status = "idle", grid = {}, revealed = 0, mineCount = 3, streak = 0, wins = 0, losses = 0, buttons = {} }
local MinesGroup = Tabs.Fun:AddLeftGroupbox("Gambling Mines")
MinesGroup:AddToggle("MinesEnabled", { Text = "Show Mines", Default = false })
MinesGroup:AddSlider("MinesCount", { Text = "Mines", Default = 3, Min = 1, Max = 24, Rounding = 0 })
MinesGroup:AddButton("New Game", function() startMinesGame() end)
local winsLabel = MinesGroup:AddLabel("Wins: 0")
local lossesLabel = MinesGroup:AddLabel("Losses: 0")

local MinesGameInfo = Tabs.Fun:AddRightGroupbox("Game Info")
local streakLabel = MinesGameInfo:AddLabel("Streak: 0")
local revealedLabel = MinesGameInfo:AddLabel("Revealed: 0 / " .. (25 - minesState.mineCount))
MinesGameInfo:AddButton("Cash Out", function() cashOutMines() end)



local ScreenTiltGroup = Tabs.Fun:AddRightGroupbox("Screen Tilt")
ScreenTiltGroup:AddToggle("ScreenTiltEnabled", { Text = "Enabled", Default = false })
ScreenTiltGroup:AddSlider("ScreenTiltX", { Text = "Tilt X (\228\191\175\228\187\176)", Default = 0, Min = -180, Max = 180, Rounding = 1, Suffix = "\194\176" })
ScreenTiltGroup:AddSlider("ScreenTiltY", { Text = "Tilt Y (\229\183\166\229\143\179)", Default = 0, Min = -180, Max = 180, Rounding = 1, Suffix = "\194\176" })
ScreenTiltGroup:AddSlider("ScreenTiltZ", { Text = "Tilt Z (\230\187\190\229\139\149)", Default = 0, Min = -180, Max = 180, Rounding = 1, Suffix = "\194\176" })
ScreenTiltGroup:AddSlider("ScreenTiltScaleX", { Text = "\231\184\174\230\148\190 X (\229\163\147\230\137\129/\230\139\137\229\175\172)", Default = 1, Min = 0.1, Max = 5, Rounding = 2 })
ScreenTiltGroup:AddSlider("ScreenTiltScaleY", { Text = "\231\184\174\230\148\190 Y (\229\163\147\230\137\129/\230\139\137\233\171\152)", Default = 1, Min = 0.1, Max = 5, Rounding = 2 })
ScreenTiltGroup:AddButton("\233\135\141\231\189\174\232\167\146\229\186\166", function()
	if Options.ScreenTiltX then Options.ScreenTiltX:SetValue(0) end
	if Options.ScreenTiltY then Options.ScreenTiltY:SetValue(0) end
	if Options.ScreenTiltZ then Options.ScreenTiltZ:SetValue(0) end
	if Options.ScreenTiltScaleX then Options.ScreenTiltScaleX:SetValue(1) end
	if Options.ScreenTiltScaleY then Options.ScreenTiltScaleY:SetValue(1) end
end)

local SoundEffectGroup = Tabs.Fun:AddLeftGroupbox("Custom Hit SFX")
SoundEffectGroup:AddToggle("CustomHitSFX", { Text = "Enabled", Default = false })

local FunCrosshairGroup = Tabs.Fun:AddLeftGroupbox("Fun Ammo Crosshair")
local funCrossToggle = FunCrosshairGroup:AddToggle("FunCrosshairEnabled", { Text = "Enabled", Default = false })
funCrossToggle:AddColorPicker("FunCrosshairColor", { Default = Color3.fromRGB(255, 0, 120), Title = "Crosshair Color" })
FunCrosshairGroup:AddSlider("FunCrosshairSpeed", { Text = "Rotation Speed", Default = 60, Min = 10, Max = 300, Suffix = "\194\176/s", Rounding = 0 })
FunCrosshairGroup:AddSlider("FunCrosshairSize", { Text = "Crosshair Size", Default = 20, Min = 5, Max = 100, Rounding = 0 })
FunCrosshairGroup:AddSlider("FunCrosshairGap", { Text = "Crosshair Gap", Default = 5, Min = 0, Max = 30, Rounding = 0 })
FunCrosshairGroup:AddSlider("FunCrosshairThickness", { Text = "Crosshair Thickness", Default = 1, Min = 1, Max = 5, Rounding = 0 })
FunCrosshairGroup:AddToggle("FunCrosshairPulse", { Text = "Breathing Animation", Default = true })
FunCrosshairGroup:AddSlider("FunCrosshairPulseSpeed", { Text = "Breathing Speed", Default = 5, Min = 1, Max = 15, Rounding = 1 })
FunCrosshairGroup:AddSlider("FunCrosshairPulseSize", { Text = "Breathing Amplitude", Default = 4, Min = 1, Max = 15, Rounding = 0 })

local FunEffectsGroup = Tabs.Fun:AddLeftGroupbox("Wing & Hat Effects")
FunEffectsGroup:AddToggle("FunCharEffects", { Text = "Enabled", Default = false })

local function setupXingHuiCrosshair()


if _G.XingHuiCrosshair then
    pcall(function()
        _G.XingHuiCrosshair.Settings.Enabled = false
        if _G.XingHuiCrosshair.Heartbeat then _G.XingHuiCrosshair.Heartbeat:Disconnect() end
        if _G.XingHuiCrosshair.Gui then _G.XingHuiCrosshair.Gui:Destroy() end
        if _G.XingHuiCrosshair.Menu then _G.XingHuiCrosshair.Menu:Destroy() end
        if _G.XingHuiCrosshair.LockBtnFrame then _G.XingHuiCrosshair.LockBtnFrame:Destroy() end
        if _G.XingHuiCrosshair.HubBtnFrame then _G.XingHuiCrosshair.HubBtnFrame:Destroy() end
    end)
    _G.XingHuiCrosshair = nil
end

_G.XingHuiCrosshair = {}


local Players = game:GetService("Players")
local player = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local function isWindowActive()
	return UserInputService.MouseBehavior == Enum.MouseBehavior.LockCenter and not UserInputService:GetFocusedTextBox()
end
local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")
local HttpService = game:GetService("HttpService")

task.spawn(function()
    pcall(function()
        if setfflag then
            setfflag("AbuseReportScreenshot", "False")
            setfflag("AbuseReportScreenshotPercentage", "0")
        end
        if syn and syn.protect_gui then syn.protect_gui(CoreGui) end
        if hookfunction then
            hookfunction(print, function() end)
            hookfunction(warn, function() end)
        end
        if hookmetamethod then
            local oldNamecall
            oldNamecall = hookmetamethod(game, "__namecall", function(self, ...)
                local method = getnamecallmethod()
                if method == "FireServer" or method == "InvokeServer" then
                    local lowerName = self.Name:lower()
                    if lowerName:find("report") or lowerName:find("ban") or lowerName:find("kick") then return nil end
                end
                return oldNamecall(self, ...)
            end)
        end
        if hookfunction then
            hookfunction(player.Kick, function(...) return true end)
        end
    end)
end)

local function GetDefaultCrossSettings()
    return {
        Enabled = false,
        Size = 24,
        Thickness = 3,
        Gap = 6,
        BigFillVis = false,
        BigFillDia = 50,
        BigFillDir = "\229\176\141\232\167\146\230\188\184\229\177\164",
        BigFillH1 = 0,
        BigFillS1 = 100,
        BigFillV1 = 100,
        BigFillH2 = 240,
        BigFillS2 = 100,
        BigFillV2 = 100,
        BigFillTrans = 10,
        BigFillRain = false,
        BigFillRainSpd = 1,
        BigStrVis = false,
        BigStrThick = 1,
        BigStrH1 = 180,
        BigStrS1 = 100,
        BigStrV1 = 100,
        BigStrTrans = 10,
        BigStrRain = false,
        BigStrRainSpd = 1.5,
        SmlFillVis = false,
        SmlFillDia = 15,
        SmlFillDir = "\229\176\141\232\167\146\230\188\184\229\177\164",
        SmlFillH1 = 240,
        SmlFillS1 = 100,
        SmlFillV1 = 100,
        SmlFillH2 = 120,
        SmlFillS2 = 100,
        SmlFillV2 = 100,
        SmlFillTrans = 20,
        SmlFillRain = false,
        SmlFillRainSpd = 1.5,
        SmlStrVis = false,
        SmlStrThick = 0.5,
        SmlStrH1 = 60,
        SmlStrS1 = 100,
        SmlStrV1 = 100,
        SmlStrTrans = 10,
        SmlStrRain = false,
        SmlStrRainSpd = 2,
        CrossFillVis = false,
        CrossFillDir = "\231\183\154\230\128\1670\194\176",
        CrossFillH1 = 0,
        CrossFillS1 = 100,
        CrossFillV1 = 100,
        CrossFillH2 = 30,
        CrossFillS2 = 100,
        CrossFillV2 = 100,
        CrossFillTrans = 20,
        CrossFillRain = false,
        CrossFillRainSpd = 1,
        CrossStrVis = false,
        CrossStrThick = 1,
        CrossStrDir = "\231\183\154\230\128\16790\194\176",
        CrossStrH1 = 0,
        CrossStrS1 = 100,
        CrossStrV1 = 100,
        CrossStrTrans = 30,
        CrossStrRain = false,
        CrossStrRainSpd = 1.2,
        DotVis = false,
        DotSize = 5,
        DotDir = "\229\176\141\232\167\146\230\188\184\229\177\164",
        DotH1 = 0,
        DotS1 = 100,
        DotV1 = 100,
        DotH2 = 30,
        DotS2 = 100,
        DotV2 = 100,
        DotTrans = 20,
        DotRain = false,
        DotRainSpd = 1,
        TextVis = false,
        TextSize = 14,
        TextDir = "\231\183\154\230\128\1670\194\176",
        TextH1 = 0,
        TextS1 = 100,
        TextV1 = 100,
        TextH2 = 240,
        TextS2 = 100,
        TextV2 = 100,
        TextTrans = 30,
        TextX = 0,
        TextY = 40,
        TextRain = false,
        TextRainSpd = 1,
        RotEnabled = false,
        RotSpeed = 30,
    }
end

local CrossSettings = GetDefaultCrossSettings()
_G.XingHuiCrosshair.Settings = CrossSettings
local Crs_makeColor = function(h, s, v) return Color3.fromHSV(h/360, s/100, v/100) end
local Crs_applyGradient = function(g, c1, c2, mode)
    if not g then return end
    if mode == "\229\189\169\232\153\185\230\188\184\229\177\164" then
        g.Rotation = 0
        g.Color = ColorSequence.new({ColorSequenceKeypoint.new(0,Color3.fromRGB(255,0,0)),ColorSequenceKeypoint.new(0.2,Color3.fromRGB(255,255,0)),ColorSequenceKeypoint.new(0.4,Color3.fromRGB(0,255,0)),ColorSequenceKeypoint.new(0.6,Color3.fromRGB(0,255,255)),ColorSequenceKeypoint.new(0.8,Color3.fromRGB(0,0,255)),ColorSequenceKeypoint.new(1,Color3.fromRGB(255,0,255))})
    elseif mode == "\229\176\141\232\167\146\230\188\184\229\177\164" then
        g.Rotation = 45
        g.Color = ColorSequence.new({ColorSequenceKeypoint.new(0,c1),ColorSequenceKeypoint.new(0.5,c1:Lerp(c2,0.5)),ColorSequenceKeypoint.new(1,c2)})
    else
        local rot = 0
        if mode == "\231\183\154\230\128\16745\194\176" then rot = 45 elseif mode == "\231\183\154\230\128\16790\194\176" then rot = 90 elseif mode == "\231\183\154\230\128\167135\194\176" then rot = 135 end
        g.Rotation = rot
        g.Color = ColorSequence.new({ColorSequenceKeypoint.new(0,c1),ColorSequenceKeypoint.new(1,c2)})
    end
end

local CrossGui = Instance.new("ScreenGui", CoreGui)
_G.XingHuiCrosshair.Gui = CrossGui
CrossGui.Name = "HubCrosshair"; CrossGui.ResetOnSpawn = false; CrossGui.IgnoreGuiInset = true
local CrossContainer = Instance.new("Frame", CrossGui)
CrossContainer.AnchorPoint = Vector2.new(0.5,0.5); CrossContainer.Size = UDim2.new(0,400,0,400); CrossContainer.Position = UDim2.new(0.5,0,0.5,0); CrossContainer.BackgroundTransparency = 1

-- \229\187\186\231\171\139\230\186\150\230\152\159\229\133\131\231\180\160
local BigFill = Instance.new("Frame", CrossContainer); BigFill.AnchorPoint = Vector2.new(0.5,0.5); BigFill.Position = UDim2.new(0.5,0,0.5,0); BigFill.Size = UDim2.new(0,CrossSettings.BigFillDia,0,CrossSettings.BigFillDia); BigFill.BorderSizePixel = 0; BigFill.Visible = false; BigFill.BackgroundColor3 = Crs_makeColor(CrossSettings.BigFillH1,CrossSettings.BigFillS1,CrossSettings.BigFillV1); BigFill.BackgroundTransparency = CrossSettings.BigFillTrans/100; Instance.new("UICorner",BigFill).CornerRadius = UDim.new(1,0); local BigFillGrad = Instance.new("UIGradient",BigFill)
local BigStroke = Instance.new("Frame", CrossContainer); BigStroke.AnchorPoint = Vector2.new(0.5,0.5); BigStroke.Position = UDim2.new(0.5,0,0.5,0); BigStroke.Size = UDim2.new(0,CrossSettings.BigFillDia+CrossSettings.BigStrThick*2,0,CrossSettings.BigFillDia+CrossSettings.BigStrThick*2); BigStroke.BorderSizePixel = 0; BigStroke.Visible = false; BigStroke.BackgroundTransparency = 1; Instance.new("UICorner",BigStroke).CornerRadius = UDim.new(1,0); local BigStrokeStroke = Instance.new("UIStroke",BigStroke); BigStrokeStroke.Thickness = CrossSettings.BigStrThick; BigStrokeStroke.Color = Crs_makeColor(CrossSettings.BigStrH1,CrossSettings.BigStrS1,CrossSettings.BigStrV1); BigStrokeStroke.Transparency = CrossSettings.BigStrTrans/100
local SmlFill = Instance.new("Frame", CrossContainer); SmlFill.AnchorPoint = Vector2.new(0.5,0.5); SmlFill.Position = UDim2.new(0.5,0,0.5,0); SmlFill.Size = UDim2.new(0,CrossSettings.SmlFillDia,0,CrossSettings.SmlFillDia); SmlFill.BorderSizePixel = 0; SmlFill.Visible = false; SmlFill.BackgroundColor3 = Crs_makeColor(CrossSettings.SmlFillH1,CrossSettings.SmlFillS1,CrossSettings.SmlFillV1); SmlFill.BackgroundTransparency = CrossSettings.SmlFillTrans/100; Instance.new("UICorner",SmlFill).CornerRadius = UDim.new(1,0); local SmlFillGrad = Instance.new("UIGradient",SmlFill)
local SmlStroke = Instance.new("Frame", CrossContainer); SmlStroke.AnchorPoint = Vector2.new(0.5,0.5); SmlStroke.Position = UDim2.new(0.5,0,0.5,0); SmlStroke.Size = UDim2.new(0,CrossSettings.SmlFillDia+CrossSettings.SmlStrThick*2,0,CrossSettings.SmlFillDia+CrossSettings.SmlStrThick*2); SmlStroke.BorderSizePixel = 0; SmlStroke.Visible = false; SmlStroke.BackgroundTransparency = 1; Instance.new("UICorner",SmlStroke).CornerRadius = UDim.new(1,0); local SmlStrokeStroke = Instance.new("UIStroke",SmlStroke); SmlStrokeStroke.Thickness = CrossSettings.SmlStrThick; SmlStrokeStroke.Color = Crs_makeColor(CrossSettings.SmlStrH1,CrossSettings.SmlStrS1,CrossSettings.SmlStrV1); SmlStrokeStroke.Transparency = CrossSettings.SmlStrTrans/100

local CrossStrokeLines, CrossFillLines = {}, {}
for _, dir in ipairs({"Top","Bottom","Left","Right"}) do
    local stroke = Instance.new("Frame",CrossContainer); stroke.AnchorPoint = Vector2.new(0.5,0.5); stroke.BorderSizePixel = 0; stroke.Visible = false; stroke.BackgroundTransparency = 1; local strokeStroke = Instance.new("UIStroke",stroke); strokeStroke.Thickness = CrossSettings.CrossStrThick; strokeStroke.Color = Crs_makeColor(CrossSettings.CrossStrH1,CrossSettings.CrossStrS1,CrossSettings.CrossStrV1); strokeStroke.Transparency = CrossSettings.CrossStrTrans/100; CrossStrokeLines[dir] = stroke
    local fill = Instance.new("Frame",CrossContainer); fill.AnchorPoint = Vector2.new(0.5,0.5); fill.BorderSizePixel = 0; fill.Visible = false; fill.BackgroundColor3 = Crs_makeColor(CrossSettings.CrossFillH1,CrossSettings.CrossFillS1,CrossSettings.CrossFillV1); fill.BackgroundTransparency = CrossSettings.CrossFillTrans/100; Instance.new("UIGradient",fill); CrossFillLines[dir] = fill
end
function Crs_updateCrossLines()
    local sz, thick, gap = CrossSettings.Size, CrossSettings.Thickness, CrossSettings.Gap
    local fillLength = sz - gap
    CrossStrokeLines.Top.Size = UDim2.new(0,thick,0,fillLength); CrossStrokeLines.Top.Position = UDim2.new(0.5,0,0.5,-(sz+gap)/2)
    CrossStrokeLines.Bottom.Size = UDim2.new(0,thick,0,fillLength); CrossStrokeLines.Bottom.Position = UDim2.new(0.5,0,0.5,(sz+gap)/2)
    CrossStrokeLines.Left.Size = UDim2.new(0,fillLength,0,thick); CrossStrokeLines.Left.Position = UDim2.new(0.5,-(sz+gap)/2,0.5,0)
    CrossStrokeLines.Right.Size = UDim2.new(0,fillLength,0,thick); CrossStrokeLines.Right.Position = UDim2.new(0.5,(sz+gap)/2,0.5,0)
    CrossFillLines.Top.Size = UDim2.new(0,thick,0,fillLength); CrossFillLines.Top.Position = UDim2.new(0.5,0,0.5,-(sz+gap)/2)
    CrossFillLines.Bottom.Size = UDim2.new(0,thick,0,fillLength); CrossFillLines.Bottom.Position = UDim2.new(0.5,0,0.5,(sz+gap)/2)
    CrossFillLines.Left.Size = UDim2.new(0,fillLength,0,thick); CrossFillLines.Left.Position = UDim2.new(0.5,-(sz+gap)/2,0.5,0)
    CrossFillLines.Right.Size = UDim2.new(0,fillLength,0,thick); CrossFillLines.Right.Position = UDim2.new(0.5,(sz+gap)/2,0.5,0)
end
Crs_updateCrossLines()
local CenterDot = Instance.new("Frame",CrossContainer); CenterDot.AnchorPoint = Vector2.new(0.5,0.5); CenterDot.Position = UDim2.new(0.5,0,0.5,0); CenterDot.Size = UDim2.new(0,CrossSettings.DotSize,0,CrossSettings.DotSize); CenterDot.BorderSizePixel = 0; CenterDot.Visible = false; CenterDot.BackgroundColor3 = Crs_makeColor(CrossSettings.DotH1,CrossSettings.DotS1,CrossSettings.DotV1); CenterDot.BackgroundTransparency = CrossSettings.DotTrans/100; Instance.new("UICorner",CenterDot).CornerRadius = UDim.new(1,0); local DotGrad = Instance.new("UIGradient",CenterDot)
local CrossText = Instance.new("TextLabel",CrossGui); CrossText.AnchorPoint = Vector2.new(0.5,0); CrossText.Size = UDim2.new(0,200,0,30); CrossText.BackgroundTransparency = 1; CrossText.Text = "XingHui\230\152\159\232\188\157"; CrossText.Font = Enum.Font.GothamBold; CrossText.TextXAlignment = Enum.TextXAlignment.Center; CrossText.TextSize = CrossSettings.TextSize; CrossText.Visible = false; CrossText.TextColor3 = Crs_makeColor(CrossSettings.TextH1,CrossSettings.TextS1,CrossSettings.TextV1); CrossText.TextTransparency = CrossSettings.TextTrans/100; local TextGrad = Instance.new("UIGradient",CrossText)

local previewUpdater = nil
local crossHeartbeat
local function Crs_startCrosshair()
    _G.XingHuiCrosshair.Start = Crs_startCrosshair
    if crossHeartbeat then return end
    crossHeartbeat = RunService.Heartbeat:Connect(function(dt)
        _G.XingHuiCrosshair.Heartbeat = crossHeartbeat
        if not CrossSettings.Enabled then
            BigStroke.Visible, BigFill.Visible, SmlStroke.Visible, SmlFill.Visible = false,false,false,false
            for _,v in pairs(CrossStrokeLines) do v.Visible = false end
            for _,v in pairs(CrossFillLines) do v.Visible = false end
            CenterDot.Visible, CrossText.Visible = false,false; return
        end
        BigStroke.Visible, BigFill.Visible = CrossSettings.BigStrVis, CrossSettings.BigFillVis
        SmlStroke.Visible, SmlFill.Visible = CrossSettings.SmlStrVis, CrossSettings.SmlFillVis
        for _,v in pairs(CrossStrokeLines) do v.Visible = CrossSettings.CrossStrVis end
        for _,v in pairs(CrossFillLines) do v.Visible = CrossSettings.CrossFillVis end
        CenterDot.Visible, CrossText.Visible = CrossSettings.DotVis, CrossSettings.TextVis
        if CrossSettings.RotEnabled then CrossContainer.Rotation = (CrossContainer.Rotation + CrossSettings.RotSpeed*dt)%360 end
        BigFill.Size = UDim2.new(0,CrossSettings.BigFillDia,0,CrossSettings.BigFillDia)
        BigStroke.Size = UDim2.new(0,CrossSettings.BigFillDia+CrossSettings.BigStrThick*2,0,CrossSettings.BigFillDia+CrossSettings.BigStrThick*2)
        SmlFill.Size = UDim2.new(0,CrossSettings.SmlFillDia,0,CrossSettings.SmlFillDia)
        SmlStroke.Size = UDim2.new(0,CrossSettings.SmlFillDia+CrossSettings.SmlStrThick*2,0,CrossSettings.SmlFillDia+CrossSettings.SmlStrThick*2)
        CenterDot.Size = UDim2.new(0,CrossSettings.DotSize,0,CrossSettings.DotSize)
        BigStrokeStroke.Thickness = CrossSettings.BigStrThick
        if CrossSettings.BigStrRain then BigStrokeStroke.Color = Color3.fromHSV((tick()*CrossSettings.BigStrRainSpd*60)%360/360,1,1) else BigStrokeStroke.Color = Crs_makeColor(CrossSettings.BigStrH1,CrossSettings.BigStrS1,CrossSettings.BigStrV1) end
        BigStrokeStroke.Transparency = CrossSettings.BigStrTrans/100
        SmlStrokeStroke.Thickness = CrossSettings.SmlStrThick
        if CrossSettings.SmlStrRain then SmlStrokeStroke.Color = Color3.fromHSV((tick()*CrossSettings.SmlStrRainSpd*60)%360/360,1,1) else SmlStrokeStroke.Color = Crs_makeColor(CrossSettings.SmlStrH1,CrossSettings.SmlStrS1,CrossSettings.SmlStrV1) end
        SmlStrokeStroke.Transparency = CrossSettings.SmlStrTrans/100
        for _,stroke in pairs(CrossStrokeLines) do
            local s = stroke:FindFirstChildOfClass("UIStroke")
            if s then s.Thickness = CrossSettings.CrossStrThick
                if CrossSettings.CrossStrRain then s.Color = Color3.fromHSV((tick()*CrossSettings.CrossStrRainSpd*60)%360/360,1,1) else s.Color = Crs_makeColor(CrossSettings.CrossStrH1,CrossSettings.CrossStrS1,CrossSettings.CrossStrV1) end
                s.Transparency = CrossSettings.CrossStrTrans/100
            end
        end
        local function updateComp(comp,grad,h1,s1,v1,h2,s2,v2,dir,rain,rainSpd)
            if rain then local hue = (tick()*rainSpd*60)%360/360; Crs_applyGradient(grad,Color3.fromHSV(hue,1,1),Color3.fromHSV((hue+0.3)%1,1,1),dir); comp.BackgroundColor3 = Color3.fromHSV(hue,1,1)
            else Crs_applyGradient(grad,Crs_makeColor(h1,s1,v1),Crs_makeColor(h2,s2,v2),dir); comp.BackgroundColor3 = Crs_makeColor(h1,s1,v1) end
        end
        updateComp(BigFill,BigFillGrad,CrossSettings.BigFillH1,CrossSettings.BigFillS1,CrossSettings.BigFillV1,CrossSettings.BigFillH2,CrossSettings.BigFillS2,CrossSettings.BigFillV2,CrossSettings.BigFillDir,CrossSettings.BigFillRain,CrossSettings.BigFillRainSpd)
        updateComp(SmlFill,SmlFillGrad,CrossSettings.SmlFillH1,CrossSettings.SmlFillS1,CrossSettings.SmlFillV1,CrossSettings.SmlFillH2,CrossSettings.SmlFillS2,CrossSettings.SmlFillV2,CrossSettings.SmlFillDir,CrossSettings.SmlFillRain,CrossSettings.SmlFillRainSpd)
        updateComp(CenterDot,DotGrad,CrossSettings.DotH1,CrossSettings.DotS1,CrossSettings.DotV1,CrossSettings.DotH2,CrossSettings.DotS2,CrossSettings.DotV2,CrossSettings.DotDir,CrossSettings.DotRain,CrossSettings.DotRainSpd)
        for _,fill in pairs(CrossFillLines) do
            if CrossSettings.CrossFillRain then local hue = (tick()*CrossSettings.CrossFillRainSpd*60)%360/360; Crs_applyGradient(fill:FindFirstChildOfClass("UIGradient"),Color3.fromHSV(hue,1,1),Color3.fromHSV((hue+0.3)%1,1,1),CrossSettings.CrossFillDir); fill.BackgroundColor3 = Color3.fromHSV(hue,1,1)
            else Crs_applyGradient(fill:FindFirstChildOfClass("UIGradient"),Crs_makeColor(CrossSettings.CrossFillH1,CrossSettings.CrossFillS1,CrossSettings.CrossFillV1),Crs_makeColor(CrossSettings.CrossFillH2,CrossSettings.CrossFillS2,CrossSettings.CrossFillV2),CrossSettings.CrossFillDir); fill.BackgroundColor3 = Crs_makeColor(CrossSettings.CrossFillH1,CrossSettings.CrossFillS1,CrossSettings.CrossFillV1) end
        end
        if CrossSettings.TextRain then local hue = (tick()*CrossSettings.TextRainSpd*60)%360/360; Crs_applyGradient(TextGrad,Color3.fromHSV(hue,1,1),Color3.fromHSV((hue+0.3)%1,1,1),CrossSettings.TextDir); CrossText.TextColor3 = Color3.fromHSV(hue,1,1)
        else Crs_applyGradient(TextGrad,Crs_makeColor(CrossSettings.TextH1,CrossSettings.TextS1,CrossSettings.TextV1),Crs_makeColor(CrossSettings.TextH2,CrossSettings.TextS2,CrossSettings.TextV2),CrossSettings.TextDir); CrossText.TextColor3 = Crs_makeColor(CrossSettings.TextH1,CrossSettings.TextS1,CrossSettings.TextV1) end
        BigFill.BackgroundTransparency = CrossSettings.BigFillTrans/100
        SmlFill.BackgroundTransparency = CrossSettings.SmlFillTrans/100
        CenterDot.BackgroundTransparency = CrossSettings.DotTrans/100
        CrossText.TextTransparency = CrossSettings.TextTrans/100
        for _,fill in pairs(CrossFillLines) do fill.BackgroundTransparency = CrossSettings.CrossFillTrans/100 end
        CrossText.Position = UDim2.new(0.5,CrossSettings.TextX,0.5,CrossSettings.TextY)
        CrossText.TextSize = CrossSettings.TextSize
        Crs_updateCrossLines()
        if previewUpdater then previewUpdater() end
    end)
end

local activeSlider = nil
UserInputService.TouchMoved:Connect(function(touch) if activeSlider then activeSlider(touch.Position) end end)
UserInputService.InputEnded:Connect(function(input) if input.UserInputType == Enum.UserInputType.Touch then activeSlider = nil end end)

local function createToggle(parent, name, defState, callback)
    local btn = Instance.new("TextButton", parent)
    btn.Size = UDim2.new(1, -6, 0, 30)
    btn.BackgroundColor3 = defState and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 60)
    btn.Text = name .. (defState and " ON" or " OFF")
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 13
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
    local state = defState
    btn.Activated:Connect(function()
        state = not state
        btn.Text = name .. (state and " ON" or " OFF")
        btn.BackgroundColor3 = state and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 60)
        callback(state)
    end)
    return btn
end

local function createSlider(parent, name, minV, maxV, defV, callback)
    local c = Instance.new("Frame", parent)
    c.Size = UDim2.new(1, -6, 0, 40)
    c.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
    Instance.new("UICorner", c).CornerRadius = UDim.new(0, 8)
    local l = Instance.new("TextLabel", c)
    l.Size = UDim2.new(1, 0, 0, 18)
    l.Position = UDim2.new(0, 6, 0, 2)
    l.BackgroundTransparency = 1
    l.Text = name .. ": " .. string.format("%.1f", defV)
    l.TextColor3 = Color3.new(1, 1, 1)
    l.Font = Enum.Font.Gotham
    l.TextSize = 12
    local bar = Instance.new("Frame", c)
    bar.Size = UDim2.new(1, -16, 0, 6)
    bar.Position = UDim2.new(0, 8, 0, 26)
    bar.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
    Instance.new("UICorner", bar).CornerRadius = UDim.new(0, 3)
    local fill = Instance.new("Frame", bar)
    fill.Size = UDim2.new((defV - minV) / (maxV - minV), 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromHSV(0.58, 1, 1)
    Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 3)
    local function update(pos)
        local x = math.clamp((pos.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
        fill.Size = UDim2.new(x, 0, 1, 0)
        local val = minV + (maxV - minV) * x
        l.Text = name .. ": " .. string.format("%.1f", val)
        callback(val)
    end
    bar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
            activeSlider = update
            update(input.Position)
        end
    end)
    return c
end

local function createFloatingDropdown(parent, panel, name, options, defChoice, callback)
    local mainBtn = Instance.new("TextButton", parent)
    mainBtn.Size = UDim2.new(1, -6, 0, 30)
    mainBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
    mainBtn.Text = defChoice .. " \240\159\148\189"
    mainBtn.TextColor3 = Color3.new(1, 1, 1)
    mainBtn.Font = Enum.Font.GothamBold
    mainBtn.TextSize = 13
    Instance.new("UICorner", mainBtn).CornerRadius = UDim.new(0, 8)
    local popup = Instance.new("Frame", panel)
    popup.Size = UDim2.new(0, 0, 0, 0)
    popup.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    popup.BorderSizePixel = 0
    popup.ClipsDescendants = true
    popup.Visible = false
    popup.ZIndex = 10
    Instance.new("UICorner", popup).CornerRadius = UDim.new(0, 6)
    local expanded = false
    for i, opt in ipairs(options) do
        local optBtn = Instance.new("TextButton", popup)
        optBtn.Size = UDim2.new(1, -8, 0, 28)
        optBtn.Position = UDim2.new(0, 4, 0, 4 + (i - 1) * 30)
        optBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
        optBtn.Text = opt
        optBtn.TextColor3 = Color3.new(1, 1, 1)
        optBtn.Font = Enum.Font.Gotham
        optBtn.TextSize = 12
        optBtn.ZIndex = 11
        Instance.new("UICorner", optBtn).CornerRadius = UDim.new(0, 4)
        optBtn.Activated:Connect(function()
            mainBtn.Text = opt .. " \240\159\148\189"
            callback(opt)
            popup.Visible = false
            expanded = false
        end)
    end
    local function updatePopupPosition()
        local absPos = mainBtn.AbsolutePosition
        local absSize = mainBtn.AbsoluteSize
        local panelAbs = panel.AbsolutePosition
        popup.Position = UDim2.new(0, absPos.X - panelAbs.X + 8, 0, absPos.Y - panelAbs.Y + absSize.Y + 2)
        popup.Size = UDim2.new(0, absSize.X - 16, 0, #options * 30 + 8)
    end
    mainBtn.Activated:Connect(function()
        expanded = not expanded
        if expanded then
            updatePopupPosition()
            popup.Visible = true
            mainBtn.Text = defChoice .. " \226\172\134\239\184\143"
        else
            popup.Visible = false
            mainBtn.Text = defChoice .. " \240\159\148\189"
        end
    end)
    return mainBtn
end

local function createDivider(parent)
    local line = Instance.new("Frame", parent)
    line.Size = UDim2.new(1, -6, 0, 1)
    line.BackgroundColor3 = Color3.fromRGB(80, 80, 90)
    line.BorderSizePixel = 0
    return line
end

local container = gethui and gethui() or CoreGui
for _, v in ipairs(container:GetChildren()) do
    if v.Name == "\230\152\159\232\188\157Hub" then v:Destroy() end
end

local screenGui = Instance.new("ScreenGui", container)
_G.XingHuiCrosshair.Menu = screenGui
screenGui.Name = "\230\152\159\232\188\157Hub"
screenGui.ResetOnSpawn = false

-- ========== 1.5\229\128\141\230\148\190\229\164\167\229\176\186\229\175\184 ==========
local panelWidth = 825   -- 550*1.5
local panelHeight = 435  -- 290*1.5
local centerX, centerY = 0.5, 0.5

local tabPos = {x = 17, y = 68, w = 180, h = 278}          -- 120*1.5, 185*1.5
local contentPos = {x = 210, y = 53, w = 600, h = 368}     -- 140*1.5, 35*1.5, 400*1.5, 245*1.5
local avatarPos = {x = 15, y = 353, w = 105, h = 105}      -- 70*1.5, 235*1.5
local nameTextPos = {x = 83, y = 18}                       -- 55*1.5, 12*1.5
local displayTextPos = {x = 83, y = 45}                    -- 30*1.5
local hubTitle = "\230\152\159\232\188\157hub"
local titleFontSize = 45                                   -- 30*1.5
local titleBarPos = {x = 8, y = 0, w = 825, h = 75}        -- 550*1.5, 50*1.5
local closeBtnPos = {x = 780, y = 5, w = 38, h = 38}       -- 520*1.5, 3*1.5, 25*1.5

local isLocked = true
-- \226\152\133 \230\140\137\233\136\1491.25\229\128\141\239\188\140\233\150\147\232\183\157\231\184\174\229\176\143\232\135\17917 \226\152\133
-- \230\152\159\232\188\157hub Y=45\239\188\140\233\171\152\229\186\16638\239\188\140Unlock Y=100 \226\134\146 \233\150\147\232\183\157 = 100 - (45+38) = 17
local lockBtnData = {x = 5, y = 100, w = 75, h = 38, text = "Unlock", fontSize = 15}   -- 60*1.25=75, 30*1.25=37.5\226\134\14638, \229\173\151\233\171\14812*1.25=15
local hubBtnData = {x = 5, y = 45, w = 88, h = 38, text = "\230\152\159\232\188\157hub", fontSize = 19}   -- 70*1.25=87.5\226\134\14688, 30*1.25=37.5\226\134\14638, \229\173\151\233\171\14815*1.25=18.75\226\134\14619
local padding = 1

local LockBtnFrame, LockBtn, HubBtnFrame, HubBtn
local Panel

local function makeDraggable(btn, frame, data, shortPressCallback)
    local dragInfo = nil
    btn.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragInfo = {
                startPos = Vector2.new(frame.Position.X.Offset, frame.Position.Y.Offset),
                dragStart = input.Position,
                moved = false,
                inputConn = nil,
                endConn = nil
            }
            dragInfo.inputConn = UserInputService.InputChanged:Connect(function(changedInput)
                if not dragInfo then return end
                if changedInput.UserInputType == Enum.UserInputType.MouseMovement or changedInput.UserInputType == Enum.UserInputType.Touch then
                    local delta = changedInput.Position - dragInfo.dragStart
                    if delta.Magnitude > 5 and not isLocked then
                        dragInfo.moved = true
                        frame.Position = UDim2.new(0, dragInfo.startPos.X + delta.X, 0, dragInfo.startPos.Y + delta.Y)
                    end
                end
            end)
            dragInfo.endConn = btn.InputEnded:Connect(function(endInput)
                if not dragInfo then return end
                if dragInfo.inputConn then dragInfo.inputConn:Disconnect() end
                if dragInfo.endConn then dragInfo.endConn:Disconnect() end
                local moved = dragInfo.moved
                dragInfo = nil
                if moved then
                    data.x = math.floor(frame.Position.X.Offset + padding + 0.5)
                    data.y = math.floor(frame.Position.Y.Offset + padding + 0.5)
                else
                    if shortPressCallback then shortPressCallback() end
                end
            end)
        end
    end)
end

local function rebuildButtons()
    LockBtnFrame.Visible = false
    HubBtnFrame.Visible = false
    if LockBtnFrame then LockBtnFrame:Destroy() end
    if HubBtnFrame then HubBtnFrame:Destroy() end
    LockBtnFrame = Instance.new("Frame", screenGui)
    _G.XingHuiCrosshair.LockBtnFrame = LockBtnFrame
    LockBtnFrame.Size = UDim2.new(0, lockBtnData.w + padding*2, 0, lockBtnData.h + padding*2)
    LockBtnFrame.Position = UDim2.new(0, lockBtnData.x - padding, 0, lockBtnData.y - padding)
    LockBtnFrame.BackgroundColor3 = Color3.new(1, 1, 1)
    LockBtnFrame.BorderSizePixel = 0
    LockBtnFrame.ZIndex = 50
    LockBtn = Instance.new("TextButton", LockBtnFrame)
    LockBtn.Size = UDim2.new(1, -padding*2, 1, -padding*2)
    LockBtn.Position = UDim2.new(0, padding, 0, padding)
    LockBtn.BackgroundColor3 = Color3.new(0, 0, 0)
    LockBtn.Text = lockBtnData.text
    LockBtn.TextColor3 = Color3.new(1, 1, 1)
    LockBtn.Font = Enum.Font.Montserrat
    LockBtn.TextSize = lockBtnData.fontSize
    LockBtn.BorderSizePixel = 0
    LockBtn.AutoButtonColor = false
    LockBtn.ZIndex = 51
    HubBtnFrame = Instance.new("Frame", screenGui)
    _G.XingHuiCrosshair.HubBtnFrame = HubBtnFrame
    HubBtnFrame.Size = UDim2.new(0, hubBtnData.w + padding*2, 0, hubBtnData.h + padding*2)
    HubBtnFrame.Position = UDim2.new(0, hubBtnData.x - padding, 0, hubBtnData.y - padding)
    HubBtnFrame.BackgroundColor3 = Color3.new(1, 1, 1)
    HubBtnFrame.BorderSizePixel = 0
    HubBtnFrame.ZIndex = 50
    HubBtn = Instance.new("TextButton", HubBtnFrame)
    HubBtn.Size = UDim2.new(1, -padding*2, 1, -padding*2)
    HubBtn.Position = UDim2.new(0, padding, 0, padding)
    HubBtn.BackgroundColor3 = Color3.new(0, 0, 0)
    HubBtn.Text = hubBtnData.text
    HubBtn.TextColor3 = Color3.new(1, 1, 1)
    HubBtn.Font = Enum.Font.Montserrat
    HubBtn.TextSize = hubBtnData.fontSize
    HubBtn.BorderSizePixel = 0
    HubBtn.AutoButtonColor = false
    HubBtn.ZIndex = 51
    makeDraggable(LockBtn, LockBtnFrame, lockBtnData, function()
        isLocked = not isLocked
        lockBtnData.text = isLocked and "Unlock" or "Lock"
        LockBtn.Text = lockBtnData.text
    end)
    makeDraggable(HubBtn, HubBtnFrame, hubBtnData, function()
        if Panel then Panel.Visible = not Panel.Visible end
    end)
end
rebuildButtons()
    LockBtnFrame.Visible = false
    HubBtnFrame.Visible = false

Panel = Instance.new("Frame", screenGui)
_G.XingHuiCrosshair.Panel = Panel
Panel.Size = UDim2.new(0, panelWidth, 0, panelHeight)
Panel.Position = UDim2.new(centerX, -panelWidth/2, centerY, -panelHeight/2)
Panel.BackgroundColor3 = Color3.fromRGB(18, 18, 22)
Panel.BackgroundTransparency = 0.5
Panel.BorderSizePixel = 0
Panel.ClipsDescendants = true
Panel.Visible = false

local PanelStroke = Instance.new("UIStroke", Panel)
PanelStroke.Thickness = 2
PanelStroke.Color = Color3.new(1, 1, 1)
PanelStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local TitleBar = Instance.new("Frame", Panel)
TitleBar.Size = UDim2.new(0, titleBarPos.w, 0, titleBarPos.h)
TitleBar.Position = UDim2.new(0, titleBarPos.x, 0, titleBarPos.y)
TitleBar.BackgroundColor3 = Color3.fromRGB(12, 12, 15)
TitleBar.BackgroundTransparency = 0.3
TitleBar.BorderSizePixel = 0

local TitleText = Instance.new("TextLabel", TitleBar)
TitleText.Size = UDim2.new(1, -50, 1, 0)
TitleText.Position = UDim2.new(0, 12, 0, 0)
TitleText.BackgroundTransparency = 1
TitleText.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleText.TextXAlignment = Enum.TextXAlignment.Left
TitleText.Font = Enum.Font.GothamBold
TitleText.TextSize = titleFontSize
TitleText.Text = hubTitle

local CloseBtn = Instance.new("TextButton", Panel)
CloseBtn.Size = UDim2.new(0, closeBtnPos.w, 0, closeBtnPos.h)
CloseBtn.Position = UDim2.new(0, closeBtnPos.x, 0, closeBtnPos.y)
CloseBtn.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
CloseBtn.Text = "X"
CloseBtn.TextColor3 = Color3.new(255, 255, 255)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 21  -- 14*1.5
CloseBtn.BorderSizePixel = 0
CloseBtn.AutoButtonColor = false
CloseBtn.MouseButton1Click:Connect(function() Panel.Visible = false end)

local TabArea = Instance.new("ScrollingFrame", Panel)
TabArea.Size = UDim2.new(0, tabPos.w, 0, tabPos.h)
TabArea.Position = UDim2.new(0, tabPos.x, 0, tabPos.y)
TabArea.BackgroundColor3 = Color3.fromRGB(22, 22, 28)
TabArea.BorderSizePixel = 0
TabArea.ScrollBarThickness = 6
TabArea.ScrollBarImageColor3 = Color3.new(1, 1, 1)
TabArea.CanvasSize = UDim2.new(0, 0, 0, 0)
TabArea.ClipsDescendants = true

local TabStroke = Instance.new("UIStroke", TabArea)
TabStroke.Thickness = 2
TabStroke.Color = Color3.new(1, 1, 1)
TabStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local TabList = Instance.new("Frame", TabArea)
TabList.Size = UDim2.new(1, 0, 0, 0)
TabList.BackgroundTransparency = 1
local TabLayout = Instance.new("UIListLayout", TabList)
TabLayout.Padding = UDim.new(0, 8)
TabLayout.SortOrder = Enum.SortOrder.LayoutOrder
TabLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center

local TabNames = {"\230\148\187\230\147\138", "\232\166\150\232\166\186", "\230\186\150\230\152\159", "\232\168\173\229\174\154"}
local TabButtons = {}
local TabContents = {}

for i, name in ipairs(TabNames) do
    local btn = Instance.new("TextButton", TabList)
    btn.Size = UDim2.new(1, -12, 0, 57)  -- 38*1.5
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 60)
    btn.Text = name
    btn.TextColor3 = Color3.new(1, 1, 1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 24   -- 16*1.5
    btn.TextTruncate = Enum.TextTruncate.None
    btn.TextXAlignment = Enum.TextXAlignment.Center
    TabButtons[i] = btn
end
TabArea.CanvasSize = UDim2.new(0, 0, 0, TabLayout.AbsoluteContentSize.Y + 10)

local ContentArea = Instance.new("Frame", Panel)
ContentArea.Size = UDim2.new(0, contentPos.w, 0, contentPos.h)
ContentArea.Position = UDim2.new(0, contentPos.x, 0, contentPos.y)
ContentArea.BackgroundColor3 = Color3.fromRGB(28, 28, 34)
ContentArea.BackgroundTransparency = 0.3
ContentArea.BorderSizePixel = 0
ContentArea.ClipsDescendants = true

local ContentStroke = Instance.new("UIStroke", ContentArea)
ContentStroke.Thickness = 2
ContentStroke.Color = Color3.new(1, 1, 1)
ContentStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

for i = 1, #TabNames do
    local content = Instance.new("Frame", ContentArea)
    content.Size = UDim2.new(1, 0, 1, 0)
    content.BackgroundTransparency = 1
    content.Visible = (i == 1)
    TabContents[i] = content
end

for i, btn in ipairs(TabButtons) do
    btn.MouseButton1Click:Connect(function()
        for j, b in ipairs(TabButtons) do
            TweenService:Create(b, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(50, 50, 60)}):Play()
        end
        TweenService:Create(btn, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(100, 100, 120)}):Play()
        for j, c in ipairs(TabContents) do c.Visible = (j == i) end
    end)
end
TweenService:Create(TabButtons[1], TweenInfo.new(0.01), {BackgroundColor3 = Color3.fromRGB(100, 100, 120)}):Play()

local AvatarContainer = Instance.new("Frame", Panel)
AvatarContainer.Size = UDim2.new(0, avatarPos.w, 0, avatarPos.h)
AvatarContainer.Position = UDim2.new(0, avatarPos.x, 0, avatarPos.y)
AvatarContainer.BackgroundTransparency = 1
AvatarContainer.ZIndex = 10
local AvatarImage = Instance.new("ImageLabel", AvatarContainer)
AvatarImage.Size = UDim2.new(0, 72, 0, 72)  -- 48*1.5
AvatarImage.Position = UDim2.new(0, 0, 0, 0)
AvatarImage.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
AvatarImage.BorderSizePixel = 0
AvatarImage.Image = "https://www.roblox.com/headshot-thumbnail/image?userId=" .. player.UserId .. "&width=150&height=150&format=png"
local AvatarName = Instance.new("TextLabel", AvatarContainer)
AvatarName.Size = UDim2.new(0, 270, 0, 30)  -- 180*1.5, 20*1.5
AvatarName.Position = UDim2.new(0, nameTextPos.x, 0, nameTextPos.y)
AvatarName.BackgroundTransparency = 1
AvatarName.Text = player.Name
AvatarName.TextColor3 = Color3.new(1, 1, 1)
AvatarName.Font = Enum.Font.GothamBold
AvatarName.TextSize = 21  -- 14*1.5
AvatarName.TextXAlignment = Enum.TextXAlignment.Left
local AvatarDisplay = Instance.new("TextLabel", AvatarContainer)
AvatarDisplay.Size = UDim2.new(0, 270, 0, 24)  -- 180*1.5, 16*1.5
AvatarDisplay.Position = UDim2.new(0, displayTextPos.x, 0, displayTextPos.y)
AvatarDisplay.BackgroundTransparency = 1
AvatarDisplay.Text = "@" .. player.DisplayName
AvatarDisplay.TextColor3 = Color3.new(0.7, 0.7, 0.7)
AvatarDisplay.Font = Enum.Font.Gotham
AvatarDisplay.TextSize = 18  -- 12*1.5
AvatarDisplay.TextXAlignment = Enum.TextXAlignment.Left

local function setupPanelDrag(dragEl, targetEl)
    local dragInfo = nil
    dragEl.InputBegan:Connect(function(input)
        if (input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1) and not isLocked then
            dragInfo = {
                startPos = targetEl.Position,
                dragStart = input.Position,
                inputConn = nil,
                endConn = nil
            }
            dragInfo.inputConn = UserInputService.InputChanged:Connect(function(changedInput)
                if not dragInfo then return end
                if changedInput.UserInputType == Enum.UserInputType.Touch or changedInput.UserInputType == Enum.UserInputType.MouseMovement then
                    local delta = changedInput.Position - dragInfo.dragStart
                    targetEl.Position = UDim2.new(
                        dragInfo.startPos.X.Scale, dragInfo.startPos.X.Offset + delta.X,
                        dragInfo.startPos.Y.Scale, dragInfo.startPos.Y.Offset + delta.Y
                    )
                end
            end)
            dragInfo.endConn = dragEl.InputEnded:Connect(function()
                if dragInfo then
                    dragInfo.inputConn:Disconnect()
                    dragInfo.endConn:Disconnect()
                    dragInfo = nil
                end
            end)
        end
    end)
end
setupPanelDrag(TitleBar, Panel)

-- ============================================
-- \230\148\187\230\147\138\229\136\134\233\160\129 (\229\174\140\229\133\168\231\169\186\231\153\189)
-- ============================================
local function BuildAttackTab()
    local attackContent = TabContents[1]
    for _, child in ipairs(attackContent:GetChildren()) do child:Destroy() end
end

local function BuildVisualTab()
    local visualContent = TabContents[2]
    for _, child in ipairs(visualContent:GetChildren()) do child:Destroy() end
end

local function BuildCrossTab()
    local crossContent = TabContents[3]
    for _, child in ipairs(crossContent:GetChildren()) do child:Destroy() end

    local leftColumn = Instance.new("Frame", crossContent)
    leftColumn.Size = UDim2.new(0.6, -6, 1, 0)
    leftColumn.Position = UDim2.new(0, 3, 0, 0)
    leftColumn.BackgroundTransparency = 1

    local topBar = Instance.new("Frame", leftColumn)
    topBar.Size = UDim2.new(1, 0, 0, 36)
    topBar.BackgroundTransparency = 1

    local mainContentContainer
    createToggle(topBar, "\229\149\159\231\148\168\230\186\150\230\152\159", CrossSettings.Enabled, function(b)
        CrossSettings.Enabled = b
        if b then Crs_startCrosshair() else if crossHeartbeat then crossHeartbeat:Disconnect(); crossHeartbeat = nil end end
        if mainContentContainer then mainContentContainer.Visible = b end
        if previewUpdater then previewUpdater() end
    end)

    local CrossScroll = Instance.new("ScrollingFrame", leftColumn)
    CrossScroll.Size = UDim2.new(1, 0, 1, -42)
    CrossScroll.Position = UDim2.new(0, 0, 0, 42)
    CrossScroll.BackgroundTransparency = 1
    CrossScroll.ScrollBarThickness = 8
    CrossScroll.CanvasSize = UDim2.new(0, 0, 0, 3000)

    local CrossList = Instance.new("UIListLayout", CrossScroll)
    CrossList.Padding = UDim.new(0, 4)

    local function updateCanvasSize()
        if CrossList and CrossList.AbsoluteContentSize.Y > 0 then
            CrossScroll.CanvasSize = UDim2.new(0, 0, 0, CrossList.AbsoluteContentSize.Y + 20)
        end
    end
    CrossList:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(updateCanvasSize)
    task.spawn(function() while true do task.wait(0.3); updateCanvasSize() end end)

    mainContentContainer = Instance.new("Frame", CrossScroll)
    mainContentContainer.Size = UDim2.new(1, -6, 0, 0)
    mainContentContainer.BackgroundTransparency = 1
    mainContentContainer.Visible = CrossSettings.Enabled

    local mainLayout = Instance.new("UIListLayout", mainContentContainer)
    mainLayout.Padding = UDim.new(0, 4)

    local function Crs_createToggle(parent, name, default, callback)
        local btn = Instance.new("TextButton", parent)
        btn.Size = UDim2.new(1, -6, 0, 30)
        btn.BackgroundColor3 = default and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 60)
        btn.Text = name .. (default and " ON" or " OFF")
        btn.TextColor3 = Color3.new(1, 1, 1)
        btn.Font = Enum.Font.GothamBold
        btn.TextSize = 13
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
        local state = default
        btn.Activated:Connect(function()
            state = not state
            btn.Text = name .. (state and " ON" or " OFF")
            btn.BackgroundColor3 = state and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 60)
            callback(state)
        end)
        return btn
    end

    local function Crs_createSlider(parent, name, minV, maxV, defV, step, callback)
        local c = Instance.new("Frame", parent)
        c.Size = UDim2.new(1, -6, 0, 40)
        c.BackgroundColor3 = Color3.fromRGB(40, 40, 48)
        Instance.new("UICorner", c).CornerRadius = UDim.new(0, 8)
        local l = Instance.new("TextLabel", c)
        l.Size = UDim2.new(1, 0, 0, 18)
        l.Position = UDim2.new(0, 6, 0, 2)
        l.BackgroundTransparency = 1
        l.Text = name .. ": " .. tostring(defV)
        l.TextColor3 = Color3.new(1, 1, 1)
        l.Font = Enum.Font.Gotham
        l.TextSize = 12
        local bar = Instance.new("Frame", c)
        bar.Size = UDim2.new(1, -16, 0, 6)
        bar.Position = UDim2.new(0, 8, 0, 26)
        bar.BackgroundColor3 = Color3.fromRGB(60, 60, 70)
        bar.ZIndex = 10
        Instance.new("UICorner", bar).CornerRadius = UDim.new(0, 3)
        local fill = Instance.new("Frame", bar)
        fill.Size = UDim2.new((defV - minV) / (maxV - minV), 0, 1, 0)
        fill.BackgroundColor3 = Color3.fromHSV(0.33, 1, 1)
        Instance.new("UICorner", fill).CornerRadius = UDim.new(0, 3)
        local function update(pos)
            local x = math.clamp((pos.X - bar.AbsolutePosition.X) / bar.AbsoluteSize.X, 0, 1)
            local val = minV + (maxV - minV) * x
            val = math.floor(val / step + 0.5) * step
            val = math.clamp(val, minV, maxV)
            fill.Size = UDim2.new((val - minV) / (maxV - minV), 0, 1, 0)
            l.Text = name .. ": " .. tostring(val)
            callback(val)
        end
        bar.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
                activeSlider = update
                update(input.Position)
            end
        end)
        return c
    end

    local function Crs_createFloatingDropdown(parent, name, options, default, callback)
        return createFloatingDropdown(parent, Panel, name, options, default, callback)
    end

    local function Crs_createDivider(parent)
        local line = Instance.new("Frame", parent)
        line.Size = UDim2.new(1, -6, 0, 1)
        line.BackgroundColor3 = Color3.fromRGB(80, 80, 90)
        line.BorderSizePixel = 0
        return line
    end

    local function Crs_createSectionToggle(parent, name, defaultOpen, callback)
        local container = Instance.new("Frame", parent)
        container.Size = UDim2.new(1, -6, 0, 34)
        container.BackgroundColor3 = defaultOpen and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 60)
        Instance.new("UICorner", container).CornerRadius = UDim.new(0, 8)
        local toggleBtn = Instance.new("TextButton", container)
        toggleBtn.Size = UDim2.new(1, 0, 1, 0)
        toggleBtn.BackgroundTransparency = 1
        toggleBtn.Text = (defaultOpen and "\226\150\188 " or "\226\150\182 ") .. name .. (defaultOpen and " ON" or " OFF")
        toggleBtn.TextColor3 = Color3.new(1, 1, 1)
        toggleBtn.Font = Enum.Font.GothamBold
        toggleBtn.TextSize = 13
        toggleBtn.TextXAlignment = Enum.TextXAlignment.Left
        local content = Instance.new("Frame", parent)
        content.Size = UDim2.new(1, -6, 0, 0)
        content.BackgroundTransparency = 1
        content.Visible = defaultOpen
        local contentLayout = Instance.new("UIListLayout", content)
        contentLayout.Padding = UDim.new(0, 4)
        local open = defaultOpen
        contentLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
            if content.Visible then
                content.Size = UDim2.new(1, -6, 0, contentLayout.AbsoluteContentSize.Y + 8)
            end
        end)
        toggleBtn.Activated:Connect(function()
            open = not open
            content.Visible = open
            toggleBtn.Text = (open and "\226\150\188 " or "\226\150\182 ") .. name .. (open and " ON" or " OFF")
            container.BackgroundColor3 = open and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 60)
            callback(open)
            if open then
                content.Size = UDim2.new(1, -6, 0, contentLayout.AbsoluteContentSize.Y + 8)
            end
        end)
        if defaultOpen then
            task.wait(0.05)
            content.Size = UDim2.new(1, -6, 0, contentLayout.AbsoluteContentSize.Y + 8)
        end
        return content, function() return open end
    end

  
    local function Crs_buildPreviewPanel(parent)
        local container = Instance.new("Frame", parent)
        container.Size = UDim2.new(1, -10, 1, -10)
        container.Position = UDim2.new(0, 5, 0, 5)
        container.BackgroundTransparency = 1

        local previewArea = Instance.new("Frame", container)
        previewArea.Size = UDim2.new(1, 0, 1, -30)
        previewArea.Position = UDim2.new(0, 0, 0, 28)
        previewArea.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
        previewArea.BackgroundTransparency = 0.5
        previewArea.BorderSizePixel = 0
        previewArea.Active = false
        Instance.new("UICorner", previewArea).CornerRadius = UDim.new(0, 8)

        local dummyFrame = Instance.new("Frame", previewArea)
        dummyFrame.Size = UDim2.new(1, 0, 1, 0)
        dummyFrame.BackgroundTransparency = 1
        local dummy = Instance.new("Frame", dummyFrame)
        dummy.AnchorPoint = Vector2.new(0.5, 0.5)
        dummy.Size = UDim2.new(0, 140, 0, 200)
        dummy.Position = UDim2.new(0.5, 0, 0.5, 0)
        dummy.BackgroundTransparency = 1

        local head = Instance.new("Frame", dummy)
        head.Size = UDim2.new(0, 50, 0, 50)
        head.Position = UDim2.new(0, 45, 0, 0)
        head.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        head.BorderSizePixel = 0
        Instance.new("UICorner", head).CornerRadius = UDim.new(0, 12)
        local headStroke = Instance.new("UIStroke", head)
        headStroke.Color = Color3.new(0, 0, 0)
        headStroke.Thickness = 1.5

        local body = Instance.new("Frame", dummy)
        body.Size = UDim2.new(0, 70, 0, 70)
        body.Position = UDim2.new(0, 35, 0, 50)
        body.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        body.BorderSizePixel = 0
        Instance.new("UICorner", body).CornerRadius = UDim.new(0, 2)
        local bodyStroke = Instance.new("UIStroke", body)
        bodyStroke.Color = Color3.new(0, 0, 0)
        bodyStroke.Thickness = 1.5

        local leftArm = Instance.new("Frame", dummy)
        leftArm.Size = UDim2.new(0, 35, 0, 70)
        leftArm.Position = UDim2.new(0, 0, 0, 50)
        leftArm.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        leftArm.BorderSizePixel = 0
        Instance.new("UICorner", leftArm).CornerRadius = UDim.new(0, 2)
        local leftArmStroke = Instance.new("UIStroke", leftArm)
        leftArmStroke.Color = Color3.new(0, 0, 0)
        leftArmStroke.Thickness = 1.5

        local rightArm = Instance.new("Frame", dummy)
        rightArm.Size = UDim2.new(0, 35, 0, 70)
        rightArm.Position = UDim2.new(0, 105, 0, 50)
        rightArm.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        rightArm.BorderSizePixel = 0
        Instance.new("UICorner", rightArm).CornerRadius = UDim.new(0, 2)
        local rightArmStroke = Instance.new("UIStroke", rightArm)
        rightArmStroke.Color = Color3.new(0, 0, 0)
        rightArmStroke.Thickness = 1.5

        local leftLeg = Instance.new("Frame", dummy)
        leftLeg.Size = UDim2.new(0, 35, 0, 70)
        leftLeg.Position = UDim2.new(0, 35, 0, 120)
        leftLeg.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        leftLeg.BorderSizePixel = 0
        Instance.new("UICorner", leftLeg).CornerRadius = UDim.new(0, 2)
        local leftLegStroke = Instance.new("UIStroke", leftLeg)
        leftLegStroke.Color = Color3.new(0, 0, 0)
        leftLegStroke.Thickness = 1.5

        local rightLeg = Instance.new("Frame", dummy)
        rightLeg.Size = UDim2.new(0, 35, 0, 70)
        rightLeg.Position = UDim2.new(0, 70, 0, 120)
        rightLeg.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
        rightLeg.BorderSizePixel = 0
        Instance.new("UICorner", rightLeg).CornerRadius = UDim.new(0, 2)
        local rightLegStroke = Instance.new("UIStroke", rightLeg)
        rightLegStroke.Color = Color3.new(0, 0, 0)
        rightLegStroke.Thickness = 1.5

        local previewCrossContainer = Instance.new("Frame", dummy)
        previewCrossContainer.AnchorPoint = Vector2.new(0.5, 0.5)
        previewCrossContainer.Size = UDim2.new(0, 400, 0, 400)
        previewCrossContainer.Position = UDim2.new(0, 70, 0, 85)
        previewCrossContainer.BackgroundTransparency = 1

        local pBigFill = Instance.new("Frame", previewCrossContainer); pBigFill.AnchorPoint = Vector2.new(0.5,0.5); pBigFill.Position = UDim2.new(0.5,0,0.5,0); pBigFill.Size = UDim2.new(0,CrossSettings.BigFillDia,0,CrossSettings.BigFillDia); pBigFill.BorderSizePixel = 0; pBigFill.Visible = false; pBigFill.BackgroundColor3 = Crs_makeColor(CrossSettings.BigFillH1,CrossSettings.BigFillS1,CrossSettings.BigFillV1); pBigFill.BackgroundTransparency = CrossSettings.BigFillTrans/100; Instance.new("UICorner",pBigFill).CornerRadius = UDim.new(1,0); local pBigFillGrad = Instance.new("UIGradient",pBigFill)
        local pBigStroke = Instance.new("Frame", previewCrossContainer); pBigStroke.AnchorPoint = Vector2.new(0.5,0.5); pBigStroke.Position = UDim2.new(0.5,0,0.5,0); pBigStroke.Size = UDim2.new(0,CrossSettings.BigFillDia+CrossSettings.BigStrThick*2,0,CrossSettings.BigFillDia+CrossSettings.BigStrThick*2); pBigStroke.BorderSizePixel = 0; pBigStroke.Visible = false; pBigStroke.BackgroundTransparency = 1; Instance.new("UICorner",pBigStroke).CornerRadius = UDim.new(1,0); local pBigStrokeStroke = Instance.new("UIStroke",pBigStroke); pBigStrokeStroke.Thickness = CrossSettings.BigStrThick; pBigStrokeStroke.Color = Crs_makeColor(CrossSettings.BigStrH1,CrossSettings.BigStrS1,CrossSettings.BigStrV1); pBigStrokeStroke.Transparency = CrossSettings.BigStrTrans/100
        local pSmlFill = Instance.new("Frame", previewCrossContainer); pSmlFill.AnchorPoint = Vector2.new(0.5,0.5); pSmlFill.Position = UDim2.new(0.5,0,0.5,0); pSmlFill.Size = UDim2.new(0,CrossSettings.SmlFillDia,0,CrossSettings.SmlFillDia); pSmlFill.BorderSizePixel = 0; pSmlFill.Visible = false; pSmlFill.BackgroundColor3 = Crs_makeColor(CrossSettings.SmlFillH1,CrossSettings.SmlFillS1,CrossSettings.SmlFillV1); pSmlFill.BackgroundTransparency = CrossSettings.SmlFillTrans/100; Instance.new("UICorner",pSmlFill).CornerRadius = UDim.new(1,0); local pSmlFillGrad = Instance.new("UIGradient",pSmlFill)
        local pSmlStroke = Instance.new("Frame", previewCrossContainer); pSmlStroke.AnchorPoint = Vector2.new(0.5,0.5); pSmlStroke.Position = UDim2.new(0.5,0,0.5,0); pSmlStroke.Size = UDim2.new(0,CrossSettings.SmlFillDia+CrossSettings.SmlStrThick*2,0,CrossSettings.SmlFillDia+CrossSettings.SmlStrThick*2); pSmlStroke.BorderSizePixel = 0; pSmlStroke.Visible = false; pSmlStroke.BackgroundTransparency = 1; Instance.new("UICorner",pSmlStroke).CornerRadius = UDim.new(1,0); local pSmlStrokeStroke = Instance.new("UIStroke",pSmlStroke); pSmlStrokeStroke.Thickness = CrossSettings.SmlStrThick; pSmlStrokeStroke.Color = Crs_makeColor(CrossSettings.SmlStrH1,CrossSettings.SmlStrS1,CrossSettings.SmlStrV1); pSmlStrokeStroke.Transparency = CrossSettings.SmlStrTrans/100
        local pCrossFillLines, pCrossStrokeLines = {}, {}
        for _, dir in ipairs({"Top","Bottom","Left","Right"}) do
            local stroke = Instance.new("Frame",previewCrossContainer); stroke.AnchorPoint = Vector2.new(0.5,0.5); stroke.BorderSizePixel = 0; stroke.Visible = false; stroke.BackgroundTransparency = 1; local strokeStroke = Instance.new("UIStroke",stroke); strokeStroke.Thickness = CrossSettings.CrossStrThick; strokeStroke.Color = Crs_makeColor(CrossSettings.CrossStrH1,CrossSettings.CrossStrS1,CrossSettings.CrossStrV1); strokeStroke.Transparency = CrossSettings.CrossStrTrans/100; pCrossStrokeLines[dir] = stroke
            local fill = Instance.new("Frame",previewCrossContainer); fill.AnchorPoint = Vector2.new(0.5,0.5); fill.BorderSizePixel = 0; fill.Visible = false; fill.BackgroundColor3 = Crs_makeColor(CrossSettings.CrossFillH1,CrossSettings.CrossFillS1,CrossSettings.CrossFillV1); fill.BackgroundTransparency = CrossSettings.CrossFillTrans/100; Instance.new("UIGradient",fill); pCrossFillLines[dir] = fill
        end
        local pCenterDot = Instance.new("Frame",previewCrossContainer); pCenterDot.AnchorPoint = Vector2.new(0.5,0.5); pCenterDot.Size = UDim2.new(0,CrossSettings.DotSize,0,CrossSettings.DotSize); pCenterDot.Position = UDim2.new(0.5,0,0.5,0); pCenterDot.BorderSizePixel = 0; pCenterDot.Visible = false; pCenterDot.BackgroundColor3 = Crs_makeColor(CrossSettings.DotH1,CrossSettings.DotS1,CrossSettings.DotV1); pCenterDot.BackgroundTransparency = CrossSettings.DotTrans/100; Instance.new("UICorner",pCenterDot).CornerRadius = UDim.new(1,0); local pDotGrad = Instance.new("UIGradient",pCenterDot)
        local pText = Instance.new("TextLabel",previewArea); pText.AnchorPoint = Vector2.new(0.5,0); pText.Size = UDim2.new(0,200,0,30); pText.BackgroundTransparency = 1; pText.Text = "XingHui\230\152\159\232\188\157"; pText.Font = Enum.Font.GothamBold; pText.TextXAlignment = Enum.TextXAlignment.Center; pText.TextSize = CrossSettings.TextSize; pText.Visible = false; pText.ZIndex = 5; pText.TextColor3 = Crs_makeColor(CrossSettings.TextH1,CrossSettings.TextS1,CrossSettings.TextV1); pText.TextTransparency = CrossSettings.TextTrans/100; local pTextGrad = Instance.new("UIGradient",pText)

        local function updatePreviewCrossLines()
            local sz, thick, gap = CrossSettings.Size, CrossSettings.Thickness, CrossSettings.Gap
            local fl = sz - gap
            pCrossStrokeLines.Top.Size = UDim2.new(0, thick, 0, fl); pCrossStrokeLines.Top.Position = UDim2.new(0.5, 0, 0.5, -(sz + gap)/2)
            pCrossStrokeLines.Bottom.Size = UDim2.new(0, thick, 0, fl); pCrossStrokeLines.Bottom.Position = UDim2.new(0.5, 0, 0.5, (sz + gap)/2)
            pCrossStrokeLines.Left.Size = UDim2.new(0, fl, 0, thick); pCrossStrokeLines.Left.Position = UDim2.new(0.5, -(sz + gap)/2, 0.5, 0)
            pCrossStrokeLines.Right.Size = UDim2.new(0, fl, 0, thick); pCrossStrokeLines.Right.Position = UDim2.new(0.5, (sz + gap)/2, 0.5, 0)
            pCrossFillLines.Top.Size = UDim2.new(0, thick, 0, fl); pCrossFillLines.Top.Position = UDim2.new(0.5, 0, 0.5, -(sz + gap)/2)
            pCrossFillLines.Bottom.Size = UDim2.new(0, thick, 0, fl); pCrossFillLines.Bottom.Position = UDim2.new(0.5, 0, 0.5, (sz + gap)/2)
            pCrossFillLines.Left.Size = UDim2.new(0, fl, 0, thick); pCrossFillLines.Left.Position = UDim2.new(0.5, -(sz + gap)/2, 0.5, 0)
            pCrossFillLines.Right.Size = UDim2.new(0, fl, 0, thick); pCrossFillLines.Right.Position = UDim2.new(0.5, (sz + gap)/2, 0.5, 0)
        end

        local function updatePreviewCross()
            local s = CrossSettings
            if s.TextVis then
                pText.Visible = true; pText.TextSize = s.TextSize
                local cap = previewCrossContainer.AbsolutePosition; local csz = previewCrossContainer.AbsoluteSize
                local cx = cap.X + csz.X/2; local cy = cap.Y + csz.Y/2
                local aap = previewArea.AbsolutePosition
                pText.Position = UDim2.new(0, cx - aap.X + s.TextX, 0, cy - aap.Y + s.TextY)
                if s.TextRain then local hue = (tick() * s.TextRainSpd * 60) % 360 / 360; Crs_applyGradient(pTextGrad, Color3.fromHSV(hue, 1, 1), Color3.fromHSV((hue + 0.3) % 1, 1, 1), s.TextDir); pText.TextColor3 = Color3.fromHSV(hue, 1, 1)
                else Crs_applyGradient(pTextGrad, Crs_makeColor(s.TextH1, s.TextS1, s.TextV1), Crs_makeColor(s.TextH2, s.TextS2, s.TextV2), s.TextDir); pText.TextColor3 = Crs_makeColor(s.TextH1, s.TextS1, s.TextV1) end
                pText.TextTransparency = s.TextTrans/100
            else pText.Visible = false end
            if not s.Enabled then
                pBigFill.Visible, pBigStroke.Visible, pSmlFill.Visible, pSmlStroke.Visible = false, false, false, false
                for _, v in pairs(pCrossStrokeLines) do v.Visible = false end
                for _, v in pairs(pCrossFillLines) do v.Visible = false end
                pCenterDot.Visible = false; previewCrossContainer.Rotation = 0; return
            end
            pBigFill.Visible, pBigStroke.Visible = s.BigFillVis, s.BigStrVis
            pSmlFill.Visible, pSmlStroke.Visible = s.SmlFillVis, s.SmlStrVis
            for _, v in pairs(pCrossStrokeLines) do v.Visible = s.CrossStrVis end
            for _, v in pairs(pCrossFillLines) do v.Visible = s.CrossFillVis end
            pCenterDot.Visible = s.DotVis; previewCrossContainer.Rotation = s.RotEnabled and CrossContainer.Rotation or 0
            if s.BigFillVis then
                pBigFill.Size = UDim2.new(0, s.BigFillDia, 0, s.BigFillDia)
                if s.BigFillRain then local hue = (tick() * s.BigFillRainSpd * 60) % 360 / 360; Crs_applyGradient(pBigFillGrad, Color3.fromHSV(hue, 1, 1), Color3.fromHSV((hue + 0.3) % 1, 1, 1), s.BigFillDir); pBigFill.BackgroundColor3 = Color3.fromHSV(hue, 1, 1)
                else Crs_applyGradient(pBigFillGrad, Crs_makeColor(s.BigFillH1, s.BigFillS1, s.BigFillV1), Crs_makeColor(s.BigFillH2, s.BigFillS2, s.BigFillV2), s.BigFillDir); pBigFill.BackgroundColor3 = Crs_makeColor(s.BigFillH1, s.BigFillS1, s.BigFillV1) end
                pBigFill.BackgroundTransparency = s.BigFillTrans/100
            end
            if s.BigStrVis then
                pBigStroke.Size = UDim2.new(0, s.BigFillDia + s.BigStrThick*2, 0, s.BigFillDia + s.BigStrThick*2)
                pBigStrokeStroke.Thickness = s.BigStrThick
                if s.BigStrRain then pBigStrokeStroke.Color = Color3.fromHSV((tick() * s.BigStrRainSpd * 60) % 360 / 360, 1, 1)
                else pBigStrokeStroke.Color = Crs_makeColor(s.BigStrH1, s.BigStrS1, s.BigStrV1) end
                pBigStrokeStroke.Transparency = s.BigStrTrans/100
            end
            if s.SmlFillVis then
                pSmlFill.Size = UDim2.new(0, s.SmlFillDia, 0, s.SmlFillDia)
                if s.SmlFillRain then local hue = (tick() * s.SmlFillRainSpd * 60) % 360 / 360; Crs_applyGradient(pSmlFillGrad, Color3.fromHSV(hue, 1, 1), Color3.fromHSV((hue + 0.3) % 1, 1, 1), s.SmlFillDir); pSmlFill.BackgroundColor3 = Color3.fromHSV(hue, 1, 1)
                else Crs_applyGradient(pSmlFillGrad, Crs_makeColor(s.SmlFillH1, s.SmlFillS1, s.SmlFillV1), Crs_makeColor(s.SmlFillH2, s.SmlFillS2, s.SmlFillV2), s.SmlFillDir); pSmlFill.BackgroundColor3 = Crs_makeColor(s.SmlFillH1, s.SmlFillS1, s.SmlFillV1) end
                pSmlFill.BackgroundTransparency = s.SmlFillTrans/100
            end
            if s.SmlStrVis then
                pSmlStroke.Size = UDim2.new(0, s.SmlFillDia + s.SmlStrThick*2, 0, s.SmlFillDia + s.SmlStrThick*2)
                pSmlStrokeStroke.Thickness = s.SmlStrThick
                if s.SmlStrRain then pSmlStrokeStroke.Color = Color3.fromHSV((tick() * s.SmlStrRainSpd * 60) % 360 / 360, 1, 1)
                else pSmlStrokeStroke.Color = Crs_makeColor(s.SmlStrH1, s.SmlStrS1, s.SmlStrV1) end
                pSmlStrokeStroke.Transparency = s.SmlStrTrans/100
            end
            if s.DotVis then
                pCenterDot.Size = UDim2.new(0, s.DotSize, 0, s.DotSize)
                if s.DotRain then local hue = (tick() * s.DotRainSpd * 60) % 360 / 360; Crs_applyGradient(pDotGrad, Color3.fromHSV(hue, 1, 1), Color3.fromHSV((hue + 0.3) % 1, 1, 1), s.DotDir); pCenterDot.BackgroundColor3 = Color3.fromHSV(hue, 1, 1)
                else Crs_applyGradient(pDotGrad, Crs_makeColor(s.DotH1, s.DotS1, s.DotV1), Crs_makeColor(s.DotH2, s.DotS2, s.DotV2), s.DotDir); pCenterDot.BackgroundColor3 = Crs_makeColor(s.DotH1, s.DotS1, s.DotV1) end
                pCenterDot.BackgroundTransparency = s.DotTrans/100
            end
            for _, fill in pairs(pCrossFillLines) do
                if s.CrossFillVis then
                    if s.CrossFillRain then local hue = (tick() * s.CrossFillRainSpd * 60) % 360 / 360; Crs_applyGradient(fill:FindFirstChildOfClass("UIGradient"), Color3.fromHSV(hue, 1, 1), Color3.fromHSV((hue + 0.3) % 1, 1, 1), s.CrossFillDir); fill.BackgroundColor3 = Color3.fromHSV(hue, 1, 1)
                    else Crs_applyGradient(fill:FindFirstChildOfClass("UIGradient"), Crs_makeColor(s.CrossFillH1, s.CrossFillS1, s.CrossFillV1), Crs_makeColor(s.CrossFillH2, s.CrossFillS2, s.CrossFillV2), s.CrossFillDir); fill.BackgroundColor3 = Crs_makeColor(s.CrossFillH1, s.CrossFillS1, s.CrossFillV1) end
                    fill.BackgroundTransparency = s.CrossFillTrans/100
                end
            end
            for _, stroke in pairs(pCrossStrokeLines) do
                if s.CrossStrVis then
                    local strokeStroke = stroke:FindFirstChildOfClass("UIStroke")
                    if strokeStroke then
                        strokeStroke.Thickness = s.CrossStrThick
                        if s.CrossStrRain then strokeStroke.Color = Color3.fromHSV((tick() * s.CrossStrRainSpd * 60) % 360 / 360, 1, 1)
                        else strokeStroke.Color = Crs_makeColor(s.CrossStrH1, s.CrossStrS1, s.CrossStrV1) end
                        strokeStroke.Transparency = s.CrossStrTrans/100
                    end
                end
            end
            updatePreviewCrossLines()
        end
        previewUpdater = updatePreviewCross
        return container
    end

    local rightColumn = Instance.new("Frame", crossContent)
    rightColumn.Size = UDim2.new(0.4, -6, 1, 0)
    rightColumn.Position = UDim2.new(0.6, 3, 0, 0)
    rightColumn.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    rightColumn.BackgroundTransparency = 0.5
    rightColumn.BorderSizePixel = 0
    Instance.new("UICorner", rightColumn).CornerRadius = UDim.new(0, 10)
    Crs_buildPreviewPanel(rightColumn)

    -- \230\186\150\230\152\159\232\168\173\229\174\154\229\141\128\230\174\181 (\229\133\167\233\131\168\228\184\141\230\148\190\229\164\167\239\188\140\231\182\173\230\140\129\229\142\159\230\168\163)
    local generalSection = Crs_createSectionToggle(mainContentContainer, "\233\128\154\231\148\168\232\168\173\229\174\154", false, function(b) end)
    Crs_createSlider(generalSection, "\230\186\150\230\152\159\229\164\167\229\176\143", 10, 50, CrossSettings.Size, 1, function(v) CrossSettings.Size = v; Crs_updateCrossLines() end)
    Crs_createSlider(generalSection, "\231\183\154\230\162\157\231\178\151\231\180\176", 1, 15, CrossSettings.Thickness, 1, function(v) CrossSettings.Thickness = v; Crs_updateCrossLines() end)
    Crs_createSlider(generalSection, "\228\184\173\229\191\131\233\150\147\233\154\153", 0, 30, CrossSettings.Gap, 1, function(v) CrossSettings.Gap = v; Crs_updateCrossLines() end)
    Crs_createToggle(generalSection, "\230\151\139\232\189\137", CrossSettings.RotEnabled, function(b) CrossSettings.RotEnabled = b end)
    Crs_createSlider(generalSection, "\230\151\139\232\189\137\233\128\159\229\186\166", 0, 180, CrossSettings.RotSpeed, 5, function(v) CrossSettings.RotSpeed = v end)
    Crs_createDivider(mainContentContainer)

    local bigFillContent = Crs_createSectionToggle(mainContentContainer, "\229\164\167\229\156\147\229\161\171\230\187\191", CrossSettings.BigFillVis, function(b) CrossSettings.BigFillVis = b end)
    Crs_createSlider(bigFillContent, "\231\155\180\229\190\145", 10, 200, CrossSettings.BigFillDia, 1, function(v) CrossSettings.BigFillDia = v end)
    Crs_createFloatingDropdown(bigFillContent, "\230\188\184\229\177\164\230\168\161\229\188\143", {"\231\183\154\230\128\1670\194\176", "\231\183\154\230\128\16745\194\176", "\231\183\154\230\128\16790\194\176", "\231\183\154\230\128\167135\194\176", "\229\176\141\232\167\146\230\188\184\229\177\164", "\229\189\169\232\153\185\230\188\184\229\177\164"}, CrossSettings.BigFillDir, function(v) CrossSettings.BigFillDir = v end)
    Crs_createSlider(bigFillContent, "\232\137\178\231\155\1841", 0, 360, CrossSettings.BigFillH1, 1, function(v) CrossSettings.BigFillH1 = v end)
    Crs_createSlider(bigFillContent, "\233\163\189\229\146\1401", 0, 100, CrossSettings.BigFillS1, 1, function(v) CrossSettings.BigFillS1 = v end)
    Crs_createSlider(bigFillContent, "\230\152\142\229\186\1661", 0, 100, CrossSettings.BigFillV1, 1, function(v) CrossSettings.BigFillV1 = v end)
    Crs_createSlider(bigFillContent, "\232\137\178\231\155\1842", 0, 360, CrossSettings.BigFillH2, 1, function(v) CrossSettings.BigFillH2 = v end)
    Crs_createSlider(bigFillContent, "\233\163\189\229\146\1402", 0, 100, CrossSettings.BigFillS2, 1, function(v) CrossSettings.BigFillS2 = v end)
    Crs_createSlider(bigFillContent, "\230\152\142\229\186\1662", 0, 100, CrossSettings.BigFillV2, 1, function(v) CrossSettings.BigFillV2 = v end)
    Crs_createSlider(bigFillContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.BigFillTrans, 5, function(v) CrossSettings.BigFillTrans = v end)
    Crs_createToggle(bigFillContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.BigFillRain, function(b) CrossSettings.BigFillRain = b end)
    Crs_createSlider(bigFillContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.BigFillRainSpd, 0.1, function(v) CrossSettings.BigFillRainSpd = v end)

    local bigStrContent = Crs_createSectionToggle(mainContentContainer, "\229\164\167\229\156\147\233\130\138\230\161\134", CrossSettings.BigStrVis, function(b) CrossSettings.BigStrVis = b end)
    Crs_createSlider(bigStrContent, "\231\178\151\231\180\176", 0.5, 8, CrossSettings.BigStrThick, 0.5, function(v) CrossSettings.BigStrThick = v end)
    Crs_createSlider(bigStrContent, "\232\137\178\231\155\184", 0, 360, CrossSettings.BigStrH1, 1, function(v) CrossSettings.BigStrH1 = v end)
    Crs_createSlider(bigStrContent, "\233\163\189\229\146\140", 0, 100, CrossSettings.BigStrS1, 1, function(v) CrossSettings.BigStrS1 = v end)
    Crs_createSlider(bigStrContent, "\230\152\142\229\186\166", 0, 100, CrossSettings.BigStrV1, 1, function(v) CrossSettings.BigStrV1 = v end)
    Crs_createSlider(bigStrContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.BigStrTrans, 5, function(v) CrossSettings.BigStrTrans = v end)
    Crs_createToggle(bigStrContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.BigStrRain, function(b) CrossSettings.BigStrRain = b end)
    Crs_createSlider(bigStrContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.BigStrRainSpd, 0.1, function(v) CrossSettings.BigStrRainSpd = v end)

    local smlFillContent = Crs_createSectionToggle(mainContentContainer, "\229\176\143\229\156\147\229\161\171\230\187\191", CrossSettings.SmlFillVis, function(b) CrossSettings.SmlFillVis = b end)
    Crs_createSlider(smlFillContent, "\231\155\180\229\190\145", 2, 80, CrossSettings.SmlFillDia, 1, function(v) CrossSettings.SmlFillDia = v end)
    Crs_createFloatingDropdown(smlFillContent, "\230\188\184\229\177\164\230\168\161\229\188\143", {"\231\183\154\230\128\1670\194\176", "\231\183\154\230\128\16745\194\176", "\231\183\154\230\128\16790\194\176", "\231\183\154\230\128\167135\194\176", "\229\176\141\232\167\146\230\188\184\229\177\164", "\229\189\169\232\153\185\230\188\184\229\177\164"}, CrossSettings.SmlFillDir, function(v) CrossSettings.SmlFillDir = v end)
    Crs_createSlider(smlFillContent, "\232\137\178\231\155\1841", 0, 360, CrossSettings.SmlFillH1, 1, function(v) CrossSettings.SmlFillH1 = v end)
    Crs_createSlider(smlFillContent, "\233\163\189\229\146\1401", 0, 100, CrossSettings.SmlFillS1, 1, function(v) CrossSettings.SmlFillS1 = v end)
    Crs_createSlider(smlFillContent, "\230\152\142\229\186\1661", 0, 100, CrossSettings.SmlFillV1, 1, function(v) CrossSettings.SmlFillV1 = v end)
    Crs_createSlider(smlFillContent, "\232\137\178\231\155\1842", 0, 360, CrossSettings.SmlFillH2, 1, function(v) CrossSettings.SmlFillH2 = v end)
    Crs_createSlider(smlFillContent, "\233\163\189\229\146\1402", 0, 100, CrossSettings.SmlFillS2, 1, function(v) CrossSettings.SmlFillS2 = v end)
    Crs_createSlider(smlFillContent, "\230\152\142\229\186\1662", 0, 100, CrossSettings.SmlFillV2, 1, function(v) CrossSettings.SmlFillV2 = v end)
    Crs_createSlider(smlFillContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.SmlFillTrans, 5, function(v) CrossSettings.SmlFillTrans = v end)
    Crs_createToggle(smlFillContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.SmlFillRain, function(b) CrossSettings.SmlFillRain = b end)
    Crs_createSlider(smlFillContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.SmlFillRainSpd, 0.1, function(v) CrossSettings.SmlFillRainSpd = v end)

    local smlStrContent = Crs_createSectionToggle(mainContentContainer, "\229\176\143\229\156\147\233\130\138\230\161\134", CrossSettings.SmlStrVis, function(b) CrossSettings.SmlStrVis = b end)
    Crs_createSlider(smlStrContent, "\231\178\151\231\180\176", 0.5, 6, CrossSettings.SmlStrThick, 0.5, function(v) CrossSettings.SmlStrThick = v end)
    Crs_createSlider(smlStrContent, "\232\137\178\231\155\184", 0, 360, CrossSettings.SmlStrH1, 1, function(v) CrossSettings.SmlStrH1 = v end)
    Crs_createSlider(smlStrContent, "\233\163\189\229\146\140", 0, 100, CrossSettings.SmlStrS1, 1, function(v) CrossSettings.SmlStrS1 = v end)
    Crs_createSlider(smlStrContent, "\230\152\142\229\186\166", 0, 100, CrossSettings.SmlStrV1, 1, function(v) CrossSettings.SmlStrV1 = v end)
    Crs_createSlider(smlStrContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.SmlStrTrans, 5, function(v) CrossSettings.SmlStrTrans = v end)
    Crs_createToggle(smlStrContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.SmlStrRain, function(b) CrossSettings.SmlStrRain = b end)
    Crs_createSlider(smlStrContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.SmlStrRainSpd, 0.1, function(v) CrossSettings.SmlStrRainSpd = v end)

    local crossFillContent = Crs_createSectionToggle(mainContentContainer, "\229\141\129\229\173\151\229\161\171\230\187\191", CrossSettings.CrossFillVis, function(b) CrossSettings.CrossFillVis = b end)
    Crs_createFloatingDropdown(crossFillContent, "\230\188\184\229\177\164\230\168\161\229\188\143", {"\231\183\154\230\128\1670\194\176", "\231\183\154\230\128\16745\194\176", "\231\183\154\230\128\16790\194\176", "\231\183\154\230\128\167135\194\176", "\229\176\141\232\167\146\230\188\184\229\177\164", "\229\189\169\232\153\185\230\188\184\229\177\164"}, CrossSettings.CrossFillDir, function(v) CrossSettings.CrossFillDir = v end)
    Crs_createSlider(crossFillContent, "\232\137\178\231\155\1841", 0, 360, CrossSettings.CrossFillH1, 1, function(v) CrossSettings.CrossFillH1 = v end)
    Crs_createSlider(crossFillContent, "\233\163\189\229\146\1401", 0, 100, CrossSettings.CrossFillS1, 1, function(v) CrossSettings.CrossFillS1 = v end)
    Crs_createSlider(crossFillContent, "\230\152\142\229\186\1661", 0, 100, CrossSettings.CrossFillV1, 1, function(v) CrossSettings.CrossFillV1 = v end)
    Crs_createSlider(crossFillContent, "\232\137\178\231\155\1842", 0, 360, CrossSettings.CrossFillH2, 1, function(v) CrossSettings.CrossFillH2 = v end)
    Crs_createSlider(crossFillContent, "\233\163\189\229\146\1402", 0, 100, CrossSettings.CrossFillS2, 1, function(v) CrossSettings.CrossFillS2 = v end)
    Crs_createSlider(crossFillContent, "\230\152\142\229\186\1662", 0, 100, CrossSettings.CrossFillV2, 1, function(v) CrossSettings.CrossFillV2 = v end)
    Crs_createSlider(crossFillContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.CrossFillTrans, 5, function(v) CrossSettings.CrossFillTrans = v end)
    Crs_createToggle(crossFillContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.CrossFillRain, function(b) CrossSettings.CrossFillRain = b end)
    Crs_createSlider(crossFillContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.CrossFillRainSpd, 0.1, function(v) CrossSettings.CrossFillRainSpd = v end)
    Crs_createDivider(crossFillContent)
    Crs_createToggle(crossFillContent, "\229\141\129\229\173\151\233\130\138\230\161\134", CrossSettings.CrossStrVis, function(b) CrossSettings.CrossStrVis = b end)
    Crs_createSlider(crossFillContent, "\233\130\138\230\161\134\231\178\151\231\180\176", 0, 6, CrossSettings.CrossStrThick, 1, function(v) CrossSettings.CrossStrThick = v; Crs_updateCrossLines() end)
    Crs_createFloatingDropdown(crossFillContent, "\233\130\138\230\161\134\230\188\184\229\177\164", {"\231\183\154\230\128\1670\194\176", "\231\183\154\230\128\16745\194\176", "\231\183\154\230\128\16790\194\176", "\231\183\154\230\128\167135\194\176", "\229\176\141\232\167\146\230\188\184\229\177\164", "\229\189\169\232\153\185\230\188\184\229\177\164"}, CrossSettings.CrossStrDir, function(v) CrossSettings.CrossStrDir = v end)
    Crs_createSlider(crossFillContent, "\233\130\138\230\161\134\232\137\178\231\155\184", 0, 360, CrossSettings.CrossStrH1, 1, function(v) CrossSettings.CrossStrH1 = v end)
    Crs_createSlider(crossFillContent, "\233\130\138\230\161\134\233\163\189\229\146\140", 0, 100, CrossSettings.CrossStrS1, 1, function(v) CrossSettings.CrossStrS1 = v end)
    Crs_createSlider(crossFillContent, "\233\130\138\230\161\134\230\152\142\229\186\166", 0, 100, CrossSettings.CrossStrV1, 1, function(v) CrossSettings.CrossStrV1 = v end)
    Crs_createSlider(crossFillContent, "\233\130\138\230\161\134\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.CrossStrTrans, 5, function(v) CrossSettings.CrossStrTrans = v end)
    Crs_createToggle(crossFillContent, "\233\130\138\230\161\134\229\189\169\232\153\185", CrossSettings.CrossStrRain, function(b) CrossSettings.CrossStrRain = b end)
    Crs_createSlider(crossFillContent, "\233\130\138\230\161\134\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.CrossStrRainSpd, 0.1, function(v) CrossSettings.CrossStrRainSpd = v end)

    local dotContent = Crs_createSectionToggle(mainContentContainer, "\228\184\173\229\191\131\233\187\158", CrossSettings.DotVis, function(b) CrossSettings.DotVis = b end)
    Crs_createSlider(dotContent, "\229\164\167\229\176\143", 2, 12, CrossSettings.DotSize, 1, function(v) CrossSettings.DotSize = v end)
    Crs_createFloatingDropdown(dotContent, "\230\188\184\229\177\164\230\168\161\229\188\143", {"\231\183\154\230\128\1670\194\176", "\231\183\154\230\128\16745\194\176", "\231\183\154\230\128\16790\194\176", "\231\183\154\230\128\167135\194\176", "\229\176\141\232\167\146\230\188\184\229\177\164", "\229\189\169\232\153\185\230\188\184\229\177\164"}, CrossSettings.DotDir, function(v) CrossSettings.DotDir = v end)
    Crs_createSlider(dotContent, "\232\137\178\231\155\1841", 0, 360, CrossSettings.DotH1, 1, function(v) CrossSettings.DotH1 = v end)
    Crs_createSlider(dotContent, "\233\163\189\229\146\1401", 0, 100, CrossSettings.DotS1, 1, function(v) CrossSettings.DotS1 = v end)
    Crs_createSlider(dotContent, "\230\152\142\229\186\1661", 0, 100, CrossSettings.DotV1, 1, function(v) CrossSettings.DotV1 = v end)
    Crs_createSlider(dotContent, "\232\137\178\231\155\1842", 0, 360, CrossSettings.DotH2, 1, function(v) CrossSettings.DotH2 = v end)
    Crs_createSlider(dotContent, "\233\163\189\229\146\1402", 0, 100, CrossSettings.DotS2, 1, function(v) CrossSettings.DotS2 = v end)
    Crs_createSlider(dotContent, "\230\152\142\229\186\1662", 0, 100, CrossSettings.DotV2, 1, function(v) CrossSettings.DotV2 = v end)
    Crs_createSlider(dotContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.DotTrans, 5, function(v) CrossSettings.DotTrans = v end)
    Crs_createToggle(dotContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.DotRain, function(b) CrossSettings.DotRain = b end)
    Crs_createSlider(dotContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.DotRainSpd, 0.1, function(v) CrossSettings.DotRainSpd = v end)

    local textContent = Crs_createSectionToggle(mainContentContainer, "\230\150\135\229\173\151\230\168\153\231\177\164", CrossSettings.TextVis, function(b) CrossSettings.TextVis = b end)
    Crs_createSlider(textContent, "\229\164\167\229\176\143", 10, 30, CrossSettings.TextSize, 1, function(v) CrossSettings.TextSize = v end)
    Crs_createFloatingDropdown(textContent, "\230\188\184\229\177\164\230\168\161\229\188\143", {"\231\183\154\230\128\1670\194\176", "\231\183\154\230\128\16745\194\176", "\231\183\154\230\128\16790\194\176", "\231\183\154\230\128\167135\194\176", "\229\176\141\232\167\146\230\188\184\229\177\164", "\229\189\169\232\153\185\230\188\184\229\177\164"}, CrossSettings.TextDir, function(v) CrossSettings.TextDir = v end)
    Crs_createSlider(textContent, "\232\137\178\231\155\1841", 0, 360, CrossSettings.TextH1, 1, function(v) CrossSettings.TextH1 = v end)
    Crs_createSlider(textContent, "\233\163\189\229\146\1401", 0, 100, CrossSettings.TextS1, 1, function(v) CrossSettings.TextS1 = v end)
    Crs_createSlider(textContent, "\230\152\142\229\186\1661", 0, 100, CrossSettings.TextV1, 1, function(v) CrossSettings.TextV1 = v end)
    Crs_createSlider(textContent, "\232\137\178\231\155\1842", 0, 360, CrossSettings.TextH2, 1, function(v) CrossSettings.TextH2 = v end)
    Crs_createSlider(textContent, "\233\163\189\229\146\1402", 0, 100, CrossSettings.TextS2, 1, function(v) CrossSettings.TextS2 = v end)
    Crs_createSlider(textContent, "\230\152\142\229\186\1662", 0, 100, CrossSettings.TextV2, 1, function(v) CrossSettings.TextV2 = v end)
    Crs_createSlider(textContent, "\233\128\143\230\152\142\229\186\166", 0, 100, CrossSettings.TextTrans, 5, function(v) CrossSettings.TextTrans = v end)
    Crs_createSlider(textContent, "\230\176\180\229\185\179\229\129\143\231\167\187", -150, 150, CrossSettings.TextX, 1, function(v) CrossSettings.TextX = v end)
    Crs_createSlider(textContent, "\229\158\130\231\155\180\229\129\143\231\167\187", -100, 200, CrossSettings.TextY, 1, function(v) CrossSettings.TextY = v end)
    Crs_createToggle(textContent, "\229\189\169\232\153\185\230\168\161\229\188\143", CrossSettings.TextRain, function(b) CrossSettings.TextRain = b end)
    Crs_createSlider(textContent, "\229\189\169\232\153\185\233\128\159\229\186\166", 0.1, 5, CrossSettings.TextRainSpd, 0.1, function(v) CrossSettings.TextRainSpd = v end)
end

-- ============================================
-- \232\168\173\229\174\154\229\136\134\233\160\129 (\230\152\159\232\188\157hub_overkill\239\188\140\229\143\170\231\174\161\231\144\134\230\186\150\230\152\159)
-- ============================================
local function BuildConfigTab()
    local configContent = TabContents[4]
    for _, child in ipairs(configContent:GetChildren()) do child:Destroy() end

    local configDir = "\230\152\159\232\188\157hub_overkill"
    pcall(function() makefolder(configDir) end)

    local cfgScroll = Instance.new("ScrollingFrame", configContent)
    cfgScroll.Size = UDim2.new(1, 0, 1, 0); cfgScroll.BackgroundTransparency = 1; cfgScroll.ScrollBarThickness = 6; cfgScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    local cfgInner = Instance.new("Frame", cfgScroll); cfgInner.Size = UDim2.new(1, 0, 0, 0); cfgInner.BackgroundTransparency = 1
    local cfgLayout = Instance.new("UIListLayout", cfgInner); cfgLayout.SortOrder = Enum.SortOrder.LayoutOrder; cfgLayout.Padding = UDim.new(0, 6)
    cfgLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function() cfgScroll.CanvasSize = UDim2.new(0, 0, 0, cfgLayout.AbsoluteContentSize.Y + 10) end)

    local lblTitle = Instance.new("TextLabel", cfgInner); lblTitle.Size = UDim2.new(1, -20, 0, 28); lblTitle.BackgroundTransparency = 1; lblTitle.Text = "\232\168\173\229\174\154\230\170\148\231\174\161\231\144\134 (\229\131\133\230\186\150\230\152\159)"; lblTitle.TextColor3 = Color3.new(1, 1, 1); lblTitle.Font = Enum.Font.GothamBold; lblTitle.TextSize = 15; lblTitle.TextXAlignment = Enum.TextXAlignment.Left; lblTitle.LayoutOrder = 1

    local txtName = Instance.new("TextBox", cfgInner); txtName.Size = UDim2.new(1, -20, 0, 34); txtName.PlaceholderText = "\232\188\184\229\133\165\232\168\173\229\174\154\230\170\148\229\144\141\231\168\177"; txtName.Text = ""; txtName.BackgroundColor3 = Color3.fromRGB(40, 40, 48); txtName.TextColor3 = Color3.new(1, 1, 1); txtName.Font = Enum.Font.Gotham; txtName.TextSize = 14
    Instance.new("UICorner", txtName).CornerRadius = UDim.new(0, 8); txtName.LayoutOrder = 2

    local btnSelect = Instance.new("TextButton", cfgInner); btnSelect.Size = UDim2.new(1, -20, 0, 34); btnSelect.BackgroundColor3 = Color3.fromRGB(100, 60, 160); btnSelect.Text = "\233\129\184\230\147\135\232\168\173\229\174\154\230\170\148 \226\172\134\239\184\143"; btnSelect.TextColor3 = Color3.new(1, 1, 1); btnSelect.Font = Enum.Font.GothamBold; btnSelect.TextSize = 14
    Instance.new("UICorner", btnSelect).CornerRadius = UDim.new(0, 8); btnSelect.LayoutOrder = 3

    local listContainer = Instance.new("Frame", cfgInner); listContainer.Size = UDim2.new(1, -20, 0, 0); listContainer.BackgroundColor3 = Color3.fromRGB(20, 20, 25); listContainer.BorderSizePixel = 0; listContainer.ClipsDescendants = true; listContainer.Visible = false
    Instance.new("UICorner", listContainer).CornerRadius = UDim.new(0, 8); listContainer.LayoutOrder = 4

    local listScroll = Instance.new("ScrollingFrame", listContainer); listScroll.Size = UDim2.new(1, -12, 1, -8); listScroll.Position = UDim2.new(0, 6, 0, 4); listScroll.BackgroundTransparency = 1; listScroll.ScrollBarThickness = 4; listScroll.CanvasSize = UDim2.new(0, 0, 0, 0)
    local listLayout = Instance.new("UIListLayout", listScroll); listLayout.Padding = UDim.new(0, 3)

    local selectedBtn = nil
    local selectedConfigName = nil

    local function refreshList()
        for _, child in ipairs(listScroll:GetChildren()) do if child:IsA("TextButton") then child:Destroy() end end
        selectedBtn = nil; selectedConfigName = nil
        local files = {}
        pcall(function() files = listfiles(configDir) end)
        local y = 0
        for _, fullPath in ipairs(files) do
            if fullPath:sub(-5) == ".json" then
                local name = fullPath:match("([^\\/]+)%.json$")
                if name then
                    local btn = Instance.new("TextButton", listScroll); btn.Size = UDim2.new(1, -6, 0, 30); btn.Position = UDim2.new(0, 3, 0, y); btn.BackgroundColor3 = Color3.fromRGB(50, 50, 60); btn.Text = name; btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = Enum.Font.Gotham; btn.TextSize = 14
                    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 6)
                    btn.Activated:Connect(function()
                        if selectedBtn then selectedBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 60) end
                        btn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
                        selectedBtn = btn
                        selectedConfigName = name
                    end)
                    y = y + 33
                end
            end
        end
        listScroll.CanvasSize = UDim2.new(0, 0, 0, y)
        local listHeight = math.min(y + 10, 200); listContainer.Size = UDim2.new(1, -20, 0, listHeight)
        listScroll.ScrollBarThickness = (y > 180) and 4 or 0
    end

    local isExpanded = false
    btnSelect.Activated:Connect(function()
        isExpanded = not isExpanded
        if isExpanded then refreshList(); listContainer.Visible = true; btnSelect.Text = "\233\129\184\230\147\135\232\168\173\229\174\154\230\170\148 \226\172\135\239\184\143"
        else listContainer.Visible = false; listContainer.Size = UDim2.new(1, -20, 0, 0); btnSelect.Text = "\233\129\184\230\147\135\232\168\173\229\174\154\230\170\148 \226\172\134\239\184\143" end
    end)

    local function makeBtn(text, color, order, callback)
        local btn = Instance.new("TextButton", cfgInner); btn.Size = UDim2.new(1, -20, 0, 34); btn.BackgroundColor3 = color; btn.Text = text; btn.TextColor3 = Color3.new(1, 1, 1); btn.Font = Enum.Font.GothamBold; btn.TextSize = 14
        Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8); btn.LayoutOrder = order; btn.Activated:Connect(callback)
        return btn
    end

    local statusLabel = Instance.new("TextLabel", cfgInner); statusLabel.Size = UDim2.new(1, -20, 0, 20); statusLabel.BackgroundTransparency = 1; statusLabel.Text = ""; statusLabel.TextColor3 = Color3.new(1, 1, 1); statusLabel.Font = Enum.Font.Gotham; statusLabel.TextSize = 12; statusLabel.TextXAlignment = Enum.TextXAlignment.Left; statusLabel.LayoutOrder = 9

    makeBtn("\229\187\186\231\171\139", Color3.fromRGB(0, 150, 0), 5, function()
        local name = txtName.Text:match("^%s*(.-)%s*$")
        if name == "" then statusLabel.Text = "\226\157\140 \229\144\141\231\168\177\228\184\141\229\143\175\231\130\186\231\169\186" return end
        local path = configDir .. "/" .. name .. ".json"
        if isfile(path) then statusLabel.Text = "\226\154\160\239\184\143 \232\168\173\229\174\154\230\170\148\229\183\178\229\173\152\229\156\168" return end
        pcall(function() makefolder(configDir) end)
        local cfg = { CrossSettings = CrossSettings }
        local ok, err = pcall(function() writefile(path, HttpService:JSONEncode(cfg)) end)
        statusLabel.Text = ok and "\226\156\133 \229\183\178\229\187\186\231\171\139\239\188\154" .. name or "\226\157\140 \229\164\177\230\149\151\239\188\154" .. tostring(err)
        if isExpanded then refreshList() end
    end)

    makeBtn("\229\136\170\233\153\164", Color3.fromRGB(200, 50, 50), 6, function()
        if not selectedConfigName then statusLabel.Text = "\226\157\140 \232\171\139\229\133\136\229\156\168\230\184\133\229\150\174\228\184\173\233\129\184\229\143\150\232\168\173\229\174\154\230\170\148" return end
        local path = configDir .. "/" .. selectedConfigName .. ".json"
        local ok, err = pcall(function() delfile(path) end)
        if ok then statusLabel.Text = "\240\159\151\145 \229\183\178\229\136\170\233\153\164\239\188\154" .. selectedConfigName; if isExpanded then refreshList() end
        else statusLabel.Text = "\226\157\140 \229\136\170\233\153\164\229\164\177\230\149\151" end
    end)

    makeBtn("\232\188\137\229\133\165", Color3.fromRGB(255, 140, 0), 7, function()
        if not selectedConfigName then statusLabel.Text = "\226\157\140 \232\171\139\229\133\136\229\156\168\230\184\133\229\150\174\228\184\173\233\129\184\229\143\150\232\168\173\229\174\154\230\170\148" return end
        local success, data = pcall(function() return readfile(configDir .. "/" .. selectedConfigName .. ".json") end)
        if not success or not data then statusLabel.Text = "\226\157\140 \232\174\128\229\143\150\229\164\177\230\149\151" return end
        local cfg = HttpService:JSONDecode(data)
        if type(cfg) == "table" and cfg.CrossSettings then
            local default = GetDefaultCrossSettings()
            for k, _ in pairs(CrossSettings) do
                CrossSettings[k] = default[k]
            end
            for k, v in pairs(cfg.CrossSettings) do
                if CrossSettings[k] ~= nil then
                    CrossSettings[k] = v
                end
            end
            if CrossSettings.Enabled then
                if not crossHeartbeat then Crs_startCrosshair() end
            else
                if crossHeartbeat then crossHeartbeat:Disconnect(); crossHeartbeat = nil end
            end
            if previewUpdater then pcall(previewUpdater) end
            BuildCrossTab()
            statusLabel.Text = "\240\159\147\130 \229\183\178\232\188\137\229\133\165\228\184\166\230\184\133\231\169\186\232\136\138\232\168\173\229\174\154\239\188\154" .. selectedConfigName
        else
            statusLabel.Text = "\226\157\140 \232\168\173\229\174\154\230\170\148\230\160\188\229\188\143\233\140\175\232\170\164\230\136\150\231\132\161\230\186\150\230\152\159\232\179\135\230\150\153"
        end
    end)

    makeBtn("\229\132\178\229\173\152", Color3.fromRGB(0, 120, 255), 8, function()
        if not selectedConfigName then statusLabel.Text = "\226\157\140 \232\171\139\229\133\136\229\156\168\230\184\133\229\150\174\228\184\173\233\129\184\229\143\150\232\168\173\229\174\154\230\170\148" return end
        local path = configDir .. "/" .. selectedConfigName .. ".json"
        local cfg = { CrossSettings = CrossSettings }
        local ok, err = pcall(function() writefile(path, HttpService:JSONEncode(cfg)) end)
        statusLabel.Text = ok and "\240\159\146\190 \229\183\178\229\132\178\229\173\152\232\135\179\239\188\154" .. selectedConfigName or "\226\157\140 \229\132\178\229\173\152\229\164\177\230\149\151\239\188\154" .. tostring(err)
    end)
end

BuildAttackTab()
BuildVisualTab()
BuildCrossTab()
BuildConfigTab()

end

local CrosshairGroup = Tabs.Fun:AddLeftGroupbox("XingHui Crosshair")
CrosshairGroup:AddToggle("XH_CrosshairEnabled", {
	Text = "Enable Crosshair",
	Default = false,
	Callback = function(v)
		if v then
			pcall(function()
				if not _G.XingHuiCrosshair then
					setupXingHuiCrosshair()
				end
				if _G.XingHuiCrosshair then
					_G.XingHuiCrosshair.Settings.Enabled = true
					if _G.XingHuiCrosshair.Start then
						_G.XingHuiCrosshair.Start()
					end
					-- Also synchronize the Lock/Unlock state and menu panel state
					if _G.XingHuiCrosshair.LockBtnFrame and _G.XingHuiCrosshair.HubBtnFrame then
						_G.XingHuiCrosshair.LockBtnFrame.Visible = Toggles.XH_FloatButtons and Toggles.XH_FloatButtons.Value or false
						_G.XingHuiCrosshair.HubBtnFrame.Visible = Toggles.XH_FloatButtons and Toggles.XH_FloatButtons.Value or false
					end
				end
			end)
		else
			pcall(function()
				if _G.XingHuiCrosshair then
					_G.XingHuiCrosshair.Settings.Enabled = false
					if _G.XingHuiCrosshair.Heartbeat then _G.XingHuiCrosshair.Heartbeat:Disconnect() end
					if _G.XingHuiCrosshair.Gui then _G.XingHuiCrosshair.Gui:Destroy() end
					if _G.XingHuiCrosshair.Menu then _G.XingHuiCrosshair.Menu:Destroy() end
					if _G.XingHuiCrosshair.LockBtnFrame then _G.XingHuiCrosshair.LockBtnFrame:Destroy() end
					if _G.XingHuiCrosshair.HubBtnFrame then _G.XingHuiCrosshair.HubBtnFrame:Destroy() end
					_G.XingHuiCrosshair = nil
				end
			end)
		end
	end
})
CrosshairGroup:AddButton("Toggle Menu Panel", function()
	if _G.XingHuiCrosshair and _G.XingHuiCrosshair.Panel then
		_G.XingHuiCrosshair.Panel.Visible = not _G.XingHuiCrosshair.Panel.Visible
	end
end)
CrosshairGroup:AddToggle("XH_FloatButtons", {
	Text = "Show Floating Buttons",
	Default = false,
	Callback = function(v)
		if _G.XingHuiCrosshair then
			if _G.XingHuiCrosshair.LockBtnFrame and _G.XingHuiCrosshair.HubBtnFrame then
				_G.XingHuiCrosshair.LockBtnFrame.Visible = v
				_G.XingHuiCrosshair.HubBtnFrame.Visible = v
			end
		end
	end
})

-- ===== SPOOFER =====
local SpooferGroup = Tabs.Fun:AddRightGroupbox("Spoofer")
SpooferGroup:AddToggle("ProfilePicSpoof", { Text = "Swap Profile Pictures", Default = false })
SpooferGroup:AddToggle("NameSpoof", { Text = "Swap Names", Default = false })
SpooferGroup:AddLabel("Pic: i.imgur.com/65scUcs.png")
SpooferGroup:AddLabel("Name: ????????????????????????????")
SpooferGroup:AddDivider()
SpooferGroup:AddToggle("NoobAvatar", { Text = "Noob Avatar", Default = false })

-- ===== FPS UNLOCKER =====
local FPSUnlockerGroup = Tabs.Fun:AddRightGroupbox("FPS Unlocker")
local hasSetFpsCap = typeof(setfpscap) == "function"
if hasSetFpsCap then
	FPSUnlockerGroup:AddToggle("FPSUnlockerEnabled", {
		Text = "FPS Unlocker",
		Default = false,
		Callback = function(v)
			if v then
				local cap = Options.FPSCapVal and Options.FPSCapVal.Value or 2000
				pcall(setfpscap, cap)
			else
				pcall(setfpscap, 60)
			end
		end
	})
	FPSUnlockerGroup:AddSlider("FPSCapVal", {
		Text = "FPS Cap Limit",
		Default = 2000,
		Min = 60,
		Max = 2000,
		Rounding = 0,
		Suffix = " FPS",
		Callback = function(val)
			if Toggles.FPSUnlockerEnabled and Toggles.FPSUnlockerEnabled.Value then
				pcall(setfpscap, val)
			end
		end
	})
else
	FPSUnlockerGroup:AddLabel("setfpscap not supported")
end

-- ===== ASPECT RATIO (STRETCHED) =====
local AspectGroup = Tabs.Fun:AddRightGroupbox("Aspect Ratio / Stretched")
AspectGroup:AddToggle("AspectEnabled", { Text = "Stretched Resolution", Default = false })
AspectGroup:AddSlider("AspectX", { Text = "Stretch Width (\229\175\172\229\186\166\230\139\137\228\188\184)", Default = 1.33, Min = 0.5, Max = 3.0, Rounding = 2, Suffix = "x" })
AspectGroup:AddSlider("AspectY", { Text = "Stretch Height (\233\171\152\229\186\166\230\137\129\229\186\166)", Default = 1.00, Min = 0.5, Max = 3.0, Rounding = 2, Suffix = "x" })

-- ===== SPIN BOT =====
local SpinBotGroup = Tabs.Fun:AddRightGroupbox("Spin Bot")
SpinBotGroup:AddToggle("SpinBotEnabled", { Text = "Enabled", Default = false })
SpinBotGroup:AddSlider("SpinBotSpeed", { Text = "Speed", Default = 50, Min = 10, Max = 150, Suffix = "", Rounding = 0 })

local VMStanceGroup = Tabs.Fun:AddRightGroupbox("Viewmodel Stance")
VMStanceGroup:AddToggle("AestheticVM", { Text = "Aesthetic Hand Position (\230\137\139\229\129\143\229\143\179\229\129\180)", Default = false })

-- ===== CUSTOM KEYBIND PANEL =====
local _kbPanelGui = nil
local _kbMainFrame = nil

local function buildCustomKeybindPanel()
	if _kbPanelGui then _kbPanelGui:Destroy() end

	local kbContainer = (gethui and gethui()) or game:GetService("CoreGui")
	local kbGui = Instance.new("ScreenGui", kbContainer)
	kbGui.Name = "MokeKeybindPanel"
	kbGui.ResetOnSpawn = false
	kbGui.IgnoreGuiInset = true
	kbGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	_kbPanelGui = kbGui

	local kbTween = game:GetService("TweenService")
	local W = 220

	local panel = Instance.new("Frame", kbGui)
	panel.Name = "Panel"
	panel.Size = UDim2.new(0, W, 0, 20)
	panel.Position = UDim2.new(0, 8, 0.5, -160)
	panel.BackgroundColor3 = Color3.fromRGB(10, 10, 16)
	panel.BackgroundTransparency = 0.05
	panel.BorderSizePixel = 0
	panel.Visible = false
	Instance.new("UICorner", panel).CornerRadius = UDim.new(0, 10)
	local panelStroke = Instance.new("UIStroke", panel)
	panelStroke.Color = Color3.fromRGB(70, 70, 110)
	panelStroke.Thickness = 1.5
	_kbMainFrame = panel

	-- Header background
	local headerH = 90
	local headerBg = Instance.new("Frame", panel)
	headerBg.Size = UDim2.new(1, 0, 0, headerH)
	headerBg.BackgroundColor3 = Color3.fromRGB(16, 16, 26)
	headerBg.BorderSizePixel = 0
	Instance.new("UICorner", headerBg).CornerRadius = UDim.new(0, 10)

	-- Left animated box: Moke.cc logo
	local logoFrame = Instance.new("Frame", headerBg)
	logoFrame.Size = UDim2.new(0, 92, 0, 68)
	logoFrame.Position = UDim2.new(0, 10, 0, 11)
	logoFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 30)
	logoFrame.BorderSizePixel = 0
	Instance.new("UICorner", logoFrame).CornerRadius = UDim.new(0, 8)
	local logoStroke = Instance.new("UIStroke", logoFrame)
	logoStroke.Color = Color3.fromRGB(90, 90, 170)
	logoStroke.Thickness = 1.5

	local mokeLabel = Instance.new("TextLabel", logoFrame)
	mokeLabel.Size = UDim2.new(1, 0, 0, 32)
	mokeLabel.Position = UDim2.new(0, 0, 0, 8)
	mokeLabel.BackgroundTransparency = 1
	mokeLabel.Text = "Moke.cc"
	mokeLabel.TextColor3 = Color3.fromRGB(140, 140, 255)
	mokeLabel.Font = Enum.Font.GothamBold
	mokeLabel.TextSize = 17

	local killLabel = Instance.new("TextLabel", logoFrame)
	killLabel.Size = UDim2.new(1, 0, 0, 22)
	killLabel.Position = UDim2.new(0, 0, 0, 40)
	killLabel.BackgroundTransparency = 1
	killLabel.Text = "OVERKILL"
	killLabel.TextColor3 = Color3.fromRGB(255, 90, 90)
	killLabel.Font = Enum.Font.GothamBold
	killLabel.TextSize = 12

	-- Right animated box: Player avatar
	local avatarFrame = Instance.new("Frame", headerBg)
	avatarFrame.Size = UDim2.new(0, 72, 0, 68)
	avatarFrame.Position = UDim2.new(1, -84, 0, 11)
	avatarFrame.BackgroundColor3 = Color3.fromRGB(18, 18, 30)
	avatarFrame.BorderSizePixel = 0
	Instance.new("UICorner", avatarFrame).CornerRadius = UDim.new(0, 8)
	local avatarStroke = Instance.new("UIStroke", avatarFrame)
	avatarStroke.Color = Color3.fromRGB(90, 90, 170)
	avatarStroke.Thickness = 1.5

	local avatarImg = Instance.new("ImageLabel", avatarFrame)
	avatarImg.Size = UDim2.new(1, -8, 1, -8)
	avatarImg.Position = UDim2.new(0, 4, 0, 4)
	avatarImg.BackgroundTransparency = 1
	avatarImg.Image = "https://www.roblox.com/headshot-thumbnail/image?userId=" .. LP.UserId .. "&width=150&height=150&format=png"
	Instance.new("UICorner", avatarImg).CornerRadius = UDim.new(0, 6)

	-- Floating animation: both boxes bob up/down offset by half period
	local function floatElement(elem, offsetY, duration, startDelay)
		task.spawn(function()
			task.wait(startDelay or 0)
			local baseY = elem.Position.Y.Offset
			local bXs = elem.Position.X.Scale
			local bXo = elem.Position.X.Offset
			local going = true
			while elem.Parent do
				local targetY = going and (baseY - offsetY) or (baseY + offsetY)
				local t = kbTween:Create(elem, TweenInfo.new(duration, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {
					Position = UDim2.new(bXs, bXo, 0, targetY)
				})
				t:Play()
				t.Completed:Wait()
				going = not going
			end
		end)
	end
	floatElement(logoFrame, 5, 1.1, 0)
	floatElement(avatarFrame, 5, 1.1, 0.55)

	-- Subtitle
	local subtitleLbl = Instance.new("TextLabel", panel)
	subtitleLbl.Size = UDim2.new(1, -10, 0, 18)
	subtitleLbl.Position = UDim2.new(0, 5, 0, headerH + 5)
	subtitleLbl.BackgroundTransparency = 1
	subtitleLbl.Text = "Moke.cc  \226\156\182  keybind menu"
	subtitleLbl.TextColor3 = Color3.fromRGB(100, 100, 160)
	subtitleLbl.Font = Enum.Font.Gotham
	subtitleLbl.TextSize = 10
	subtitleLbl.TextXAlignment = Enum.TextXAlignment.Center

	-- Divider line
	local divY = headerH + 26
	local divLine = Instance.new("Frame", panel)
	divLine.Size = UDim2.new(1, -16, 0, 1)
	divLine.Position = UDim2.new(0, 8, 0, divY)
	divLine.BackgroundColor3 = Color3.fromRGB(55, 55, 90)
	divLine.BorderSizePixel = 0

	-- Keybind rows list
	local listTop = divY + 6
	local listFrame = Instance.new("Frame", panel)
	listFrame.Position = UDim2.new(0, 0, 0, listTop)
	listFrame.Size = UDim2.new(1, 0, 0, 10)
	listFrame.BackgroundTransparency = 1

	local listLayout = Instance.new("UIListLayout", listFrame)
	listLayout.Padding = UDim.new(0, 3)
	listLayout.SortOrder = Enum.SortOrder.LayoutOrder

	local lPad = Instance.new("UIPadding", listFrame)
	lPad.PaddingLeft = UDim.new(0, 6)
	lPad.PaddingRight = UDim.new(0, 6)
	lPad.PaddingTop = UDim.new(0, 3)
	lPad.PaddingBottom = UDim.new(0, 8)

	local function getKeyStr(keyId)
		local opt = Options[keyId]
		if opt and opt.Value then
			local s = tostring(opt.Value):gsub("Enum%.KeyCode%.",""):gsub("Enum%.UserInputType%.","")
			local map = {
				MouseButton1="MB1", MouseButton2="MB2", MouseButton3="MB3",
				LeftShift="L.SHIFT", RightShift="R.SHIFT",
				LeftControl="L.CTRL", RightControl="R.CTRL",
			}
			return map[s] or s:upper()
		end
		return "?"
	end

	local function addKbRow(keyId, displayName, toggleId, staticKey)
		local row = Instance.new("Frame", listFrame)
		row.Size = UDim2.new(1, 0, 0, 34)
		row.BackgroundColor3 = Color3.fromRGB(17, 16, 28)
		row.BackgroundTransparency = 0
		row.BorderSizePixel = 0
		row.ZIndex = 3
		Instance.new("UICorner", row).CornerRadius = UDim.new(0, 7)
		local rowStroke = Instance.new("UIStroke", row)
		rowStroke.Color = Color3.fromRGB(38, 35, 70)
		rowStroke.Thickness = 1
		rowStroke.Transparency = 0.3

		-- Keyboard-key badge (3-D top-highlight style)
		local kbg = Instance.new("Frame", row)
		kbg.Size = UDim2.new(0, 52, 0, 22)
		kbg.Position = UDim2.new(0, 6, 0.5, -11)
		kbg.BackgroundColor3 = Color3.fromRGB(22, 20, 40)
		kbg.BorderSizePixel = 0
		kbg.ZIndex = 4
		Instance.new("UICorner", kbg).CornerRadius = UDim.new(0, 5)
		local kbgStroke = Instance.new("UIStroke", kbg)
		kbgStroke.Color = Color3.fromRGB(75, 65, 145)
		kbgStroke.Thickness = 1
		local kTop = Instance.new("Frame", kbg)
		kTop.Size = UDim2.new(1, -4, 0, 1)
		kTop.Position = UDim2.new(0, 2, 0, 1)
		kTop.BackgroundColor3 = Color3.fromRGB(100, 88, 200)
		kTop.BackgroundTransparency = 0.55
		kTop.BorderSizePixel = 0
		kTop.ZIndex = 5
		Instance.new("UICorner", kTop).CornerRadius = UDim.new(0, 1)

		local kLbl = Instance.new("TextLabel", kbg)
		kLbl.Size = UDim2.new(1, 0, 1, 0)
		kLbl.BackgroundTransparency = 1
		kLbl.TextColor3 = Color3.fromRGB(165, 155, 240)
		kLbl.Font = Enum.Font.GothamBold
		kLbl.TextSize = 10
		kLbl.ZIndex = 5
		kLbl.Text = staticKey or getKeyStr(keyId)
		task.spawn(function()
			while row.Parent do
				if not staticKey then kLbl.Text = getKeyStr(keyId) end
				task.wait(0.5)
			end
		end)

		-- Feature label
		local fLbl = Instance.new("TextLabel", row)
		fLbl.Size = UDim2.new(1, -130, 1, 0)
		fLbl.Position = UDim2.new(0, 64, 0, 0)
		fLbl.BackgroundTransparency = 1
		fLbl.Text = displayName
		fLbl.TextColor3 = Color3.fromRGB(195, 192, 218)
		fLbl.Font = Enum.Font.Gotham
		fLbl.TextSize = 12
		fLbl.TextXAlignment = Enum.TextXAlignment.Left
		fLbl.ZIndex = 4

		-- Pill toggle button (rounded, colored background)
		local pillW, pillH = 50, 20
		local pill = Instance.new("TextButton", row)
		pill.Size = UDim2.new(0, pillW, 0, pillH)
		pill.Position = UDim2.new(1, -(pillW + 6), 0.5, -pillH/2)
		pill.BorderSizePixel = 0
		pill.Font = Enum.Font.GothamBold
		pill.TextSize = 10
		pill.AutoButtonColor = false
		pill.ZIndex = 4
		Instance.new("UICorner", pill).CornerRadius = UDim.new(1, 0)

		local TS2 = game:GetService("TweenService")
		if toggleId and Toggles[toggleId] then
			local function refreshPill()
				local on = Toggles[toggleId].Value
				pill.Text = on and "ON" or "OFF"
				pill.TextColor3 = on and Color3.fromRGB(55, 210, 90) or Color3.fromRGB(195, 60, 60)
				TS2:Create(pill, TweenInfo.new(0.12, Enum.EasingStyle.Quad), {
					BackgroundColor3 = on and Color3.fromRGB(22, 52, 28) or Color3.fromRGB(42, 18, 18)
				}):Play()
			end
			pill.BackgroundColor3 = Toggles[toggleId].Value and Color3.fromRGB(22, 52, 28) or Color3.fromRGB(42, 18, 18)
			refreshPill()
			Toggles[toggleId]:OnChanged(refreshPill)
			pill.MouseButton1Click:Connect(function()
				Toggles[toggleId]:SetValue(not Toggles[toggleId].Value)
			end)
		else
			pill.Text = "\226\128\148"
			pill.BackgroundColor3 = Color3.fromRGB(22, 20, 40)
			pill.TextColor3 = Color3.fromRGB(90, 85, 140)
		end
	end

	addKbRow("AimbotKey",     "Aim Key",      "AimbotEnabled")
	addKbRow("TriggerbotKey", "Trigger Key",  "TriggerbotEnabled")
	addKbRow(nil,             "Silent Aim",   "SilentAim",   "SA")
	addKbRow("MenuKeybind",   "Menu Keybind", nil)

	-- Auto-resize panel height to fit rows
	local function resize()
		local h = listLayout.AbsoluteContentSize.Y + 12
		listFrame.Size = UDim2.new(1, 0, 0, h)
		panel.Size = UDim2.new(0, W, 0, listTop + h)
	end
	listLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(resize)
	task.spawn(function() task.wait(0.1); resize() end)

	return panel
end

-- ===== UI SETTINGS =====
local MenuGroup = Tabs["UI Settings"]:AddLeftGroupbox("Menu")
MenuGroup:AddToggle("KeybindMenuOpen", { Default = Library.KeybindFrame.Visible, Text = "Open Keybind Menu", Callback = function(v) Library.KeybindFrame.Visible = v end })
MenuGroup:AddDropdown("NotificationSide", { Values = { "Left", "Right" }, Default = "Right", Text = "Notification Side", Callback = function(v) Library:SetNotifySide(v) end })
MenuGroup:AddDivider()
MenuGroup:AddLabel("Menu bind"):AddKeyPicker("MenuKeybind", { Default = "RightShift", NoUI = true, Text = "Menu keybind" })
MenuGroup:AddButton("Unload", function() Library:Unload() end)

Library.ToggleKeybind = Options.MenuKeybind

ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)
SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ "MenuKeybind" })
ThemeManager:SetFolder("Desy")
SaveManager:SetFolder("Desy/OVERKILL")
SaveManager:BuildConfigSection(Tabs["UI Settings"])
ThemeManager:ApplyToTab(Tabs["UI Settings"])
SaveManager:LoadAutoloadConfig()
end

-- ===== SILENT AIM =====
-- _G._SA stores target; toggle state is read DIRECTLY from Toggles["SilentAim"].Value
-- inside every hook/renderStep \226\128\148 bypasses all upvalue/callback sync issues
_G._SA = { target = nil, camSave = nil }

local phem1 = {}
local phem2 = {}

do
	local function isPlayerVisible(targetPart)
		local camera = workspace.CurrentCamera
		if not camera then return false end
		
		local origin = camera.CFrame.Position
		local dest = targetPart.Position
		local dir = dest - origin
		
		local params = RaycastParams.new()
		params.FilterType = Enum.RaycastFilterType.Exclude
		
		local excludeList = { LP.Character }
		if targetPart.Parent then
			table.insert(excludeList, targetPart.Parent)
		end
		params.FilterDescendantsInstances = excludeList
		
		local result = workspace:Raycast(origin, dir, params)
		if result then
			return false
		end
		return true
	end

	function phem1:get_closest_player()
		-- pcall so require errors don't kill the whole scan loop
		local ok, err = pcall(function()
			local phem3  = cloneref(game:GetService('Players'))
			local phem4  = cloneref(game:GetService('Workspace'))
			local phem5  = cloneref(game:GetService('ReplicatedFirst'))
			local phem6  = cloneref(game:GetService('ReplicatedStorage'))
			local phem7  = phem3.LocalPlayer
			local phem8  = phem4.CurrentCamera
			local phem9  = require(game.FindFirstChild(phem5, 'neuron', true))
			local phem10 = require(game.FindFirstChild(phem6, 'States', true))
			local phem11 = game.GetChildren(game.FindFirstChild(phem4, 'Entities', true))
			local phem12 = 9e9
			_G._SA.target = nil
			local phem13 = {}
			for _, phem14 in phem3:GetPlayers() do
				if phem14 == phem7 then continue end
				local phem15 = phem9:get_character(phem14)
				if phem15 then table.insert(phem13, { char = phem15 }) end
			end
			for _, phem16 in phem11 do
				table.insert(phem13, { char = phem16 })
			end
			local phem17 = Vector2.new((phem8.ViewportSize.X / 2), (phem8.ViewportSize.Y / 2))
			for _, phem18 in phem13 do
				local phem19 = phem18.char
				if phem10:GetStateValue(phem19, 'Dead', false) then continue end
				local phem20 = game.FindFirstChild(phem19, 'HitboxHead')
				if not phem20 then continue end
				
				-- Visibility check
				if not isPlayerVisible(phem20) then continue end
				
				local phem21, phem22 = phem8:WorldToViewportPoint(phem20.Position)
				if not phem22 then continue end
				local phem23 = (Vector2.new(phem21.X, phem21.Y) - phem17).Magnitude
				if phem23 < phem12 then
					phem12 = phem23
					_G._SA.target = phem20.Position
				end
			end
		end)
	end
end

local phem24 = cloneref(game:GetService('RunService'))
phem24.PreRender:Connect(function()
	phem1:get_closest_player()
end)

local phem26 = cloneref(game:GetService('Workspace')).CurrentCamera
local phem27 = cloneref(game:GetService('ReplicatedFirst'))
local phem28 = cloneref(game:GetService('ReplicatedStorage'))
local phem29 = require(game.FindFirstChild(phem27, 'GenerateCommand', true))
local phem30 = require(game.FindFirstChild(phem28, 'CameraHandler', true))
local phem31 = require(game.FindFirstChild(phem28, 'Effects', true))

local phem33; phem33 = hookfunction(phem29.GetCameraAngles, newlclosure(function(...)
	if not phem30.firstPerson then return phem33(...) end
	local phem34 = (_G._SA.camSave or phem30.currentRotation)
	return phem34.X, phem34.Y
end))

local function getVictimHead(victim)
	if not victim then return nil end
	if type(victim) == "table" then
		if victim.Head then return victim.Head end
		if victim.Model then
			return victim.Model:FindFirstChild("Head") or victim.Model:FindFirstChildOfClass("BasePart") or victim.Model.PrimaryPart
		end
	end
	if typeof(victim) == "Instance" then
		if victim:IsA("BasePart") then return victim end
		local head = victim:FindFirstChild("Head")
		if head then return head end
		if victim:IsA("Model") then
			return victim.PrimaryPart or victim:FindFirstChildOfClass("BasePart")
		end
	end
	return nil
end

local spawnDamageIndicator
do
	local TweenService = game:GetService("TweenService")
	local Debris = game:GetService("Debris")
	local fontMap = {
		Minecraft = Enum.Font.Arcade,
		Cartoon = Enum.Font.LuckiestGuy,
		Comic = Enum.Font.Bangers,
		Arcade = Enum.Font.Arcade
	}
	function spawnDamageIndicator(victimChar, damage, style)
		local ok, err = pcall(function()
			local head = getVictimHead(victimChar)
			if not head then return end
			
			local color = Options.DamageIndicatorsColor and Options.DamageIndicatorsColor.Value or Color3.fromRGB(255, 100, 0)
			
			local billing = Instance.new("BillboardGui")
			billing.Size = UDim2.new(0, 150, 0, 60)
			billing.AlwaysOnTop = true
			billing.Adornee = head
			billing.StudsOffset = Vector3.new(0, 1.5, 0)
			
			local label = Instance.new("TextLabel")
			label.Size = UDim2.new(1, 0, 1, 0)
			label.BackgroundTransparency = 1
			label.Text = tostring(math.round(damage))
			label.TextColor3 = color
			label.TextSize = 26
			label.Font = fontMap[style] or Enum.Font.LuckiestGuy
			label.TextStrokeTransparency = 0
			label.TextStrokeColor3 = Color3.new(0, 0, 0)
			label.Parent = billing
			
			-- Parent to the game's FX folder or victim character to ensure replication and rendering
			local fx = workspace:FindFirstChild("World") and workspace.World:FindFirstChild("FX")
			billing.Parent = fx or head
			
			Debris:AddItem(billing, 1.0)
			
			-- Animate floating up and scaling down/transparency
			local tweenInfo = TweenInfo.new(0.8, Enum.EasingStyle.OutQuad, Enum.EasingDirection.Out)
			local randomX = math.random(-15, 15) / 10 -- Random shift left/right
			local targetOffset = Vector3.new(randomX, 4.5, 0)
			
			local offsetTween = TweenService:Create(billing, tweenInfo, { StudsOffset = targetOffset })
			local textTween = TweenService:Create(label, tweenInfo, { TextTransparency = 1, TextStrokeTransparency = 1 })
			
			offsetTween:Play()
			textTween:Play()
		end)
	end
end

local phem35; phem35 = hookfunction(phem31.Identify, newlclosure(function(self, phem36, ...)
	if phem36 == "Shot" then
		local phem37 = (select(4, ...))
		-- Silent Aim: redirect hitPos to our locked target
		if Toggles["SilentAim"].Value and _G._SA.target and phem37 and phem37.origin and phem37.hitPos then
			phem37.hitPos = (phem37.origin + (_G._SA.target - phem37.origin).Unit * (phem37.hitPos - phem37.origin).Magnitude)
		end
	end

	return phem35(self, phem36, ...)
end))

phem24:BindToRenderStep('_SA_rot', (math.huge - math.huge), function()
	-- Read Toggles value directly every frame
	if not Toggles["SilentAim"].Value or not phem30.firstPerson or not _G._SA.target then return end
	if not _G._SA.camSave then
		_G._SA.camSave = phem30.currentRotation
	end
	local dir = (_G._SA.target - phem26.CFrame.p).Unit
	phem30.currentRotation = vector.create(math.asin(dir.Y), math.atan2(-dir.X, -dir.Z), 0)
end)

phem24:BindToRenderStep('_SA_restore', 101, function()
	if _G._SA.camSave then
		if phem30.firstPerson then
			phem30.currentRotation = _G._SA.camSave
		end
		_G._SA.camSave = nil
	end
end)

local UserInputService = game:GetService("UserInputService")
UserInputService.WindowFocusReleased:Connect(function()
	_G._SA.camSave = nil
end)
UserInputService.WindowFocused:Connect(function()
	_G._SA.camSave = nil
end)


-- ===== GAME LOGIC =====
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera
local Lighting = game:GetService("Lighting")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local CrossGui, screenGui, LockBtnFrame, HubBtnFrame, crossHeartbeat
local fovCircle
local espPool = {}
local maxESPObjects = 50
local entities = {}
local chamsHighlights = {}
local lastScan = 0
local skyInstance
local customSkyInstance
local originalLightingSettings
local skeletonParts = { "Head", "UpperTorso", "LowerTorso", "LeftUpperArm", "LeftLowerArm", "LeftHand", "RightUpperArm", "RightLowerArm", "RightHand", "LeftUpperLeg", "LeftLowerLeg", "LeftFoot", "RightUpperLeg", "RightLowerLeg", "RightFoot" }
local skeletonConnections = { {1,2},{2,3},{2,4},{4,5},{5,6},{2,7},{7,8},{8,9},{3,10},{10,11},{11,12},{3,13},{13,14},{14,15} }

-- Initialize ESP Pool
for i = 1, maxESPObjects do
	espPool[i] = {
		BoxOutline = Drawing.new("Square"),
		Box = Drawing.new("Square"),
		BoxFill = Drawing.new("Square"),
		HeadDotOutline = Drawing.new("Circle"),
		HeadDot = Drawing.new("Circle"),
		Name = Drawing.new("Text"),
		Distance = Drawing.new("Text"),
		WeaponName = Drawing.new("Text"),
		HealthBarOutline = Drawing.new("Square"),
		HealthBar = Drawing.new("Square"),
		HealthText = Drawing.new("Text"),
		TracerOutline = Drawing.new("Line"),
		Tracer = Drawing.new("Line"),
		OffscreenArrow = Drawing.new("Triangle"),
		Corners = {},
		CornerOutlines = {},
		Skeleton = {}
	}

	local obj = espPool[i]
	obj.BoxOutline.Thickness = 3
	obj.BoxOutline.Filled = false
	obj.BoxOutline.Color = Color3.fromRGB(0, 0, 0)
	obj.BoxOutline.Transparency = 0.5
	obj.BoxOutline.Visible = false

	obj.Box.Thickness = 1
	obj.Box.Filled = false
	obj.Box.Visible = false

	obj.BoxFill.Thickness = 0
	obj.BoxFill.Filled = true
	obj.BoxFill.Color = Color3.fromRGB(12, 12, 16)
	obj.BoxFill.Transparency = 0.22
	obj.BoxFill.Visible = false

	for c = 1, 8 do
		obj.Corners[c] = Drawing.new("Line")
		obj.Corners[c].Thickness = 1.2
		obj.Corners[c].Visible = false

		obj.CornerOutlines[c] = Drawing.new("Line")
		obj.CornerOutlines[c].Thickness = 3.2
		obj.CornerOutlines[c].Color = Color3.fromRGB(0, 0, 0)
		obj.CornerOutlines[c].Transparency = 0.5
		obj.CornerOutlines[c].Visible = false
	end

	obj.HeadDotOutline.Radius = 4
	obj.HeadDotOutline.Thickness = 1
	obj.HeadDotOutline.Filled = true
	obj.HeadDotOutline.Color = Color3.fromRGB(0, 0, 0)
	obj.HeadDotOutline.Visible = false

	obj.HeadDot.Radius = 3
	obj.HeadDot.Thickness = 1
	obj.HeadDot.Filled = true
	obj.HeadDot.Visible = false

	obj.Name.Size = 13
	obj.Name.Font = 2
	obj.Name.Center = true
	obj.Name.Outline = true
	obj.Name.Visible = false

	obj.Distance.Size = 10
	obj.Distance.Font = 2
	obj.Distance.Center = true
	obj.Distance.Outline = true
	obj.Distance.Visible = false

	obj.WeaponName.Size = 10
	obj.WeaponName.Font = 2
	obj.WeaponName.Center = true
	obj.WeaponName.Outline = true
	obj.WeaponName.Visible = false

	obj.HealthBarOutline.Thickness = 1
	obj.HealthBarOutline.Filled = true
	obj.HealthBarOutline.Color = Color3.fromRGB(0, 0, 0)
	obj.HealthBarOutline.Visible = false

	obj.HealthBar.Thickness = 1
	obj.HealthBar.Filled = true
	obj.HealthBar.Visible = false

	obj.HealthText.Size = 9
	obj.HealthText.Font = 2
	obj.HealthText.Center = true
	obj.HealthText.Outline = true
	obj.HealthText.Visible = false

	obj.TracerOutline.Thickness = 3
	obj.TracerOutline.Color = Color3.fromRGB(0, 0, 0)
	obj.TracerOutline.Visible = false

	obj.Tracer.Thickness = 1
	obj.Tracer.Visible = false

	obj.OffscreenArrow.Filled = true
	obj.OffscreenArrow.Visible = false

	for j = 1, #skeletonConnections do
		obj.Skeleton[j] = {
			Outline = Drawing.new("Line"),
			Line = Drawing.new("Line")
		}
		obj.Skeleton[j].Outline.Thickness = 3
		obj.Skeleton[j].Outline.Color = Color3.fromRGB(0, 0, 0)
		obj.Skeleton[j].Outline.Visible = false
		obj.Skeleton[j].Line.Thickness = 1
		obj.Skeleton[j].Line.Visible = false
	end
end

-- Hook Library Unload to clean up ESP Pool
local oldUnload = Library.Unload
Library.Unload = function(self)
	pcall(function() RunService:UnbindFromRenderStep("ESP_Draw") end)
	pcall(function()
		if _G.XingHuiCrosshair then
			_G.XingHuiCrosshair.Settings.Enabled = false
			if _G.XingHuiCrosshair.Heartbeat then _G.XingHuiCrosshair.Heartbeat:Disconnect() end
			if _G.XingHuiCrosshair.Gui then _G.XingHuiCrosshair.Gui:Destroy() end
			if _G.XingHuiCrosshair.Menu then _G.XingHuiCrosshair.Menu:Destroy() end
			if _G.XingHuiCrosshair.LockBtnFrame then _G.XingHuiCrosshair.LockBtnFrame:Destroy() end
			if _G.XingHuiCrosshair.HubBtnFrame then _G.XingHuiCrosshair.HubBtnFrame:Destroy() end
			_G.XingHuiCrosshair = nil
		end
		if CrossGui then CrossGui:Destroy() end
		if screenGui then screenGui:Destroy() end
		if LockBtnFrame then LockBtnFrame:Destroy() end
		if HubBtnFrame then HubBtnFrame:Destroy() end
		if crossHeartbeat then crossHeartbeat:Disconnect() end
	end)
	for i = 1, maxESPObjects do
		local obj = espPool[i]
		if obj then
			pcall(function() obj.BoxOutline:Remove() end)
			pcall(function() obj.Box:Remove() end)
			pcall(function() obj.BoxFill:Remove() end)
			pcall(function() obj.HeadDotOutline:Remove() end)
			pcall(function() obj.HeadDot:Remove() end)
			pcall(function() obj.Name:Remove() end)
			pcall(function() obj.Distance:Remove() end)
			pcall(function() obj.WeaponName:Remove() end)
			pcall(function() obj.HealthBarOutline:Remove() end)
			pcall(function() obj.HealthBar:Remove() end)
			pcall(function() obj.HealthText:Remove() end)
			pcall(function() obj.TracerOutline:Remove() end)
			pcall(function() obj.Tracer:Remove() end)
			pcall(function() obj.OffscreenArrow:Remove() end)
			if obj.Corners then
				for c = 1, 8 do
					pcall(function() obj.Corners[c]:Remove() end)
					pcall(function() obj.CornerOutlines[c]:Remove() end)
				end
			end
			if obj.Skeleton then
				for j = 1, #skeletonConnections do
					if obj.Skeleton[j] then
						pcall(function() obj.Skeleton[j].Outline:Remove() end)
						pcall(function() obj.Skeleton[j].Line:Remove() end)
					end
				end
			end
		end
	end
	if fovCircle then pcall(function() fovCircle:Remove() end) end
	clearTracers()
	pcall(oldUnload, self)
end

local ViewModel, CameraHandler, Effects, SoundManager, InventoryHandler, UniversalDamageModule
local function tryRequire(path)
	local ok, mod = pcall(function() return require(path) end)
	return ok and mod or nil
end

local function lazyLoad()
	if not Effects then Effects = tryRequire(ReplicatedStorage.Modules.Handlers.Effects) end
	if not SoundManager then SoundManager = tryRequire(ReplicatedStorage.Modules.SoundManager) end
	if not CameraHandler then CameraHandler = tryRequire(ReplicatedStorage.Modules.Handlers.CameraHandler) end
	if not ViewModel then
		local render = tryRequire(ReplicatedStorage.Modules.WeaponECS.Render)
		if render then
			ViewModel = render.ViewModel or tryRequire(ReplicatedStorage.Modules.WeaponECS.Render.ViewModel)
		end
	end
	if not InventoryHandler then InventoryHandler = tryRequire(ReplicatedStorage.Modules.Handlers.InventoryHandler) end
	if not UniversalDamageModule then
		UniversalDamageModule = tryRequire(ReplicatedStorage.Modules.Handlers.Effects.universal.Damage)
	end
	local damageMod = (Effects and Effects.Effects and Effects.Effects.universal and Effects.Effects.universal.Damage) or UniversalDamageModule
	if damageMod and not damageMod._hooked then
		damageMod._hooked = true
		local oldText = damageMod.Text
		damageMod.Text = function(self, attackerChar, victimChar, damage, isHeadshot)
			if not Toggles.DamageIndicators or not Toggles.DamageIndicators.Value then
				return
			end
			
			local style = Options.DamageIndicatorsStyle and Options.DamageIndicatorsStyle.Value or "Cartoon"
			if style == "Default" then
				oldText(self, attackerChar, victimChar, damage, isHeadshot)
			else
				pcall(function()
					if victimChar and damage and damage > 0 then
						spawnDamageIndicator(victimChar, damage, style)
					end
				end)
			end
		end
	end
end

local function isFriend(ent)
	if ent.Model and ent.Model:IsA("Model") then
		for _, p in ipairs(Players:GetPlayers()) do
			if p.Character == ent.Model then return LP:IsFriendsWith(p.UserId) end
		end
	end
	return false
end

local function getHeadPos(ent)
	if ent.Head and ent.Head:IsA("BasePart") then return ent.Head.Position end
	if ent.Model and ent.Model:IsA("Model") then
		local h = ent.Model:FindFirstChild("Head")
		if h and h:IsA("BasePart") then ent.Head = h; return h.Position end
	end
	if ent.Parts then
		for _, p in ipairs(ent.Parts) do
			if p.Name == "Head" and p:IsA("BasePart") then ent.Head = p; return p.Position end
		end
	end
	return nil
end

local function getFootPos(ent)
	if ent.Parts then
		local bestY, best = math.huge, nil
		for _, p in ipairs(ent.Parts) do if p:IsA("BasePart") and p.Position.Y < bestY then bestY = p.Position.Y; best = p end end
		if best then return best.Position end
	end
	return nil
end

local function findAll()
	local models = {}
	local nMod = getNeuronModule()
	local localChar = LP.Character or (nMod and nMod:get_character(LP))
	
	local function scanModel(m, nameOverride)
		local isDead = false
		local hp = 100
		local sMod = getStatesModule()
		local charStates = sMod and sMod.liveCharacters[m]
		if charStates and charStates.Health then
			isDead = sMod:GetStateValue(m, "Dead", false)
			hp = sMod:GetStateValue(m, "Health", 100)
		else
			local hum = m:FindFirstChildWhichIsA("Humanoid")
			if not hum then return end
			isDead = hum.Health <= 0
			hp = hum.Health
		end
		if isDead or hp <= 0 then return end
		local parts = {}
		for _, child in ipairs(m:GetDescendants()) do
			if child:IsA("BasePart") and child.Transparency < 0.95 then
				table.insert(parts, child)
			end
		end
		if #parts >= 3 then
			local avg = Vector3.new()
			for _, p2 in ipairs(parts) do avg += p2.Position end
			avg /= #parts
			models[m] = { Position = avg, Name = nameOverride or m.Name, Parts = parts, Model = m, lastSeen = tick() }
		end
	end
	for _, p in ipairs(Players:GetPlayers()) do
		if p ~= LP and p.Character then
			scanModel(p.Character, p.Name)
		end
	end
	local entitiesFolder = workspace:FindFirstChild("World")
	if entitiesFolder then
		entitiesFolder = entitiesFolder:FindFirstChild("Entities")
	end
	if entitiesFolder then
		for _, child in ipairs(entitiesFolder:GetChildren()) do
			if child:IsA("Model") and child ~= localChar and not models[child] then
				scanModel(child)
			end
		end
	end
	for _, obj in ipairs(workspace:GetDescendants()) do
		if obj:IsA("Model") and obj ~= localChar and obj ~= LP.Character and not models[obj] then
			scanModel(obj)
		end
	end
	return models
end

-- Mines game
local minesGui
local function createMinesUI()
	if minesGui then minesGui:Destroy() end
	minesGui = Instance.new("ScreenGui")
	minesGui.Name = "MinesGame"
	minesGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	minesGui.ResetOnSpawn = false
	minesGui.Parent = LP:WaitForChild("PlayerGui")

	local frame = Instance.new("Frame")
	frame.BackgroundTransparency = 1
	frame.Position = UDim2.fromScale(0.5, 0.5)
	frame.AnchorPoint = Vector2.new(0.5, 0.5)
	frame.Size = UDim2.fromOffset(320, 350)
	frame.Parent = minesGui

	local gridFrame = Instance.new("Frame")
	gridFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
	gridFrame.BorderSizePixel = 0
	gridFrame.Size = UDim2.fromOffset(300, 300)
	gridFrame.Position = UDim2.fromOffset(10, 10)
	gridFrame.Parent = frame

	local uigrid = Instance.new("UIGridLayout")
	uigrid.CellPadding = UDim2.fromOffset(4, 4)
	uigrid.CellSize = UDim2.fromOffset(55, 55)
	uigrid.FillDirection = Enum.FillDirection.Horizontal
	uigrid.HorizontalAlignment = Enum.HorizontalAlignment.Center
	uigrid.VerticalAlignment = Enum.VerticalAlignment.Center
	uigrid.Parent = gridFrame

	minesState.buttons = {}
	for i = 1, 25 do
		local btn = Instance.new("TextButton")
		btn.Text = "?"
		btn.TextColor3 = Color3.fromRGB(255, 255, 255)
		btn.TextScaled = true
		btn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
		btn.BorderSizePixel = 0
		btn.AutoButtonColor = false
		btn.Parent = gridFrame
		btn.MouseButton1Click:Connect(function()
			if minesState.status ~= "playing" or minesState.buttons[i].Revealed then return end
			revealTile(i)
		end)
		minesState.buttons[i] = btn
	end

	local bottomFrame = Instance.new("Frame")
	bottomFrame.BackgroundTransparency = 1
	bottomFrame.Size = UDim2.new(1, 0, 0, 40)
	bottomFrame.Position = UDim2.fromOffset(0, 315)
	bottomFrame.Parent = frame

	local infoLabel = Instance.new("TextLabel")
	infoLabel.Text = "Mines: " .. minesState.mineCount .. "  |  Revealed: 0"
	infoLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
	infoLabel.TextScaled = false
	infoLabel.TextSize = 14
	infoLabel.BackgroundTransparency = 1
	infoLabel.Size = UDim2.new(0.6, -5, 1, 0)
	infoLabel.Position = UDim2.fromOffset(0, 0)
	infoLabel.Name = "InfoLabel"
	infoLabel.Parent = bottomFrame

	local closeBtn = Instance.new("TextButton")
	closeBtn.Text = "X"
	closeBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
	closeBtn.TextSize = 16
	closeBtn.BackgroundTransparency = 1
	closeBtn.Size = UDim2.fromOffset(30, 30)
	closeBtn.Position = UDim2.fromOffset(285, 0)
	closeBtn.Parent = bottomFrame
	closeBtn.MouseButton1Click:Connect(function()
		if minesGui then minesGui.Enabled = false end
		if Toggles.MinesEnabled then Toggles.MinesEnabled:SetValue(false) end
	end)
end

local function updateMinesUI()
	if not minesGui or not minesGui.Parent then return end
	local info = minesGui:FindFirstChildWhichIsA("Frame"):FindFirstChild("InfoLabel", true)
	if info then
		info.Text = "Mines: " .. minesState.mineCount .. "  |  Revealed: " .. minesState.revealed .. "  |  Streak: " .. minesState.streak
	end
	if winsLabel then winsLabel:SetText("Wins: " .. minesState.wins) end
	if lossesLabel then lossesLabel:SetText("Losses: " .. minesState.losses) end
	if streakLabel then streakLabel:SetText("Streak: " .. minesState.streak) end
	if revealedLabel then revealedLabel:SetText("Revealed: " .. minesState.revealed .. " / " .. (25 - minesState.mineCount)) end
end

local function revealTile(idx)
	if minesState.status ~= "playing" then return end
	local btn = minesState.buttons[idx]
	local tile = minesState.grid[idx]
	if not btn or not tile or btn.Revealed then return end
	btn.Revealed = true
	minesState.revealed += 1

	if tile then
		btn.Text = "\240\159\146\163"
		btn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
		minesState.status = "gameover"
		minesState.losses += 1
		minesState.streak = 0
		for i, b in ipairs(minesState.buttons) do
			if minesState.grid[i] and not b.Revealed then
				b.Text = "\240\159\146\163"
				b.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
				b.Revealed = true
			end
		end
		Library:Notify("Mines - Hit a mine! Lost", 3)
	else
		btn.Text = "\226\173\144"
		btn.BackgroundColor3 = Color3.fromRGB(50, 180, 50)
		minesState.streak += 1
		if minesState.revealed >= 25 - minesState.mineCount then
			minesState.status = "win"
			minesState.wins += 1
			Library:Notify("Mines - Cleared all safe tiles! Won $" .. (minesState.revealed * 100), 3)
		end
	end
	updateMinesUI()
end

function startMinesGame()
	if not minesGui then createMinesUI() end
	minesGui.Enabled = true
	minesState.mineCount = Options.MinesCount and Options.MinesCount.Value or 3
	minesState.revealed = 0
	minesState.status = "playing"
	minesState.grid = {}
	minesState.buttons = minesState.buttons or {}

	local indices = {}
	for i = 1, 25 do indices[i] = false end
	local placed = 0
	while placed < minesState.mineCount do
		local r = math.random(1, 25)
		if not indices[r] then indices[r] = true; placed += 1 end
	end
	for i = 1, 25 do minesState.grid[i] = indices[i] end

	for i, btn in ipairs(minesState.buttons) do
		if btn then
			btn.Text = "?"
			btn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
			btn.Revealed = false
		end
	end
	updateMinesUI()
	Library:Notify("Mines - New game! " .. minesState.mineCount .. " mines", 2)
end

function cashOutMines()
	if minesState.status ~= "playing" or minesState.revealed == 0 then return end
	local winnings = minesState.revealed * 100
	minesState.wins += 1
	minesState.status = "cashedout"
	Library:Notify("Mines - Cashed out $" .. winnings .. "! (" .. minesState.revealed .. " safe)", 3)
	for i, btn in ipairs(minesState.buttons) do
		if btn and not btn.Revealed then
			btn.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
		end
	end
	updateMinesUI()
end

-- Name Changer
local nameChangerGui
local nameHookEnabled = false
local customDisplayName = LP.DisplayName
local customUserName = LP.Name

local function setupNameHook()
	if nameHookEnabled then return end
	nameHookEnabled = true
end

local function createNameChangerUI()
	if nameChangerGui then nameChangerGui:Destroy() end
	nameChangerGui = Instance.new("ScreenGui")
	nameChangerGui.Name = "NameChanger"
	nameChangerGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
	nameChangerGui.ResetOnSpawn = false
	nameChangerGui.Parent = LP:WaitForChild("PlayerGui")
	nameChangerGui.Enabled = false

	local frame = Instance.new("Frame")
	frame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
	frame.BorderSizePixel = 0
	frame.Size = UDim2.fromOffset(280, 180)
	frame.Position = UDim2.fromScale(0.5, 0.5)
	frame.AnchorPoint = Vector2.new(0.5, 0.5)
	frame.Active = true
	frame.Draggable = true
	frame.Name = "Frame"
	frame.Parent = nameChangerGui

	local title = Instance.new("TextLabel")
	title.Text = "Name Changer"
	title.TextColor3 = Color3.fromRGB(255, 255, 255)
	title.TextSize = 16
	title.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
	title.BorderSizePixel = 0
	title.Size = UDim2.new(1, 0, 0, 30)
	title.Parent = frame

	local dispLabel = Instance.new("TextLabel")
	dispLabel.Text = "Display Name:"
	dispLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
	dispLabel.TextSize = 13
	dispLabel.BackgroundTransparency = 1
	dispLabel.Size = UDim2.fromOffset(120, 24)
	dispLabel.Position = UDim2.fromOffset(10, 40)
	dispLabel.TextXAlignment = Enum.TextXAlignment.Left
	dispLabel.Parent = frame

	local dispBox = Instance.new("TextBox")
	dispBox.Text = LP.DisplayName
	dispBox.TextColor3 = Color3.fromRGB(255, 255, 255)
	dispBox.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
	dispBox.BorderSizePixel = 0
	dispBox.Size = UDim2.fromOffset(140, 24)
	dispBox.Position = UDim2.fromOffset(130, 40)
	dispBox.TextSize = 13
	dispBox.ClearTextOnFocus = false
	dispBox.Name = "DispBox"
	dispBox.Parent = frame

	local userLabel = Instance.new("TextLabel")
	userLabel.Text = "Username:"
	userLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
	userLabel.TextSize = 13
	userLabel.BackgroundTransparency = 1
	userLabel.Size = UDim2.fromOffset(120, 24)
	userLabel.Position = UDim2.fromOffset(10, 74)
	userLabel.TextXAlignment = Enum.TextXAlignment.Left
	userLabel.Parent = frame

	local userBox = Instance.new("TextBox")
	userBox.Text = LP.Name
	userBox.TextColor3 = Color3.fromRGB(255, 255, 255)
	userBox.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
	userBox.BorderSizePixel = 0
	userBox.Size = UDim2.fromOffset(140, 24)
	userBox.Position = UDim2.fromOffset(130, 74)
	userBox.TextSize = 13
	userBox.ClearTextOnFocus = false
	userBox.Name = "UserBox"
	userBox.Parent = frame

	local applyBtn = Instance.new("TextButton")
	applyBtn.Text = "Apply"
	applyBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
	applyBtn.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
	applyBtn.BorderSizePixel = 0
	applyBtn.Size = UDim2.fromOffset(100, 28)
	applyBtn.Position = UDim2.fromOffset(10, 115)
	applyBtn.TextSize = 14
	applyBtn.Parent = frame
	applyBtn.MouseButton1Click:Connect(function()
		customDisplayName = dispBox.Text
		customUserName = userBox.Text
		if nameStatusLabel then nameStatusLabel:SetText("Display: " .. customDisplayName) end
		if usernameStatusLabel then usernameStatusLabel:SetText("Username: " .. customUserName) end
		setupNameHook()
		nameChangerGui.Enabled = false
		Library:Notify("Name changed! Display: " .. customDisplayName .. " | User: " .. customUserName, 3)
	end)

	local closeBtn = Instance.new("TextButton")
	closeBtn.Text = "X"
	closeBtn.TextColor3 = Color3.fromRGB(255, 100, 100)
	closeBtn.TextSize = 14
	closeBtn.BackgroundTransparency = 1
	closeBtn.Size = UDim2.fromOffset(24, 24)
	closeBtn.Position = UDim2.fromOffset(250, 3)
	closeBtn.Parent = frame
	closeBtn.MouseButton1Click:Connect(function()
		nameChangerGui.Enabled = false
		if Toggles.NameChanger then Toggles.NameChanger:SetValue(false) end
	end)
end

Toggles.NameChanger:OnChanged(function()
	if Toggles.NameChanger.Value then
		if not nameChangerGui then createNameChangerUI() end
		nameChangerGui.Enabled = true
		setupNameHook()
	else
		if nameChangerGui then nameChangerGui.Enabled = false end
	end
end)

local function backupLightingSettings()
	if originalLightingSettings then return end
	originalLightingSettings = {
		ClockTime = Lighting.ClockTime,
		OutdoorAmbient = Lighting.OutdoorAmbient,
		Ambient = Lighting.Ambient,
		GeographicLatitude = Lighting.GeographicLatitude
	}
end

local function restoreLightingSettings()
	if not originalLightingSettings then return end
	Lighting.ClockTime = originalLightingSettings.ClockTime
	Lighting.OutdoorAmbient = originalLightingSettings.OutdoorAmbient
	Lighting.Ambient = originalLightingSettings.Ambient
	Lighting.GeographicLatitude = originalLightingSettings.GeographicLatitude
	originalLightingSettings = nil
end

local function backupOriginalSky()
	if skyInstance then return end
	for _, child in ipairs(Lighting:GetChildren()) do
		if child:IsA("Sky") and child.Name ~= "CustomSkybox" then
			skyInstance = child:Clone()
			break
		end
	end
end

local function restoreOriginalSky()
	if customSkyInstance then
		pcall(function() customSkyInstance:Destroy() end)
		customSkyInstance = nil
	end
	for _, child in ipairs(Lighting:GetChildren()) do
		if child:IsA("Sky") then
			pcall(function() child:Destroy() end)
		end
	end
	if skyInstance then
		local restored = skyInstance:Clone()
		restored.Parent = Lighting
	end
end

local function updateSkybox()
	if not Toggles.SkyboxEnabled or not Toggles.SkyboxEnabled.Value or (Toggles.NoSky and Toggles.NoSky.Value) then
		if customSkyInstance then
			pcall(function() customSkyInstance:Destroy() end)
			customSkyInstance = nil
		end
		if not Toggles.SkyboxEnabled.Value and (not Toggles.NoSky or not Toggles.NoSky.Value) then
			restoreOriginalSky()
			restoreLightingSettings()
		end
		return
	end

	backupOriginalSky()
	backupLightingSettings()

	for _, child in ipairs(Lighting:GetChildren()) do
		if child:IsA("Sky") and child ~= customSkyInstance then
			pcall(function() child:Destroy() end)
		end
	end

	if not customSkyInstance or not customSkyInstance.Parent then
		customSkyInstance = Instance.new("Sky")
		customSkyInstance.Name = "CustomSkybox"
		customSkyInstance.Parent = Lighting
	end

	local preset = Options.SkyboxPreset and Options.SkyboxPreset.Value or "Purple Nebula"
	local bk, dn, ft, lf, rt, up = "", "", "", "", "", ""

	if preset == "Purple Nebula" then
		bk = "rbxassetid://159454299"
		dn = "rbxassetid://159454286"
		ft = "rbxassetid://159454296"
		lf = "rbxassetid://159454293"
		rt = "rbxassetid://159454300"
		up = "rbxassetid://159454288"
		Lighting.ClockTime = 22
		Lighting.OutdoorAmbient = Color3.fromRGB(40, 20, 60)
	elseif preset == "Warm Sunset" then
		bk = "rbxassetid://600886090"
		dn = "rbxassetid://600886134"
		ft = "rbxassetid://600886159"
		lf = "rbxassetid://600886196"
		rt = "rbxassetid://600886240"
		up = "rbxassetid://600886283"
		Lighting.ClockTime = 17.8
		Lighting.OutdoorAmbient = Color3.fromRGB(80, 50, 40)
	elseif preset == "Space" then
		bk = "rbxassetid://120616147"
		dn = "rbxassetid://120616174"
		ft = "rbxassetid://120616130"
		lf = "rbxassetid://120616157"
		rt = "rbxassetid://120616167"
		up = "rbxassetid://120616120"
		Lighting.ClockTime = 0
		Lighting.OutdoorAmbient = Color3.fromRGB(20, 20, 30)
	elseif preset == "Blue Clouds" then
		bk = "rbxassetid://252760975"
		dn = "rbxassetid://252763035"
		ft = "rbxassetid://252761405"
		lf = "rbxassetid://252761162"
		rt = "rbxassetid://252761300"
		up = "rbxassetid://252762777"
		Lighting.ClockTime = 12
		Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
	elseif preset == "Aesthetic Sakura" then
		bk = "rbxassetid://271042519"
		dn = "rbxassetid://271042306"
		ft = "rbxassetid://271042456"
		lf = "rbxassetid://271042358"
		rt = "rbxassetid://271042565"
		up = "rbxassetid://271042407"
		Lighting.ClockTime = 16
		Lighting.OutdoorAmbient = Color3.fromRGB(85, 60, 75)
	elseif preset == "Starry Night" then
		bk = "rbxassetid://208691316"
		dn = "rbxassetid://208691152"
		ft = "rbxassetid://208691238"
		lf = "rbxassetid://208691185"
		rt = "rbxassetid://208691288"
		up = "rbxassetid://208691211"
		Lighting.ClockTime = 0
		Lighting.OutdoorAmbient = Color3.fromRGB(15, 15, 25)
	elseif preset == "Red Aurora" then
		bk = "rbxassetid://474328227"
		dn = "rbxassetid://474328003"
		ft = "rbxassetid://474328135"
		lf = "rbxassetid://474328054"
		rt = "rbxassetid://474328285"
		up = "rbxassetid://474328091"
		Lighting.ClockTime = 22
		Lighting.OutdoorAmbient = Color3.fromRGB(45, 20, 35)
	elseif preset == "Custom" then
		local customID = Options.SkyboxCustomID and Options.SkyboxCustomID.Value or ""
		if customID ~= "" then
			if not customID:find("rbxassetid://") and tonumber(customID) then
				customID = "rbxassetid://" .. customID
			end
			bk, dn, ft, lf, rt, up = customID, customID, customID, customID, customID, customID
		end
	end

	if bk ~= "" then customSkyInstance.SkyboxBk = bk end
	if dn ~= "" then customSkyInstance.SkyboxDn = dn end
	if ft ~= "" then customSkyInstance.SkyboxFt = ft end
	if lf ~= "" then customSkyInstance.SkyboxLf = lf end
	if rt ~= "" then customSkyInstance.SkyboxRt = rt end
	if up ~= "" then customSkyInstance.SkyboxUp = up end
end

Toggles.SkyboxEnabled:OnChanged(updateSkybox)
Options.SkyboxPreset:OnChanged(updateSkybox)
Options.SkyboxCustomID:OnChanged(updateSkybox)
Toggles.AestheticVM:OnChanged(function()
	_G.AestheticVMEnabled = Toggles.AestheticVM.Value
	if not Toggles.AestheticVM.Value then
		lazyLoad()
		if CameraHandler and CameraHandler.setBaseFOV then
			CameraHandler:setBaseFOV(80)
		end
	end
end)

Toggles.NoSky:OnChanged(function()
	if Toggles.NoSky.Value then
		backupOriginalSky()
		for _, child in ipairs(Lighting:GetChildren()) do
			if child:IsA("Sky") then
				pcall(function() child:Destroy() end)
			end
		end
		if customSkyInstance then pcall(function() customSkyInstance:Destroy() end) customSkyInstance = nil end
	else
		if Toggles.SkyboxEnabled and Toggles.SkyboxEnabled.Value then
			updateSkybox()
		else
			restoreOriginalSky()
		end
	end
end)

Toggles.XRay:OnChanged(function()
	if not Toggles.XRay.Value then
		for _, v in ipairs(workspace:GetDescendants()) do
			if v:IsA("BasePart") then pcall(function() v.LocalTransparencyModifier = 0 end) end
		end
	end
end)

Toggles.MinesEnabled:OnChanged(function()
	if Toggles.MinesEnabled.Value then
		if not minesGui then createMinesUI() end
		minesGui.Enabled = true
		if minesState.status == "idle" then startMinesGame() end
	else
		if minesGui then minesGui.Enabled = false end
	end
end)

do
local defaultSoundIds = {}
Toggles.CustomHitSFX:OnChanged(function()
	pcall(function()
		local enabled = Toggles.CustomHitSFX.Value
		local replicatedStorage = game:GetService("ReplicatedStorage")
		local hitMarkerFolder = replicatedStorage:FindFirstChild("Assets")
			and replicatedStorage.Assets:FindFirstChild("SFX")
			and replicatedStorage.Assets.SFX:FindFirstChild("Misc")
			and replicatedStorage.Assets.SFX.Misc:FindFirstChild("Gun")
			and replicatedStorage.Assets.SFX.Misc.Gun:FindFirstChild("HitMarker")
			
		if not hitMarkerFolder then return end
		
		local body = hitMarkerFolder:FindFirstChild("HitMarkerBody1")
		local head = hitMarkerFolder:FindFirstChild("HitMarkerHead1")
		local kill = hitMarkerFolder:FindFirstChild("HitMarkerKill1")
		
		if enabled then
			if body then
				defaultSoundIds.Body = defaultSoundIds.Body or body.SoundId
				body.SoundId = "rbxassetid://8766809464"
			end
			if head then
				defaultSoundIds.Head = defaultSoundIds.Head or head.SoundId
				head.SoundId = "rbxassetid://198598793"
			end
			if kill then
				defaultSoundIds.Kill = defaultSoundIds.Kill or kill.SoundId
				kill.SoundId = "rbxassetid://135097031120155"
			end
		else
			if body and defaultSoundIds.Body then
				body.SoundId = defaultSoundIds.Body
			end
			if head and defaultSoundIds.Head then
				head.SoundId = defaultSoundIds.Head
			end
			if kill and defaultSoundIds.Kill then
				kill.SoundId = defaultSoundIds.Kill
			end
		end
	end)
end)

local funCrosshairGui = nil
local funCrosshairConnection = nil

local function setGameCrosshairVisible(visible)
	pcall(function()
		local gameCrosshair = LP.PlayerGui:FindFirstChild("Crosshair")
		if gameCrosshair then
			local target = gameCrosshair:FindFirstChild("Target")
			if target then
				target.Visible = visible
			end
		end
	end)
end

local function updateFunCrosshair(dt)
	if not Toggles.FunCrosshairEnabled or not Toggles.FunCrosshairEnabled.Value then
		if funCrosshairGui then
			funCrosshairGui:Destroy()
			funCrosshairGui = nil
		end
		if funCrosshairConnection then
			funCrosshairConnection:Disconnect()
			funCrosshairConnection = nil
		end
		setGameCrosshairVisible(true)
		return
	end
	
	setGameCrosshairVisible(false)
	
	local color = Options.FunCrosshairColor and Options.FunCrosshairColor.Value or Color3.fromRGB(255, 0, 120)
	local size = Options.FunCrosshairSize and Options.FunCrosshairSize.Value or 20
	local gap = Options.FunCrosshairGap and Options.FunCrosshairGap.Value or 5
	local speed = Options.FunCrosshairSpeed and Options.FunCrosshairSpeed.Value or 60
	local thickness = Options.FunCrosshairThickness and Options.FunCrosshairThickness.Value or 1
	local pulseEnabled = Toggles.FunCrosshairPulse and Toggles.FunCrosshairPulse.Value or false
	local pulseSpeed = Options.FunCrosshairPulseSpeed and Options.FunCrosshairPulseSpeed.Value or 5
	local pulseSize = Options.FunCrosshairPulseSize and Options.FunCrosshairPulseSize.Value or 4
	
	local currentGap = gap
	if pulseEnabled then
		currentGap = gap + math.sin(tick() * pulseSpeed) * pulseSize
		if currentGap < 0 then currentGap = 0 end
	end
	
	if not funCrosshairGui then
		local container = gethui and gethui() or game:GetService("CoreGui")
		funCrosshairGui = Instance.new("ScreenGui")
		funCrosshairGui.Name = "FunAmmoCrosshairGui"
		funCrosshairGui.ResetOnSpawn = false
		funCrosshairGui.IgnoreGuiInset = true
		funCrosshairGui.Parent = container
		
		local centerFrame = Instance.new("Frame")
		centerFrame.Name = "CenterContainer"
		centerFrame.AnchorPoint = Vector2.new(0.5, 0.5)
		centerFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
		centerFrame.BackgroundTransparency = 1
		centerFrame.Parent = funCrosshairGui
		
		local lines = { "Top", "Bottom", "Left", "Right" }
		for _, name in ipairs(lines) do
			local f = Instance.new("Frame")
			f.Name = name .. "Line"
			f.BorderSizePixel = 0
			f.AnchorPoint = Vector2.new(0.5, 0.5)
			f.Parent = centerFrame
		end
		
		local ammoText = Instance.new("TextLabel")
		ammoText.Name = "AmmoDisplay"
		ammoText.AnchorPoint = Vector2.new(0.5, 0.5)
		ammoText.BackgroundTransparency = 1
		ammoText.Font = Enum.Font.GothamBold
		ammoText.TextSize = 14
		ammoText.TextStrokeTransparency = 0
		ammoText.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
		ammoText.Parent = funCrosshairGui
	end
	
	local centerFrame = funCrosshairGui:FindFirstChild("CenterContainer")
	local ammoText = funCrosshairGui:FindFirstChild("AmmoDisplay")
	
	if centerFrame and ammoText then
		centerFrame.Size = UDim2.new(0, size, 0, size)
		centerFrame.Rotation = (centerFrame.Rotation + speed * dt) % 360
		
		local lineLength = (size - currentGap * 2) / 2
		if lineLength < 1 then lineLength = 1 end
		
		local top = centerFrame:FindFirstChild("TopLine")
		if top then
			top.BackgroundColor3 = color
			top.Size = UDim2.new(0, thickness, 0, lineLength)
			top.Position = UDim2.new(0.5, 0, 0.5, -currentGap - lineLength/2)
		end
		
		local bottom = centerFrame:FindFirstChild("BottomLine")
		if bottom then
			bottom.BackgroundColor3 = color
			bottom.Size = UDim2.new(0, thickness, 0, lineLength)
			bottom.Position = UDim2.new(0.5, 0, 0.5, currentGap + lineLength/2)
		end
		
		local left = centerFrame:FindFirstChild("LeftLine")
		if left then
			left.BackgroundColor3 = color
			left.Size = UDim2.new(0, lineLength, 0, thickness)
			left.Position = UDim2.new(0.5, -currentGap - lineLength/2, 0.5, 0)
		end
		
		local right = centerFrame:FindFirstChild("RightLine")
		if right then
			right.BackgroundColor3 = color
			right.Size = UDim2.new(0, lineLength, 0, thickness)
			right.Position = UDim2.new(0.5, currentGap + lineLength/2, 0.5, 0)
		end
		
		-- Update ammo text
		ammoText.TextColor3 = color
		ammoText.Position = UDim2.new(0.5, 0, 0.5, -size/2 - 15)
		
		local magCount, reserveCount = 0, 0
		local chickynoid = _G.LocalChickynoidInstance
		if chickynoid and chickynoid.simulation and chickynoid.simulation.state then
			local state = chickynoid.simulation.state
			local slot = state.equippedSlot or 1
			magCount = state["slot" .. slot .. "_magCount"] or 0
			reserveCount = state["slot" .. slot .. "_reserveCount"] or 0
			ammoText.Text = tostring(magCount) .. " / " .. tostring(reserveCount)
		else
			ammoText.Text = "- / -"
		end
	end
end

local function setupFunCrosshairHook()
	if funCrosshairConnection then return end
	funCrosshairConnection = game:GetService("RunService").RenderStepped:Connect(updateFunCrosshair)
end

Toggles.FunCrosshairEnabled:OnChanged(function()
	if Toggles.FunCrosshairEnabled.Value then
		setupFunCrosshairHook()
	else
		if funCrosshairGui then
			funCrosshairGui:Destroy()
			funCrosshairGui = nil
		end
		if funCrosshairConnection then
			funCrosshairConnection:Disconnect()
			funCrosshairConnection = nil
		end
		setGameCrosshairVisible(true)
	end
end)
end

do
	local WING_ID = 493489765
	local HAT_ID = 86301034324090
	local hatSpeed = 360
	local hatAngle = 0
	local wingData, hatData = nil, nil
	local isActive = false
	local hatRotConnection = nil
	
	local BODY_PARTS = {
		"Head", "Torso",
		"LeftArm", "RightArm", "LeftLeg", "RightLeg",
		"Left Arm", "Right Arm", "Left Leg", "Right Leg", 
		"UpperTorso", "LowerTorso",
		"LeftUpperArm", "LeftLowerArm", "RightUpperArm", "RightLowerArm",
		"LeftUpperLeg", "LeftLowerLeg", "RightUpperLeg", "RightLowerLeg",
		"LeftHand", "RightHand", "LeftFoot", "RightFoot",
	}
	
	local function isBodyPart(name)
		for _, v in ipairs(BODY_PARTS) do
			if v == name then return true end
		end
		return false
	end

	local function getLocalCharacter()
		local cam = workspace.CurrentCamera
		if not cam then return LP.Character end
		local subject = cam.CameraSubject
		if not subject then return LP.Character end
		
		if subject:IsA("Humanoid") then
			return subject.Parent
		elseif subject:IsA("BasePart") then
			return subject:FindFirstAncestorOfClass("Model")
		end
		return LP.Character
	end

	local function loadAccessory(id, char, parent, pos, rot, scale)
		if not char or not parent then return nil end
		local handle = nil
		local success, objects = pcall(function()
			return game:GetObjects("rbxassetid://" .. id)
		end)
		if success and objects then
			for _, obj in ipairs(objects) do
				if obj:IsA("Accessory") then
					handle = obj:FindFirstChild("Handle")
					if handle then
						handle.Parent = nil
						break
					end
				end
				pcall(function() obj:Destroy() end)
			end
		end
		if not handle then return nil end
		handle.Anchored = false
		handle.CanCollide = false
		handle.Massless = true
		handle.Parent = char 
		local weld = Instance.new("Weld")
		weld.Part0 = parent
		weld.Part1 = handle
		weld.C0 = CFrame.new(pos) * CFrame.Angles(math.rad(rot.X), math.rad(rot.Y), math.rad(rot.Z))
		weld.Parent = handle
		handle.Size = handle.Size * scale
		for _, c in ipairs(handle:GetDescendants()) do
			if c:IsA("SpecialMesh") then
				c.Scale = c.Scale * scale
			end
		end
		return { handle = handle, weld = weld }
	end

	local function clearAccessory(data)
		if not data then return end
		pcall(function() if data.weld then data.weld:Destroy() end end)
		pcall(function() if data.handle then data.handle:Destroy() end end)
	end

	local function loadWing(char)
		clearAccessory(wingData)
		if not char then return end
		local parent = char:FindFirstChild("UpperTorso") or char:FindFirstChild("Torso") or char:FindFirstChild("HumanoidRootPart")
		wingData = loadAccessory(WING_ID, char, parent, Vector3.new(0,0,0), Vector3.new(0,0,0), 1)
	end

	local function loadHat(char)
		clearAccessory(hatData)
		if not char then return end
		local parent = char:FindFirstChild("Head")
		hatData = loadAccessory(HAT_ID, char, parent, Vector3.new(0,2,0), Vector3.new(0,0,0), 2)
		hatAngle = 0
	end

	local function applyFullEffect(char)
		if not char then return end
		for _, child in ipairs(char:GetChildren()) do
			if child:IsA("Highlight") and child.Name == "PlayerOutlineHighlight" then
				child:Destroy()
			end
		end
		local h = Instance.new("Highlight")
		h.Name = "PlayerOutlineHighlight"
		h.FillTransparency = 1.0
		h.OutlineColor = Color3.new(0,0,0)
		h.OutlineTransparency = 0.0
		h.Parent = char

		for _, part in ipairs(char:GetChildren()) do
			if part:IsA("BasePart") and isBodyPart(part.Name) then
				part.Transparency = 0.99
			end
		end
	end

	local function disableEffect()
		isActive = false
		if hatRotConnection then
			hatRotConnection:Disconnect()
			hatRotConnection = nil
		end
		clearAccessory(wingData)
		clearAccessory(hatData)
		wingData = nil
		hatData = nil
		
		local char = getLocalCharacter()
		if char then
			local highlight = char:FindFirstChild("PlayerOutlineHighlight")
			if highlight then
				highlight:Destroy()
			end
			for _, part in ipairs(char:GetChildren()) do
				if part:IsA("BasePart") and isBodyPart(part.Name) then
					part.Transparency = 0
				end
			end
		end
		
		pcall(function()
			local cam = workspace.CurrentCamera
			if cam then
				local vmFov = Options.VMFOV and Options.VMFOV.Value or 80
				cam.FieldOfView = vmFov
			end
		end)
	end

	local function enableEffect()
		isActive = true
		
		if not hatRotConnection then
			hatRotConnection = game:GetService("RunService").RenderStepped:Connect(function(dt)
				if hatData and hatData.weld and hatData.weld.Parent then
					hatAngle = (hatAngle + hatSpeed * dt) % 360
					hatData.weld.C0 = CFrame.new(0,2,0) * CFrame.Angles(0, math.rad(hatAngle), 0)
				end
			end)
		end
		
		task.spawn(function()
			local lastChar = nil
			while isActive do
				local char = getLocalCharacter()
				if char and char:FindFirstChildOfClass("Humanoid") then
					local isNew = (char ~= lastChar)
					local wingOk = wingData and wingData.handle and wingData.handle.Parent == char
					local hatOk = hatData and hatData.handle and hatData.handle.Parent == char
					
					if isNew or not wingOk or not hatOk then
						lastChar = char
						task.wait(0.3) 
						if not isActive then break end
						loadWing(char)
						loadHat(char)
						applyFullEffect(char)
					else
						for _, part in ipairs(char:GetChildren()) do
							if part:IsA("BasePart") and isBodyPart(part.Name) and part.Transparency < 0.9 then
								part.Transparency = 0.99
							end
						end
						if not char:FindFirstChild("PlayerOutlineHighlight") then
							applyFullEffect(char)
						end
					end
				else
					lastChar = nil
				end
				task.wait(0.5)
			end
		end)

		task.spawn(function()
			while isActive do
				pcall(function()
					local cam = workspace.CurrentCamera
					if cam then
						cam.FieldOfView = 120
					end
				end)
				task.wait(1)
			end
		end)
	end

	Toggles.FunCharEffects:OnChanged(function()
		if Toggles.FunCharEffects.Value then
			enableEffect()
		else
			disableEffect()
		end
	end)
end

-- Silent Aim disabled: Camera metatable hook triggers anti-cheat
-- ===== ADVANCED FEATURES =====

-- Aspect ratio stretch (CFrame matrix method)
local stretchHooked = false
local function setupStretchHook()
	if stretchHooked then return end
	lazyLoad()
	if not CameraHandler then
		task.delay(1, setupStretchHook)
		return
	end
	stretchHooked = true
	CameraHandler:addPostUpdateHook(function()
		if Toggles.AspectEnabled and Toggles.AspectEnabled.Value then
			local rx = Options.AspectX and Options.AspectX.Value or 1
			local ry = Options.AspectY and Options.AspectY.Value or 1
			if rx ~= 1 or ry ~= 1 then
				Camera.CFrame = Camera.CFrame * CFrame.new(0, 0, 0, rx, 0, 0, 0, ry, 0, 0, 0, 1)
			end
		end
	end)
end

local screenTiltHooked = false
local function setupScreenTilt()
	if screenTiltHooked then return end
	lazyLoad()
	if not CameraHandler then
		task.delay(1, setupScreenTilt)
		return
	end
	screenTiltHooked = true

	-- \230\151\139\232\189\137 + \231\184\174\230\148\190\229\133\168\233\131\168\230\148\190\229\156\168 addPostUpdateHook\239\188\140\232\136\135 AspectEnabled \231\155\184\229\144\140\230\169\159\229\136\182
	-- \233\128\153\230\168\163\231\184\174\230\148\190\231\159\169\233\153\163\230\137\141\228\184\141\230\156\131\232\162\171 Roblox \230\173\163\232\166\143\229\140\150\230\142\137
	CameraHandler:addPostUpdateHook(function()
		if not (Toggles.ScreenTiltEnabled and Toggles.ScreenTiltEnabled.Value) then return end

		local rx = math.rad(Options.ScreenTiltX and Options.ScreenTiltX.Value or 0)
		local ry = math.rad(Options.ScreenTiltY and Options.ScreenTiltY.Value or 0)
		local rz = math.rad(Options.ScreenTiltZ and Options.ScreenTiltZ.Value or 0)
		local sx = Options.ScreenTiltScaleX and Options.ScreenTiltScaleX.Value or 1
		local sy = Options.ScreenTiltScaleY and Options.ScreenTiltScaleY.Value or 1

		local cf = Camera.CFrame

		-- \230\151\139\232\189\137\239\188\136\229\133\136\229\165\151\239\188\137
		if rx ~= 0 or ry ~= 0 or rz ~= 0 then
			cf = cf * CFrame.Angles(rx, ry, rz)
		end

		-- \231\184\174\230\148\190 X/Y\239\188\154\229\144\140 AspectEnabled \229\174\140\229\133\168\228\184\128\230\168\163\231\154\132\229\175\171\230\179\149
		-- CFrame.new(0,0,0, sx,0,0, 0,sy,0, 0,0,1) = right \232\187\184\195\151sx\239\188\140up \232\187\184\195\151sy
		if sx ~= 1 or sy ~= 1 then
			cf = cf * CFrame.new(0, 0, 0,  sx, 0, 0,  0, sy, 0,  0, 0, 1)
		end

		Camera.CFrame = cf
	end)

	-- \229\130\153\231\148\168\239\188\154BindToRenderStep \229\143\170\229\129\154\230\151\139\232\189\137\239\188\136\232\139\165 CameraHandler \231\137\136\228\184\141\232\181\183\228\189\156\231\148\168\239\188\137
	-- RunService:BindToRenderStep("ScreenTilt_Camera", Enum.RenderPriority.Camera.Value + 2, function() end)
end



local thirdPersonHooked = false
local function setupThirdPersonHook()
	if thirdPersonHooked then return end
	lazyLoad()
	if not CameraHandler then
		task.delay(1, setupThirdPersonHook)
		return
	end
	thirdPersonHooked = true
	
	-- Enforce third person camera offset and part visibility inside CameraHandler's post update hook (Priority: Camera)
	CameraHandler:addPostUpdateHook(function(dt)
		if Toggles.ThirdPersonEnabled and Toggles.ThirdPersonEnabled.Value then
			-- Rebuild VMCs and enable third-person animations for hands and weapon
			if not CameraHandler.thirdPersonDebug then
				CameraHandler.firstPerson = false
				pcall(function() CameraHandler:enterThirdPersonDebug() end)
				CameraHandler.firstPerson = true
			end
			
			local cam = workspace.CurrentCamera
			if not cam then return end
			
			local distance = Options.ThirdPersonDistance and Options.ThirdPersonDistance.Value or 10
			local offsetX = Options.ThirdPersonOffsetX and Options.ThirdPersonOffsetX.Value or 0
			
			local nMod = getNeuronModule()
			local char = LP.Character or (nMod and nMod:get_character(LP))
			
			-- Third person offset: custom horizontal offset, 1.2 studs vertical offset, and pushed back by distance
			local offset = Vector3.new(offsetX, 1.2, distance)
			local targetCFrame = cam.CFrame * CFrame.new(offset)
			local rayOrigin = cam.CFrame.Position
			local rayDirection = (targetCFrame.Position - rayOrigin)
			
			local ignoreList = {cam}
			if char then
				table.insert(ignoreList, char)
			end
			
			local raycastParams = RaycastParams.new()
			raycastParams.FilterType = Enum.RaycastFilterType.Exclude
			raycastParams.FilterDescendantsInstances = ignoreList
			
			local raycastResult = workspace:Raycast(rayOrigin, rayDirection, raycastParams)
			if raycastResult then
				local hitPos = raycastResult.Position
				local finalFraction = (hitPos - rayOrigin).Magnitude / rayDirection.Magnitude
				local finalOffset = offset * math.clamp(finalFraction - 0.05, 0, 1)
				cam.CFrame = cam.CFrame * CFrame.new(finalOffset)
			else
				cam.CFrame = targetCFrame
			end
			
			if char then
				for _, part in ipairs(char:GetDescendants()) do
					if part:IsA("BasePart") or part:IsA("Decal") then
						if part.Name ~= "HumanoidRootPart" and part.Name ~= "HeadCollider" and part.Name ~= "BoundingBody" and not part.Name:find("Hitbox") then
							part.LocalTransparencyModifier = 0
							if part.Transparency == 1 then
								part.Transparency = 0
							end
						end
					end
				end
			end
		else
			if CameraHandler.thirdPersonDebug then
				pcall(function() CameraHandler:exitThirdPersonDebug() end)
			end
		end
	end)
end
	-- Visual spin bot (local character model rotation) in RenderStepped
	game:GetService("RunService").RenderStepped:Connect(function()
		local char = LP.Character
		if Toggles.SpinBotEnabled and Toggles.SpinBotEnabled.Value and char and char.PrimaryPart then
			local isAiming = false
			local aimHeld = UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
			local aimKey = Options.AimbotKey
			if not aimHeld and aimKey and aimKey.Key then
				aimHeld = UserInputService:IsKeyDown(aimKey.Key)
			end
			if Toggles.AimbotEnabled and Toggles.AimbotEnabled.Value and aimHeld then
				isAiming = true
			end
			
			if not isAiming then
				local sSpeed = Options.SpinBotSpeed and Options.SpinBotSpeed.Value or 50
				local visualSpinAngle = (tick() * sSpeed) % (math.pi * 2)
				local currentPos = char.PrimaryPart.Position
				char.PrimaryPart.CFrame = CFrame.new(currentPos) * CFrame.Angles(0, visualSpinAngle, 0)
			end
		end
	end)

-- Hit overlay
local hitOverlayGui
local hitOverlayFrame
local function setupHitOverlay()
	if not hitOverlayGui then
		hitOverlayGui = Instance.new("ScreenGui")
		hitOverlayGui.Name = "HitOverlay"
		hitOverlayGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
		hitOverlayGui.ResetOnSpawn = false
		hitOverlayGui.Parent = LP:WaitForChild("PlayerGui")
		hitOverlayGui.Enabled = false

		hitOverlayFrame = Instance.new("Frame")
		hitOverlayFrame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
		hitOverlayFrame.BorderSizePixel = 0
		hitOverlayFrame.BackgroundTransparency = 1
		hitOverlayFrame.Size = UDim2.new(1, 0, 1, 0)
		hitOverlayFrame.Parent = hitOverlayGui
	end
end

-- Bullet tracers (3D beams)
local tracerFolder
local activeWireframes = 0
local maxActiveWireframes = 3

local function clearTracers()
	if tracerFolder then tracerFolder:ClearAllChildren() end
end

function playHitVisualEffect(toChar, headshot)
	local targetPart = headshot and toChar:FindFirstChild("Head") or toChar:FindFirstChild("UpperTorso") or toChar.PrimaryPart
	if not targetPart then return end
	local position = targetPart.Position
	
	local color = headshot and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(255, 50, 50) -- Gold for headshot, Red for body
	if Options.HitVisualColor then
		color = Options.HitVisualColor.Value
	end
	
	-- 1. Expanding Neon Sphere
	local sphere = Instance.new("Part")
	sphere.Shape = Enum.PartType.Ball
	sphere.Size = Vector3.new(0.5, 0.5, 0.5)
	sphere.Color = color
	sphere.Material = Enum.Material.Neon
	sphere.Anchored = true
	sphere.CanCollide = false
	sphere.Position = position
	sphere.Parent = workspace
	
	local TweenService = game:GetService("TweenService")
	local info = TweenInfo.new(0.4, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
	local tween = TweenService:Create(sphere, info, {
		Size = Vector3.new(4, 4, 4),
		Transparency = 1
	})
	tween:Play()
	tween.Completed:Connect(function()
		sphere:Destroy()
	end)
	
	-- 2. Flying Spark Particles
	local numParticles = 6
	for i = 1, numParticles do
		local p = Instance.new("Part")
		p.Size = Vector3.new(0.15, 0.15, 0.15)
		p.Color = color
		p.Material = Enum.Material.Neon
		p.Anchored = false
		p.CanCollide = false
		p.Position = position
		p.Parent = workspace
		
		-- Random velocity vector
		local vel = Vector3.new(
			math.random(-12, 12),
			math.random(-5, 15),
			math.random(-12, 12)
		)
		p.AssemblyLinearVelocity = vel
		
		task.spawn(function()
			local start = tick()
			local dur = 0.4
			while tick() - start < dur do
				local elapsed = tick() - start
				p.Transparency = elapsed / dur
				task.wait()
			end
			p:Destroy()
		end)
	end
end

local function drawTracer(origin, hitPos, color, thickness)
	if not tracerFolder then
		tracerFolder = Instance.new("Folder")
		tracerFolder.Name = "Tracers"
		tracerFolder.Parent = workspace
	end
	local dist = (hitPos - origin).Magnitude
	if dist < 1 then return end

	local tracerThickness = Options.TracersThickness and Options.TracersThickness.Value or thickness or 0.05
	local duration = Options.TracersDuration and Options.TracersDuration.Value or 0.5
	local startTrans = Options.TracersTransparency and Options.TracersTransparency.Value or 0
	local tracerType = Options.TracersType and Options.TracersType.Value or "Beam"

	if tracerType == "Wireframe" and activeWireframes >= maxActiveWireframes then
		tracerType = "Part" -- Fallback to standard Part tracer if too many wireframes are active to prevent lag
	end

	if tracerType == "Beam" then
		local a0 = Instance.new("Attachment")
		a0.Position = origin
		a0.Parent = workspace.Terrain

		local a1 = Instance.new("Attachment")
		a1.Position = hitPos
		a1.Parent = workspace.Terrain

		local beam = Instance.new("Beam")
		beam.Attachment0 = a0
		beam.Attachment1 = a1
		beam.Color = ColorSequence.new(color)
		beam.Width0 = tracerThickness
		beam.Width1 = tracerThickness
		beam.LightEmission = 1
		beam.LightInfluence = 0
		beam.FaceCamera = true
		beam.Transparency = NumberSequence.new(startTrans)
		beam.Parent = tracerFolder

		-- Use TweenService for smooth and performance-friendly fade out
		local numVal = Instance.new("NumberValue")
		numVal.Value = startTrans
		local connection
		connection = numVal.Changed:Connect(function(val)
			if beam and beam.Parent then
				beam.Transparency = NumberSequence.new(val)
			else
				connection:Disconnect()
			end
		end)

		local tween = game:GetService("TweenService"):Create(numVal, TweenInfo.new(duration, Enum.EasingStyle.Linear), {Value = 1})
		tween:Play()
		tween.Completed:Connect(function()
			connection:Disconnect()
			numVal:Destroy()
			beam:Destroy()
			a0:Destroy()
			a1:Destroy()
		end)
	elseif tracerType == "Part" then
		local mid = (origin + hitPos) / 2
		local p = Instance.new("Part")
		p.Size = Vector3.new(tracerThickness, tracerThickness, dist)
		p.CFrame = CFrame.lookAt(mid, hitPos) * CFrame.new(0, 0, dist / 2)
		p.Anchored = true
		p.CanCollide = false
		p.CanQuery = false
		p.CanTouch = false
		p.CastShadow = false
		p.Material = Enum.Material.Neon
		p.Color = color
		p.Transparency = startTrans
		p.Parent = tracerFolder

		-- Use TweenService for smooth and performance-friendly fade out
		local tween = game:GetService("TweenService"):Create(p, TweenInfo.new(duration, Enum.EasingStyle.Linear), {Transparency = 1})
		tween:Play()
		tween.Completed:Connect(function()
			p:Destroy()
		end)
	elseif tracerType == "Wireframe" then
		activeWireframes = activeWireframes + 1

		-- Draw the central main beam (thick black part to contrast the glowing wires)
		local mainBeam = Instance.new("Part")
		mainBeam.Size = Vector3.new(0.4, 0.4, dist)
		mainBeam.CFrame = CFrame.lookAt((origin + hitPos) / 2, hitPos)
		mainBeam.Anchored = true
		mainBeam.CanCollide = false
		mainBeam.CanQuery = false
		mainBeam.CanTouch = false
		mainBeam.CastShadow = false
		mainBeam.Material = Enum.Material.SmoothPlastic -- Solid matte black core
		mainBeam.Color = Color3.fromRGB(5, 5, 8)
		mainBeam.Transparency = 0
		mainBeam.Parent = tracerFolder

		-- Bright glowing color for wireframe and particles
		local wireColor = color:Lerp(Color3.new(1, 1, 1), 0.85)

		-- Particle emitter for the white smoke/glow effect
		local emitter = Instance.new("ParticleEmitter")
		emitter.Texture = "rbxassetid://252796148" -- Glow/smoke texture
		emitter.LightEmission = 0.9
		emitter.LightInfluence = 0
		emitter.Color = ColorSequence.new(wireColor)
		emitter.Size = NumberSequence.new({
			NumberSequenceKeypoint.new(0, 0.5),
			NumberSequenceKeypoint.new(0.5, 1.8),
			NumberSequenceKeypoint.new(1, 3.0)
		})
		emitter.Transparency = NumberSequence.new({
			NumberSequenceKeypoint.new(0, 0.1),
			NumberSequenceKeypoint.new(0.6, 0.4),
			NumberSequenceKeypoint.new(1, 1)
		})
		emitter.Lifetime = NumberRange.new(0.4, 0.8)
		emitter.Rate = 260
		emitter.Speed = NumberRange.new(1, 4)
		emitter.SpreadAngle = Vector2.new(-180, 180)
		emitter.Parent = mainBeam

		task.spawn(function()
			task.wait(0.06)
			emitter.Enabled = false -- Emit a quick burst along the trajectory then stop
		end)

		-- Wireframe math
		local dir = (hitPos - origin).Unit
		local u = dir:Cross(Vector3.new(0, 1, 0))
		if u.Magnitude < 0.001 then
			u = dir:Cross(Vector3.new(1, 0, 0))
		end
		u = u.Unit
		local v = dir:Cross(u).Unit

		local radius = 0.6 -- Radius of the wireframe cage
		local numSides = 4 -- 4 sides forms a square grid
		local segLength = 8 -- Optimized segment length (increased from 2.5 to 8 to reduce parts count significantly)
		local numSegments = math.clamp(math.floor(dist / segLength), 2, 8) -- Clamped to 2-8 segments max to prevent lag
		
		local vertices = {}
		for i = 0, numSegments do
			vertices[i] = {}
			local t = i / numSegments
			local center = origin + (hitPos - origin) * t
			for j = 1, numSides do
				local angle = (j - 1) * 2 * math.pi / numSides
				vertices[i][j] = center + u * (math.cos(angle) * radius) + v * (math.sin(angle) * radius)
			end
		end

		local parts = { mainBeam }
		local function addLine(p1, p2)
			local lineDist = (p2 - p1).Magnitude
			if lineDist < 0.01 then return end
			local p = Instance.new("Part")
			p.Size = Vector3.new(0.035, 0.035, lineDist) -- Fixed visible wire thickness
			p.CFrame = CFrame.lookAt((p1 + p2) / 2, p2)
			p.Anchored = true
			p.CanCollide = false
			p.CanQuery = false
			p.CanTouch = false
			p.CastShadow = false -- Huge optimization: turn off shadows for neon wires
			p.Material = Enum.Material.Neon
			p.Color = wireColor
			p.Transparency = startTrans
			p.Parent = tracerFolder
			table.insert(parts, p)
		end

		-- Draw the longitudinal lines (long edges)
		for i = 0, numSegments - 1 do
			for j = 1, numSides do
				addLine(vertices[i][j], vertices[i + 1][j])
			end
		end

		-- Draw the rings (cross-sections)
		for i = 0, numSegments do
			for j = 1, numSides do
				local nextJ = j % numSides + 1
				addLine(vertices[i][j], vertices[i][nextJ])
			end
		end

		-- Draw the double diagonal lines (forming 'X' truss patterns on all faces)
		for i = 0, numSegments - 1 do
			for j = 1, numSides do
				local nextJ = j % numSides + 1
				addLine(vertices[i][j], vertices[i + 1][nextJ])
				addLine(vertices[i][nextJ], vertices[i + 1][j])
			end
		end

		-- Single Tween to animate transparency on all parts simultaneously (super light weight)
		local numVal = Instance.new("NumberValue")
		numVal.Value = startTrans
		
		local connection
		connection = numVal.Changed:Connect(function(val)
			local progress = (val - startTrans) / (1 - startTrans)
			progress = math.clamp(progress, 0, 1)
			
			for _, part in ipairs(parts) do
				if part.Parent then
					if part == mainBeam then
						part.Transparency = progress
					else
						part.Transparency = val
					end
				end
			end
		end)

		local tween = game:GetService("TweenService"):Create(numVal, TweenInfo.new(duration, Enum.EasingStyle.Linear), {Value = 1})
		tween:Play()
		tween.Completed:Connect(function()
			connection:Disconnect()
			numVal:Destroy()
			for _, part in ipairs(parts) do
				if part.Parent then part:Destroy() end
			end
			activeWireframes = activeWireframes - 1
		end)
	end
end

-- Viewmodel chams storage
local originalProperties = {}
local worldOriginalProperties = {}
local partConnections = {}
local gameTransparency = {}
local updatingChams = false

local function checkAiming(vm)
	if vm.aiming ~= nil then return vm.aiming end
	if vm.ads ~= nil then return vm.ads end
	if vm.isAiming ~= nil then return vm.isAiming end
	if vm.aimSpring and vm.aimSpring._target then
		return vm.aimSpring._target > 0.5
	end
	if vm.aimProgress then return vm.aimProgress > 0.5 end

	local currentFOV = workspace.CurrentCamera.FieldOfView
	local baseFOV = 70
	if CameraHandler and CameraHandler.getBaseFOV then
		baseFOV = CameraHandler:getBaseFOV() or 70
	end
	if currentFOV < (baseFOV - 5) then
		return true
	end

	if game:GetService("UserInputService"):IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
		return true
	end

	return false
end

local function shouldIgnorePart(part)
	local name = part.Name:lower()
	if part.Transparency >= 0.95 then
		return true
	end
	if name:find("hitbox") or name:find("collider") or name:find("trigger") then
		return true
	end
	return false
end

local function cleanPart(part)
	if partConnections[part] then
		partConnections[part]:Disconnect()
		partConnections[part] = nil
	end
	gameTransparency[part] = nil
end

local function getViewModelParts(viewModel)
	local arms = {}
	local weapon = {}
	for _, part in ipairs(viewModel:GetDescendants()) do
		if part:IsA("BasePart") then
			local isArm = false
			local current = part
			while current and current ~= viewModel do
				local name = current.Name:lower()
				if name:find("arm") or name:find("hand") or name:find("glove") or name:find("sleeve") or name:find("limb") then
					isArm = true
					break
				end
				current = current.Parent
			end
			if isArm then
				table.insert(arms, part)
			else
				table.insert(weapon, part)
			end
		end
	end
	return arms, weapon
end

local function getPlayerWeapon(model)
	local tool = model:FindFirstChildOfClass("Tool")
	if tool then return tool.Name end
	for _, child in ipairs(model:GetChildren()) do
		if child:IsA("Model") and (child.Name:lower():find("gun") or child.Name:lower():find("weapon") or child.Name:lower():find("blade") or child.Name:lower():find("sword") or child.Name:lower():find("rifle")) then
			return child.Name
		end
	end
	return nil
end

local AssetService = game:GetService("AssetService")

local function applyWireframeToPart(part, color, alwaysOnTop)
	local wireBox = part:FindFirstChild("WireframeCham")
	if wireBox then
		if wireBox.Color3 ~= color or wireBox.AlwaysOnTop ~= alwaysOnTop then
			wireBox:Destroy()
			wireBox = nil
		end
	end
	if not wireBox then
		wireBox = Instance.new("WireframeHandleAdornment")
		wireBox.Name = "WireframeCham"
		wireBox.AlwaysOnTop = alwaysOnTop
		wireBox.Thickness = 1
		wireBox.Color3 = color
		wireBox.Adornee = part
		
		task.spawn(function()
			local meshId = part:IsA("MeshPart") and part.MeshId or (part:FindFirstChildWhichIsA("SpecialMesh") and part:FindFirstChildWhichIsA("SpecialMesh").MeshId)
			if not meshId or meshId == "" then
				-- Fallback to box wireframe if the part has no mesh data
				local S = part.Size
				local hx, hy, hz = S.X / 2, S.Y / 2, S.Z / 2
				local localVertices = {
					Vector3.new(-hx, -hy, -hz), Vector3.new(hx, -hy, -hz), Vector3.new(hx, hy, -hz), Vector3.new(-hx, hy, -hz),
					Vector3.new(-hx, -hy, hz), Vector3.new(hx, -hy, hz), Vector3.new(hx, hy, hz), Vector3.new(-hx, hy, hz)
				}
				wireBox:AddPath({localVertices[1], localVertices[2], localVertices[3], localVertices[4]}, true)
				wireBox:AddPath({localVertices[5], localVertices[6], localVertices[7], localVertices[8]}, true)
				wireBox:AddLine(localVertices[1], localVertices[5])
				wireBox:AddLine(localVertices[2], localVertices[6])
				wireBox:AddLine(localVertices[3], localVertices[7])
				wireBox:AddLine(localVertices[4], localVertices[8])
				return
			end
			
			local success, editableMesh = pcall(function()
				return AssetService:CreateEditableMeshAsync(meshId)
			end)
			if success and editableMesh then
				local faces = editableMesh:GetFaces()
				for _, faceId in ipairs(faces) do
					local vIds = editableMesh:GetFaceVertices(faceId)
					if vIds and #vIds >= 3 then
						local p1 = editableMesh:GetPosition(vIds[1])
						local p2 = editableMesh:GetPosition(vIds[2])
						local p3 = editableMesh:GetPosition(vIds[3])
						if p1 and p2 and p3 then
							wireBox:AddPath({p1, p2, p3}, true)
						end
					end
				end
				editableMesh:Destroy() -- Clean up memory mesh copy
			else
				-- Fallback to box wireframe if EditableMesh fails to create
				local S = part.Size
				local hx, hy, hz = S.X / 2, S.Y / 2, S.Z / 2
				local localVertices = {
					Vector3.new(-hx, -hy, -hz), Vector3.new(hx, -hy, -hz), Vector3.new(hx, hy, -hz), Vector3.new(-hx, hy, -hz),
					Vector3.new(-hx, -hy, hz), Vector3.new(hx, -hy, hz), Vector3.new(hx, hy, hz), Vector3.new(-hx, hy, hz)
				}
				wireBox:AddPath({localVertices[1], localVertices[2], localVertices[3], localVertices[4]}, true)
				wireBox:AddPath({localVertices[5], localVertices[6], localVertices[7], localVertices[8]}, true)
				wireBox:AddLine(localVertices[1], localVertices[5])
				wireBox:AddLine(localVertices[2], localVertices[6])
				wireBox:AddLine(localVertices[3], localVertices[7])
				wireBox:AddLine(localVertices[4], localVertices[8])
			end
		end)
		
		wireBox.Parent = part
	end
end

local function applyChamsToParts(parts, enabled, color, style, transparency)
	for _, part in ipairs(parts) do
		local wireBox = part:FindFirstChild("WireframeCham")
		if enabled then
			if shouldIgnorePart(part) and not originalProperties[part] then
				if wireBox then pcall(function() wireBox:Destroy() end) end
				continue
			end

			if not originalProperties[part] then
				originalProperties[part] = {
					Material = part.Material,
					Color = part.Color,
					Transparency = part.Transparency,
					TextureID = part:IsA("MeshPart") and part.TextureID or nil
				}
				gameTransparency[part] = part.Transparency

				partConnections[part] = part:GetPropertyChangedSignal("Transparency"):Connect(function()
					if updatingChams then return end
					gameTransparency[part] = part.Transparency
				end)
			end

			local currentGameTrans = gameTransparency[part] or 0
			pcall(function()
				if currentGameTrans >= 0.99 then
					updatingChams = true
					part.Transparency = 1
					updatingChams = false
					if wireBox then wireBox:Destroy() end
				else
					if style == "Wireframe" then
						applyWireframeToPart(part, color, true)
						updatingChams = true
						part.Material = Enum.Material.SmoothPlastic
						part.Color = Color3.fromRGB(5, 5, 8) -- Dark matte core
						part.Transparency = 0.85 -- Make it mostly hollow so the wireframe is very clear
						updatingChams = false
						if part:IsA("MeshPart") then
							part.TextureID = ""
						end
					else
						if wireBox then wireBox:Destroy() end
						updatingChams = true
						if style == "ForceField" then
							part.Material = Enum.Material.ForceField
						elseif style == "Neon" then
							part.Material = Enum.Material.Neon
						elseif style == "Glass" then
							part.Material = Enum.Material.Glass
						else
							part.Material = Enum.Material.SmoothPlastic
						end
						part.Color = color
						part.Transparency = transparency
						updatingChams = false

						if part:IsA("MeshPart") then
							part.TextureID = ""
						end
					end
				end
			end)
		else
			if wireBox then pcall(function() wireBox:Destroy() end) end
			local orig = originalProperties[part]
			if orig then
				pcall(function()
					part.Material = orig.Material
					part.Color = orig.Color
					updatingChams = true
					part.Transparency = orig.Transparency
					updatingChams = false
					if part:IsA("MeshPart") and orig.TextureID then
						part.TextureID = orig.TextureID
					end
				end)
				originalProperties[part] = nil
			end
			cleanPart(part)
		end
	end
end

-- Unlock all hook
local _unlockAllActive = false
local _unlockAllOrigGetDataAsync = nil
local _unlockAllOrigPktFire = nil
local _unlockAllHeartbeat = nil

local _uaMilk = nil
local _uaRS = game:GetService("ReplicatedStorage")
local _uaRF = game:GetService("ReplicatedFirst")
local _uaRunService = game:GetService("RunService")
local _uaHttpService = game:GetService("HttpService")
local _uaLP = game:GetService("Players").LocalPlayer

local _uaW2n, _uaNidToWName = {}, {}
local _uaConfigPath = "overkill_skins.json"
local _uaDefaults = {
	ak47 = "", shotgun = "", deagle = "",
	handgun = "", tec9 = "", katana = "",
	scythe = "", karambit = "", shorty = "",
	raygun = "", medkit = "", minigun = "",
	grenade = "", rpg = "", burst = "",
	pulse_rifle = "", bolt_action_sniper = "", tommy_gun = "",
}

local function _uaLoadConfig()
	local ok, data = pcall(readfile, _uaConfigPath)
	if ok and data and data ~= "" then
		local ok2, tbl = pcall(function() return _uaHttpService:JSONDecode(data) end)
		if ok2 and type(tbl) == "table" then return tbl end
	end
	return {}
end

local function _uaSaveConfig()
	local ok, json = pcall(function() return _uaHttpService:JSONEncode(getgenv().__UA_Equipped) end)
	if ok then pcall(writefile, _uaConfigPath, json) end
end

if not getgenv().__UA_Equipped then
	getgenv().__UA_Equipped = _uaLoadConfig()
	if next(getgenv().__UA_Equipped) == nil then
		for k, v in pairs(_uaDefaults) do getgenv().__UA_Equipped[k] = v end
		_uaSaveConfig()
	end
end

local function _uaGetSkin(w) return getgenv().__UA_Equipped[w] or _uaDefaults[w] end

local function _uaInitNames(milk)
	_uaW2n, _uaNidToWName = {}, {}
	for name, data in pairs(milk.Directory.Items.all) do
		if type(data) == "table" and data.nID then
			local nid = data.nID
			_uaW2n[name] = nid
			_uaNidToWName[nid] = name
			_uaNidToWName[tostring(nid)] = name
		end
	end
end

local function setupUnlockAll()
	if _unlockAllActive then return end

	-- \229\187\182\233\129\178\229\136\157\229\167\139\229\140\150 Milk
	local ok, milk = pcall(function() return require(_uaRS:WaitForChild("Milk")) end)
	if not ok or not milk then
		Library:Notify("Unlock All: \231\132\161\230\179\149\232\188\137\229\133\165 Milk \230\168\161\231\181\132", 3)
		return
	end
	_uaMilk = milk

	-- \232\168\173\231\189\174\232\167\163\233\142\150 UserId
	_uaMilk.Constants.UnlockAllUserIds = _uaMilk.Constants.UnlockAllUserIds or {}
	_uaMilk.Constants.UnlockAllUserIds[_uaLP.UserId] = true
	pcall(function() _uaMilk:Init() end)

	-- \229\187\186\231\171\139\230\173\166\229\153\168\229\144\141\231\168\177\229\176\141\231\133\167\232\161\168
	_uaInitNames(_uaMilk)

	-- Hook DataHandler
	local dh = _uaMilk.Handlers and _uaMilk.Handlers.DataHandler
	if dh and dh.GetDataAsync and not _unlockAllOrigGetDataAsync then
		_unlockAllOrigGetDataAsync = dh.GetDataAsync
		dh.GetDataAsync = function(self, ...)
			local data = _unlockAllOrigGetDataAsync(self, ...)
			if not _unlockAllActive then return data end
			if type(data) == "table" and type(data.Items) == "table" then
				local real = data.Items
				data.Items = setmetatable({}, {
					__index = function(t, nid)
						local r = real[nid]
						if not r then return nil end
						local wName = _uaNidToWName[nid] or _uaNidToWName[tostring(nid)]
						if wName then
							local s = _uaGetSkin(wName)
							if s and s ~= "" then
								local item = {}
								for k, v in next, r do item[k] = v end
								item.skin = s
								return item
							end
						end
						return r
					end,
					__newindex = function(t, k, v) real[k] = v end,
					__pairs = function() return pairs(real) end
				})
			end
			return data
		end
	end

	-- Hook neuron armory_change_skin
	pcall(function()
		local neuron = require(_uaRF:WaitForChild("neuron"))
		if neuron and neuron.net and neuron.net.packets and neuron.net.packets.armory_change_skin then
			local pkt = neuron.net.packets.armory_change_skin
			if not _unlockAllOrigPktFire then
				_unlockAllOrigPktFire = pkt.Fire
				pkt.Fire = function(self, weaponName, skinName)
					if _unlockAllActive and type(weaponName) == "string" and type(skinName) == "string" then
						getgenv().__UA_Equipped[weaponName] = skinName
						_uaSaveConfig()
					end
					return _unlockAllOrigPktFire(self, weaponName, skinName)
				end
			end
		end
	end)

	-- Heartbeat hook for chickynoid skin state
	local ok2, chickModule = pcall(function()
		return require(_uaRF.Packages.Chickynoid.Client.ClientModule)
	end)
	if ok2 and chickModule then
		_unlockAllHeartbeat = _uaRunService.Heartbeat:Connect(function()
			if not _unlockAllActive then return end
			pcall(function()
				local chick = chickModule.localChickynoid
				if not chick or not chick.simulation then return end
				local state = chick.simulation.state
				if not state then return end
				for i = 1, 4 do
					local wid = state["slot" .. i .. "_weaponID"]
					if wid and wid ~= "" then
						local s = _uaGetSkin(wid)
						if s and s ~= "" then state["slot" .. i .. "_skin"] = s end
					end
				end
			end)
		end)
	end

	_unlockAllActive = true
	Library:Notify("Unlock All \229\183\178\233\150\139\229\149\159", 2)
end

local function teardownUnlockAll()
	if not _unlockAllActive then return end
	_unlockAllActive = false

	-- \232\167\163\233\153\164 UserId \232\167\163\233\142\150
	if _uaMilk then
		if _uaMilk.Constants and _uaMilk.Constants.UnlockAllUserIds then
			_uaMilk.Constants.UnlockAllUserIds[_uaLP.UserId] = nil
		end
	end

	-- \233\130\132\229\142\159 DataHandler
	if _uaMilk then
		local dh = _uaMilk.Handlers and _uaMilk.Handlers.DataHandler
		if dh and _unlockAllOrigGetDataAsync then
			dh.GetDataAsync = _unlockAllOrigGetDataAsync
			_unlockAllOrigGetDataAsync = nil
		end
	end

	-- \233\130\132\229\142\159 neuron packet
	pcall(function()
		local neuron = require(_uaRF:WaitForChild("neuron"))
		if neuron and neuron.net and neuron.net.packets and neuron.net.packets.armory_change_skin then
			local pkt = neuron.net.packets.armory_change_skin
			if _unlockAllOrigPktFire then
				pkt.Fire = _unlockAllOrigPktFire
				_unlockAllOrigPktFire = nil
			end
		end
	end)

	-- \230\150\183\233\150\139 Heartbeat
	if _unlockAllHeartbeat then
		_unlockAllHeartbeat:Disconnect()
		_unlockAllHeartbeat = nil
	end

	Library:Notify("Unlock All \229\183\178\233\151\156\233\150\137", 2)
end

-- \231\182\129\229\174\154 Toggle \229\155\158\232\170\191
Toggles.UnlockAll:OnChanged(function(v)
	if v then
		setupUnlockAll()
	else
		teardownUnlockAll()
	end
end)

-- Hook Effects for tracers and hit sounds
local function setupEffectsHooks()
	lazyLoad()
	if not Effects then
		task.delay(1, setupEffectsHooks)
		return
	end

	Effects:Listen("Shot", function(player, character, weaponId, shotData)
		local isLocal = (player == LP)
		if Toggles.TracersEnabled and Toggles.TracersEnabled.Value and shotData.origin and shotData.hitPos and isLocal then
			local color = Options.TracersColor and Options.TracersColor.Value or Color3.fromRGB(0, 200, 255)
			local thickness = Options.TracersThickness and Options.TracersThickness.Value or 1
			local headPos = LP.Character and LP.Character:FindFirstChild("Head") and LP.Character.Head.Position or shotData.origin
			drawTracer(headPos, shotData.hitPos, color, thickness)
		end
		if Toggles.CustomShootSound and Toggles.CustomShootSound.Value and isLocal then
			local soundId = Options.ShootSoundID and Options.ShootSoundID.Value or ""
			playCustomSound(soundId, 1)
		end
	end)

	Effects:Listen("Damage", function(weaponId, fromChar, toChar, damage, headshot, deflected)
		if Toggles.CustomHitSound and Toggles.CustomHitSound.Value and fromChar == LP.Character and toChar ~= LP.Character and damage > 0 then
			local soundId = Options.HitSoundID and Options.HitSoundID.Value or ""
			local vol = Options.HitSoundVolume and Options.HitSoundVolume.Value or 1
			playCustomSound(soundId, vol)
		end

		if Toggles.HitNotification and Toggles.HitNotification.Value and fromChar == LP.Character and toChar ~= LP.Character and damage > 0 then
			local name = "(unknown)"
			for _, p in ipairs(Players:GetPlayers()) do
				if p.Character == toChar then name = p.Name; break end
			end
			local htype = headshot and " HEADSHOT" or ""
			Library:Notify("Hit " .. name .. " for " .. math.floor(damage) .. " dmg" .. htype, 2)
		end

		if Toggles.HitOverlay and Toggles.HitOverlay.Value and fromChar == LP.Character and toChar ~= LP.Character and damage > 0 then
			setupHitOverlay()
			if hitOverlayFrame then
				local color = Options.HitOverlayColor and Options.HitOverlayColor.Value or Color3.fromRGB(255, 0, 0)
				local alpha = Options.HitOverlayAlpha and Options.HitOverlayAlpha.Value or 0.3
				local dur = Options.HitOverlayDuration and Options.HitOverlayDuration.Value or 0.3
				hitOverlayFrame.BackgroundColor3 = color
				hitOverlayFrame.BackgroundTransparency = 1 - alpha
				hitOverlayGui.Enabled = true
				task.delay(dur, function()
					if hitOverlayGui then hitOverlayGui.Enabled = false end
				end)
			end
		end

		if Toggles.HitVisualEffect and Toggles.HitVisualEffect.Value and fromChar == LP.Character and toChar ~= LP.Character and damage > 0 then
			playHitVisualEffect(toChar, headshot)
		end
	end)
end

-- Hook gun fire for custom shoot sounds (hook into Effects:Listen("Shot") instead)
local function playCustomSound(soundId, volume)
	if soundId == "" then return end
	local s = Instance.new("Sound")
	s.SoundId = soundId
	s.Volume = volume or 1
	local world = workspace:FindFirstChild("World")
	s.Parent = world and world:FindFirstChild("FX") or workspace
	s:Play()
	game:GetService("Debris"):AddItem(s, 5)
end

-- Viewmodel changer & chams & FOV
local function getEquippedVM()
	lazyLoad()
	if not CameraHandler then return nil end
	local nMod = getNeuronModule()
	local character = LP.Character or (nMod and nMod:get_character(LP))
	if not character then return nil end
	if ViewModel then
		return ViewModel.getEquipped(character)
	end
	return nil
end

-- Camera FOV hook for weapon FOV
local fovHooked = false
local function setupFOVHooks()
	if fovHooked then return end
	lazyLoad()
	if not CameraHandler then
		task.delay(1, setupFOVHooks)
		return
	end
	fovHooked = true

	CameraHandler:addPostUpdateHook(function(dt)
		if CameraHandler.setBaseFOV then
			if Toggles.AestheticVM and Toggles.AestheticVM.Value then
				local vm = getEquippedVM()
				if vm and vm.aiming then
					CameraHandler:setBaseFOV(85)
				else
					CameraHandler:setBaseFOV(95)
				end
			end
		end
	end)
end

local getEquippedHooked = false
local function updateVMOffsets()
	if getEquippedHooked then return end

	local originalGetEquipped = nil
	for _, obj in ipairs(getgc(true)) do
		if typeof(obj) == "function" then
			local info = debug.getinfo(obj)
			if info.source == "=ReplicatedStorage.Modules.WeaponECS.Render.ViewModel" and info.name == "getEquipped" then
				originalGetEquipped = obj
				break
			end
		end
	end

	if originalGetEquipped then
		getEquippedHooked = true
		_G.OriginalZOfVMs = _G.OriginalZOfVMs or {}
		_G.OriginalADSOfVMs = _G.OriginalADSOfVMs or {}
		local originalZs = _G.OriginalZOfVMs
		local originalADSs = _G.OriginalADSOfVMs

		local oldHook; oldHook = hookfunction(originalGetEquipped, function(p1)
			local result = oldHook(p1)
			if result then
				local baseZ = originalZs[result.weaponID]
				if not baseZ then
					baseZ = result.hipOffsetCF.Position.Z
					originalZs[result.weaponID] = baseZ
				end

				local baseADS = originalADSs[result.weaponID]
				if not baseADS then
					baseADS = result.adsOffsetCF or result.adsOffsetCFrame or result.adsOffset or result.aimOffsetCF or result.aimOffset
					originalADSs[result.weaponID] = baseADS
				end

				if _G.AestheticVMEnabled then
					local offsetCF = CFrame.new(1.6, -0.3, -1.2)
					local baseHipCF = CFrame.new(0, -1.5, baseZ)
					local targetCF = baseHipCF * offsetCF
					
					result.hipOffsetCF = targetCF
					result.offsetCFrame = targetCF
					if result.adsOffsetCF then result.adsOffsetCF = targetCF end
					if result.adsOffsetCFrame then result.adsOffsetCFrame = targetCF end
					if result.adsOffset then result.adsOffset = targetCF end
					if result.aimOffsetCF then result.aimOffsetCF = targetCF end
					if result.aimOffset then result.aimOffset = targetCF end
				else
					if baseZ then
						result.hipOffsetCF = CFrame.new(0, -1.5, baseZ)
					end
					if baseADS then
						if result.adsOffsetCF then result.adsOffsetCF = baseADS end
						if result.adsOffsetCFrame then result.adsOffsetCFrame = baseADS end
						if result.adsOffset then result.adsOffset = baseADS end
						if result.aimOffsetCF then result.aimOffsetCF = baseADS end
						if result.aimOffset then result.aimOffset = baseADS end
					end
				end
			end
			return result
		end)
	end
end

-- Viewmodel / Gun chams
local function updateVMChams()
	lazyLoad()
	if not ViewModel then return end
	local nMod = getNeuronModule()
	local character = LP.Character or (nMod and nMod:get_character(LP))
	if not character then return end

	local controllers = ViewModel.getControllers(character)
	if not controllers then return end

	local weaponChamsOn = Toggles.VMChams and Toggles.VMChams.Value or false
	local weaponColor = Options.VMChamsColor and Options.VMChamsColor.Value or Color3.fromRGB(0, 200, 255)
	local weaponStyle = Options.VMChamsStyle and Options.VMChamsStyle.Value or "ForceField"
	local weaponTrans = Options.VMChamsTransparency and Options.VMChamsTransparency.Value or 0.5

	local armChamsOn = Toggles.VMArmChams and Toggles.VMArmChams.Value or false
	local armColor = Options.VMArmChamsColor and Options.VMArmChamsColor.Value or Color3.fromRGB(0, 200, 255)
	local armStyle = Options.VMArmChamsStyle and Options.VMArmChamsStyle.Value or "Plastic"
	local armTrans = Options.VMArmChamsTransparency and Options.VMArmChamsTransparency.Value or 0
	local armHideADS = Toggles.VMArmHideADS and Toggles.VMArmHideADS.Value or false

	for id, vm in controllers do
		local model = vm and vm.viewModel
		if model and model.Parent then
			local arms, weapon = getViewModelParts(model)

			local aiming = checkAiming(vm)
			local finalArmChamsOn = armChamsOn
			local finalArmTrans = armTrans
			local finalArmStyle = armStyle

			if aiming and armHideADS then
				finalArmChamsOn = true
				finalArmTrans = 1
				finalArmStyle = "Plastic"
			end

			applyChamsToParts(weapon, weaponChamsOn, weaponColor, weaponStyle, weaponTrans)
			applyChamsToParts(arms, finalArmChamsOn, armColor, finalArmStyle, finalArmTrans)
		end
	end

	-- Clean up destroyed parts from originalProperties
	for part in pairs(originalProperties) do
		if not part or not part.Parent then
			originalProperties[part] = nil
			cleanPart(part)
		end
	end
end

-- Gun chams (weapons on entities/players)
local gunHighlights = {}
local function updateGunChams()
	local chamsOn = Toggles.GunChams and Toggles.GunChams.Value or false
	local chamsColor = Options.GunChamsColor and Options.GunChamsColor.Value or Color3.fromRGB(0, 200, 255)
	local chamsDepth = Options.GunChamsMode and Options.GunChamsMode.Value == "Always" and Enum.HighlightDepthMode.AlwaysOnTop or Enum.HighlightDepthMode.Occluded
	local chamsStyle = Options.GunChamsStyle and Options.GunChamsStyle.Value or "ForceField"
	local chamsTrans = Options.GunChamsTransparency and Options.GunChamsTransparency.Value or 0.5

	local world = workspace:FindFirstChild("World")
	local gunModels = {}
	if world then
		local function scanForGuns(container)
			for _, child in ipairs(container:GetChildren()) do
				if child:IsA("Model") and child:FindFirstChildWhichIsA("BasePart") then
					local isWeapon = false
					for _, part in ipairs(child:GetDescendants()) do
						if part:IsA("Tool") or (part:IsA("Part") and part.Name:find("Grip")) then
							isWeapon = true; break
						end
					end
					if isWeapon or child.Name:find("Gun") or child.Name:find("Weapon") or child.Name:find("sword") or child.Name:find("blade") or child.Name:find("launcher") then
						gunModels[child] = true
					end
				end
			end
		end
		local entities = world:FindFirstChild("Entities")
		if entities then scanForGuns(entities) end
		local fx = world:FindFirstChild("FX")
		if fx then
			for _, child in ipairs(fx:GetChildren()) do
				if child:IsA("Model") and child:FindFirstChildWhichIsA("BasePart") then
					if not child.Name:find("Dummy") then
						gunModels[child] = true
					end
				end
			end
		end
	end

	-- Clean up Highlights if not using Highlight style or chams is off
	for model, hl in pairs(gunHighlights) do
		if not chamsOn or not gunModels[model] or chamsStyle ~= "Highlight" then
			pcall(function() hl:Destroy() end)
			gunHighlights[model] = nil
		end
	end

	-- Restore part properties if not using part styles or chams is off
	for part, orig in pairs(worldOriginalProperties) do
		local shouldRestore = not chamsOn or chamsStyle == "Highlight"
		if not shouldRestore then
			local isStillGun = false
			for model in pairs(gunModels) do
				if part:IsDescendantOf(model) then
					isStillGun = true; break
				end
			end
			if not isStillGun then shouldRestore = true end
		end
		if shouldRestore then
			pcall(function()
				local wireBox = part:FindFirstChild("WireframeCham")
				if wireBox then wireBox:Destroy() end
				part.Material = orig.Material
				part.Color = orig.Color
				part.Transparency = orig.Transparency
				if part:IsA("MeshPart") and orig.TextureID then
					part.TextureID = orig.TextureID
				end
			end)
			worldOriginalProperties[part] = nil
		end
	end

	if chamsOn then
		for model in pairs(gunModels) do
			if chamsStyle == "Highlight" then
				if not gunHighlights[model] then
					local hl = Instance.new("Highlight")
					hl.Adornee = model
					hl.DepthMode = chamsDepth
					hl.FillColor = chamsColor
					hl.FillTransparency = chamsTrans
					hl.OutlineColor = Color3.fromRGB(255, 255, 255)
					hl.OutlineTransparency = 0.5
					hl.Parent = model
					gunHighlights[model] = hl
				else
					local hl = gunHighlights[model]
					hl.DepthMode = chamsDepth
					hl.FillColor = chamsColor
					hl.FillTransparency = chamsTrans
				end
			else
				-- Part modification style (ForceField, Neon, etc.)
				for _, part in ipairs(model:GetDescendants()) do
					if part:IsA("BasePart") then
						if shouldIgnorePart(part) and not worldOriginalProperties[part] then
							continue
						end

						if not worldOriginalProperties[part] then
							worldOriginalProperties[part] = {
								Material = part.Material,
								Color = part.Color,
								Transparency = part.Transparency,
								TextureID = part:IsA("MeshPart") and part.TextureID or nil
							}
						end
						pcall(function()
							local wireBox = part:FindFirstChild("WireframeCham")
							if chamsStyle == "Wireframe" then
								applyWireframeToPart(part, chamsColor, chamsDepth == Enum.HighlightDepthMode.AlwaysOnTop)

								part.Material = Enum.Material.SmoothPlastic
								part.Color = Color3.fromRGB(5, 5, 8)
								part.Transparency = 0.85
								if part:IsA("MeshPart") then
									part.TextureID = ""
								end
							else
								if wireBox then wireBox:Destroy() end
								if chamsStyle == "ForceField" then
									part.Material = Enum.Material.ForceField
								elseif chamsStyle == "Neon" then
									part.Material = Enum.Material.Neon
								elseif chamsStyle == "Glass" then
									part.Material = Enum.Material.Glass
								else
									part.Material = Enum.Material.SmoothPlastic
								end
								part.Color = chamsColor
								part.Transparency = chamsTrans
								if part:IsA("MeshPart") then
									part.TextureID = ""
								end
							end
						end)
					end
				end
			end
		end
	end
end

-- Initialize advanced features
task.spawn(function()
	task.wait(2)
	setupEffectsHooks()
	setupFOVHooks()
	setupStretchHook()
	setupScreenTilt()
	setupHitOverlay()
	setupThirdPersonHook()
	pcall(updateSkybox)
end)


RunService:BindToRenderStep("ESP_Draw", Enum.RenderPriority.Camera.Value + 1, function()
	Camera = workspace.CurrentCamera
	if not Camera then return end
	local camPos = Camera.CFrame.Position
	local ctr = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
	local char = LP.Character

	-- World
	if Toggles.NightMode and Toggles.NightMode.Value then
		Lighting.ClockTime = 0; Lighting.Brightness = 0.5; Lighting.Ambient = Color3.fromRGB(40, 40, 80); Lighting.FogEnd = 99999
	end
	if Toggles.CustomTime and Toggles.CustomTime.Value then Lighting.ClockTime = Options.TimeOfDay and Options.TimeOfDay.Value or 12 end
	if Toggles.CustomFog and Toggles.CustomFog.Value then
		Lighting.FogEnd = Options.FogEnd and Options.FogEnd.Value or 500; Lighting.FogStart = Options.FogStart and Options.FogStart.Value or 0
	end
	if Toggles.CustomAmbient and Toggles.CustomAmbient.Value and Options.AmbientColor then
		Lighting.Ambient = Options.AmbientColor.Value
	end
	if Options.Brightness then Lighting.Brightness = Options.Brightness.Value end
	if Toggles.Fullbright and Toggles.Fullbright.Value then
		Lighting.Brightness = 10; Lighting.OutdoorAmbient = Color3.new(1,1,1); Lighting.Ambient = Color3.new(1,1,1)
		Lighting.GlobalShadows = true
	end
	if Toggles.RemoveFog and Toggles.RemoveFog.Value then Lighting.FogEnd = 999999 end

	-- ===== RAIN EFFECT =====
	if Toggles.RainEffect and Toggles.RainEffect.Value then
		-- Create rain container if needed
		if not _G._WeatherFX or not _G._WeatherFX.rainPart or not _G._WeatherFX.rainPart.Parent then
			_G._WeatherFX = _G._WeatherFX or {}
			-- Clean up old
			pcall(function() if _G._WeatherFX.rainPart then _G._WeatherFX.rainPart:Destroy() end end)
			pcall(function() if _G._WeatherFX.rainSound then _G._WeatherFX.rainSound:Destroy() end end)
			if _G._WeatherFX.rainExtraParts then
				for _, ep in ipairs(_G._WeatherFX.rainExtraParts) do pcall(function() ep:Destroy() end) end
			end
			-- Main emitter part (\229\164\167\231\175\132\229\156\141\233\160\173\233\160\130)
			local rPart = Instance.new("Part")
			rPart.Name = "MokeRainEmitter"
			rPart.Anchored = true
			rPart.CanCollide = false
			rPart.CastShadow = false
			rPart.Transparency = 1
			rPart.Size = Vector3.new(150, 1, 150) -- \230\155\180\229\164\167\231\175\132\229\156\141
			rPart.CFrame = Camera.CFrame * CFrame.new(0, 35, 0)
			rPart.Parent = workspace
			local function makeRainEmitter(parent, rate)
				local pe = Instance.new("ParticleEmitter", parent)
				pe.Name = "RainParticle"
				-- \228\186\174\231\153\189\232\151\141\232\137\178\239\188\140\230\155\180\230\152\142\233\161\175
				pe.Color = ColorSequence.new(Color3.fromRGB(200, 225, 255))
				pe.LightEmission = 0.15
				pe.LightInfluence = 0.6
				-- \230\155\180\231\178\151\227\128\129\230\155\180\233\149\183\231\154\132\233\155\168\230\187\180
				pe.Size = NumberSequence.new({
					NumberSequenceKeypoint.new(0, 0.12),
					NumberSequenceKeypoint.new(1, 0.12)
				})
				-- \229\185\190\228\185\142\228\184\141\233\128\143\230\152\142
				pe.Transparency = NumberSequence.new({
					NumberSequenceKeypoint.new(0, 0.05),
					NumberSequenceKeypoint.new(0.85, 0.1),
					NumberSequenceKeypoint.new(1, 1)
				})
				pe.Lifetime = NumberRange.new(1.0, 1.5)
				-- \230\155\180\229\191\171\231\154\132\233\128\159\229\186\166
				pe.Speed = NumberRange.new(80, 100)
				pe.SpreadAngle = Vector2.new(4, 4)
				pe.Rate = rate
				pe.Rotation = NumberRange.new(90, 90)
				pe.RotSpeed = NumberRange.new(0, 0)
				pe.VelocityInheritance = 0
				pe.EmissionDirection = Enum.NormalId.Bottom
				return pe
			end
			makeRainEmitter(rPart, 500)
			-- \233\161\141\229\164\150\232\163\156\229\133\133\239\188\154\229\137\141\230\150\185/\229\190\140\230\150\185\229\144\132\229\134\141\231\148\159\230\136\144\228\184\128\231\181\132 emitter\239\188\140\229\161\171\230\187\191\231\142\169\229\174\182\232\166\150\232\167\146
			local extraParts = {}
			local offsets = {
				Vector3.new(0, 30, -25),
				Vector3.new(0, 30, 25),
				Vector3.new(-25, 30, 0),
				Vector3.new(25, 30, 0),
			}
			for _, offset in ipairs(offsets) do
				local ep = Instance.new("Part")
				ep.Name = "MokeRainEmitterExtra"
				ep.Anchored = true
				ep.CanCollide = false
				ep.CastShadow = false
				ep.Transparency = 1
				ep.Size = Vector3.new(80, 1, 80)
				ep.CFrame = Camera.CFrame * CFrame.new(offset.X, offset.Y, offset.Z)
				ep.Parent = workspace
				makeRainEmitter(ep, 150)
				table.insert(extraParts, ep)
			end
			-- \233\155\168\232\129\178
			local rs = Instance.new("Sound", rPart)
			rs.Name = "MokeRainSound"
			rs.SoundId = "rbxassetid://130766740"
			rs.Volume = 1.0
			rs.Looped = true
			rs:Play()
			_G._WeatherFX.rainPart = rPart
			_G._WeatherFX.rainSound = rs
			_G._WeatherFX.rainExtraParts = extraParts
		end
		-- Follow camera (main + extra parts)
		local rp = _G._WeatherFX.rainPart
		local baseRate = Options.RainIntensity and Options.RainIntensity.Value or 500
		if rp and rp.Parent then
			rp.CFrame = CFrame.new(Camera.CFrame.Position + Vector3.new(0, 35, 0))
			local pe2 = rp:FindFirstChild("RainParticle")
			if pe2 then pe2.Rate = baseRate end
		end
		local extraOffsets = {
			Vector3.new(0, 30, -25),
			Vector3.new(0, 30, 25),
			Vector3.new(-25, 30, 0),
			Vector3.new(25, 30, 0),
		}
		if _G._WeatherFX.rainExtraParts then
			for i, ep in ipairs(_G._WeatherFX.rainExtraParts) do
				if ep and ep.Parent then
					local off = extraOffsets[i] or Vector3.new(0, 30, 0)
					ep.CFrame = CFrame.new(Camera.CFrame.Position + off)
					local pe3 = ep:FindFirstChild("RainParticle")
					if pe3 then pe3.Rate = math.floor(baseRate * 0.3) end
				end
			end
		end
		-- \233\155\168\229\164\169\229\133\137\231\133\167\239\188\154\230\152\142\233\161\175\232\174\138\230\154\151\239\188\139\229\164\167\233\156\167
		if not (Toggles.Fullbright and Toggles.Fullbright.Value) and not (Toggles.NightMode and Toggles.NightMode.Value) then
			Lighting.Ambient = Color3.fromRGB(60, 70, 85)
			Lighting.OutdoorAmbient = Color3.fromRGB(70, 80, 95)
			Lighting.Brightness = math.min(Lighting.Brightness, 1.5)
			Lighting.FogEnd = math.min(Lighting.FogEnd, 400)
			Lighting.FogStart = 20
			Lighting.FogColor = Color3.fromRGB(130, 145, 165)
		end
	else
		-- Clean up rain
		if _G._WeatherFX and _G._WeatherFX.rainPart and _G._WeatherFX.rainPart.Parent then
			pcall(function() _G._WeatherFX.rainPart:Destroy() end)
			_G._WeatherFX.rainPart = nil
			_G._WeatherFX.rainSound = nil
		end
	end

	-- ===== SNOW EFFECT =====
	if Toggles.SnowEffect and Toggles.SnowEffect.Value then
		if not _G._WeatherFX or not _G._WeatherFX.snowPart or not _G._WeatherFX.snowPart.Parent then
			_G._WeatherFX = _G._WeatherFX or {}
			pcall(function() if _G._WeatherFX.snowPart then _G._WeatherFX.snowPart:Destroy() end end)
			local sPart = Instance.new("Part")
			sPart.Name = "MokeSnowEmitter"
			sPart.Anchored = true
			sPart.CanCollide = false
			sPart.CastShadow = false
			sPart.Transparency = 1
			sPart.Size = Vector3.new(80, 1, 80)
			sPart.CFrame = Camera.CFrame * CFrame.new(0, 25, 0)
			sPart.Parent = workspace
			-- Snow particle
			local se = Instance.new("ParticleEmitter", sPart)
			se.Name = "SnowParticle"
			se.Color = ColorSequence.new(Color3.fromRGB(240, 248, 255))
			se.LightEmission = 0.2
			se.LightInfluence = 0.8
			se.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.18), NumberSequenceKeypoint.new(0.5, 0.22), NumberSequenceKeypoint.new(1, 0.05)})
			se.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.1), NumberSequenceKeypoint.new(0.7, 0.2), NumberSequenceKeypoint.new(1, 1)})
			se.Lifetime = NumberRange.new(3.5, 5)
			se.Speed = NumberRange.new(8, 14)
			se.SpreadAngle = Vector2.new(25, 25)
			se.Rate = 150
			se.Rotation = NumberRange.new(0, 360)
			se.RotSpeed = NumberRange.new(-20, 20)
			se.VelocityInheritance = 0
			se.EmissionDirection = Enum.NormalId.Bottom
			-- Wind drift for snow
			se.Acceleration = Vector3.new(2, 0, 0)
			_G._WeatherFX.snowPart = sPart
		end
		-- Follow camera
		local sp = _G._WeatherFX.snowPart
		if sp and sp.Parent then
			sp.CFrame = CFrame.new(Camera.CFrame.Position + Vector3.new(0, 25, 0))
			local se2 = sp:FindFirstChild("SnowParticle")
			if se2 then
				se2.Rate = Options.SnowIntensity and Options.SnowIntensity.Value or 150
			end
		end
		-- Snow makes ambient cooler/bluish
		if not (Toggles.Fullbright and Toggles.Fullbright.Value) and not (Toggles.NightMode and Toggles.NightMode.Value) then
			Lighting.Ambient = Color3.fromRGB(160, 170, 190)
			Lighting.FogEnd = math.min(Lighting.FogEnd, 500)
			Lighting.FogColor = Color3.fromRGB(220, 225, 235)
		end
	else
		-- Clean up snow
		if _G._WeatherFX and _G._WeatherFX.snowPart and _G._WeatherFX.snowPart.Parent then
			pcall(function() _G._WeatherFX.snowPart:Destroy() end)
			_G._WeatherFX.snowPart = nil
		end
	end

	-- XRay
	if Toggles.XRay and Toggles.XRay.Value and char then
		for _, v in ipairs(workspace:GetDescendants()) do
			if v:IsA("BasePart") and not v:IsDescendantOf(char) then pcall(function() v.LocalTransparencyModifier = 0.8 end) end
		end
	end

	-- FOV Circle
	if not fovCircle then fovCircle = Drawing.new("Circle"); fovCircle.Visible = false; fovCircle.Thickness = 1; fovCircle.NumSides = 64; fovCircle.Filled = false end
	local showFov = Toggles.FOVCircle and Toggles.FOVCircle.Value or false
	if showFov then
		local fovDeg = Options.FOVCircleSize and Options.FOVCircleSize.Value or 10
		local fovPx = math.tan(math.rad(fovDeg / 2)) * (Camera.ViewportSize.Y / 2) / math.tan(math.rad(Camera.FieldOfView) / 2)
		fovCircle.Radius = fovPx; fovCircle.Position = ctr; fovCircle.Visible = true
		fovCircle.Color = Options.FOVCircleColor and Options.FOVCircleColor.Value or Color3.fromRGB(127, 72, 163)
	else fovCircle.Visible = false end

	-- Aspect ratio handled via CameraHandler:addPostUpdateHook (CFrame matrix)

	-- Scan entities
	if tick() - lastScan > 0.5 then entities = findAll(); lastScan = tick() end
	local valid = {}; for eid, ent in pairs(entities) do if tick() - (ent.lastSeen or 0) <= 5 then valid[eid] = ent end end
	local teamCheck = Toggles.ESPTeamCheck and Toggles.ESPTeamCheck.Value or false

	-- First pass: build aim targets (always)
	local aimTargets = {}
	for eid, ent in pairs(valid) do
		if teamCheck and isFriend(ent) then continue end
		local headPos = getHeadPos(ent)
		if not headPos then continue end
		local dist = (headPos - camPos).Magnitude
		if dist > 500 then continue end
		local hsp, hos = Camera:WorldToViewportPoint(headPos)
		local footPos = getFootPos(ent)
		local onScreen = hos and (hsp.X >= 0 and hsp.X <= Camera.ViewportSize.X and hsp.Y >= 0 and hsp.Y <= Camera.ViewportSize.Y)
		table.insert(aimTargets, { ent = ent, headPos = headPos, headS = Vector2.new(hsp.X, hsp.Y), footPos = footPos, dist = dist, onScreen = onScreen, hos = hos })
	end

	-- Second pass: ESP drawing
	local espOn = Toggles.ESPEnabled and Toggles.ESPEnabled.Value or false
	local count = 0

	if espOn then
		for _, target in ipairs(aimTargets) do
			if count >= maxESPObjects then break end
			local ent = target.ent
			local model = ent.Model
			if not model or not model.Parent then continue end

			local headPos = target.headPos
			if not headPos then continue end

			local headS = target.headS
			local onScreen = target.onScreen

			count = count + 1
			local obj = espPool[count]

			-- 0. Offscreen indicator (arrow around crosshair pointing to offscreen enemies)
			local showOffscreen = Toggles.ESPOffscreen and Toggles.ESPOffscreen.Value or false
			if showOffscreen and not onScreen then
				local relativePos = Camera.CFrame:PointToObjectSpace(headPos)
				local angle = math.atan2(-relativePos.X, -relativePos.Z)
				local radius = 140 -- Radius from crosshair
				local screenCenter = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
				local arrowPos = screenCenter + Vector2.new(math.sin(angle), -math.cos(angle)) * radius
				
				local dirVec = (arrowPos - screenCenter).Unit
				local rightVec = Vector2.new(-dirVec.Y, dirVec.X)
				
				local p1 = arrowPos + dirVec * 9
				local p2 = arrowPos - dirVec * 5 + rightVec * 5
				local p3 = arrowPos - dirVec * 5 - rightVec * 5
				
				obj.OffscreenArrow.PointA = p1
				obj.OffscreenArrow.PointB = p2
				obj.OffscreenArrow.PointC = p3
				obj.OffscreenArrow.Color = Options.ESPOffscreenColor and Options.ESPOffscreenColor.Value or Color3.fromRGB(255, 50, 50)
				obj.OffscreenArrow.Transparency = 0.8
				obj.OffscreenArrow.Visible = true
			else
				obj.OffscreenArrow.Visible = false
			end

			-- Check if the player is offscreen. If so, hide standard 2D elements and skip drawing them!
			if not onScreen then
				obj.BoxOutline.Visible = false
				obj.Box.Visible = false
				obj.BoxFill.Visible = false
				obj.HeadDotOutline.Visible = false
				obj.HeadDot.Visible = false
				obj.Name.Visible = false
				obj.Distance.Visible = false
				obj.WeaponName.Visible = false
				obj.HealthBarOutline.Visible = false
				obj.HealthBar.Visible = false
				obj.HealthText.Visible = false
				obj.TracerOutline.Visible = false
				obj.Tracer.Visible = false
				for c = 1, 8 do
					obj.CornerOutlines[c].Visible = false
					obj.Corners[c].Visible = false
				end
				for j = 1, #skeletonConnections do
					obj.Skeleton[j].Outline.Visible = false
					obj.Skeleton[j].Line.Visible = false
				end
				continue
			end

			-- Determine if foot is visible and get box dimensions
			local footPos = target.footPos
			local footS
			local hasFoot = false
			if footPos then
				local fsp, fos = Camera:WorldToViewportPoint(footPos)
				if fos then
					footS = Vector2.new(fsp.X, fsp.Y)
					hasFoot = true
				end
			end

			local boxH, boxW, boxX, boxY
			local boxPos, boxSize
			if hasFoot then
				boxH = math.abs(headS.Y - footS.Y)
				boxW = boxH * 0.5
				boxX = headS.X - boxW / 2
				boxY = headS.Y - boxH * 0.1
				boxPos = Vector2.new(boxX, boxY)
				boxSize = Vector2.new(boxW, boxH)
			end

			-- 1. Box ESP (Border + Dark Glass background + Sharp corner brackets)
			if Toggles.ESPBox and Toggles.ESPBox.Value and hasFoot then
				local color = Options.ESPBoxColor and Options.ESPBoxColor.Value or Color3.fromRGB(255, 50, 50)

				-- Draw dark background fill
				obj.BoxFill.Position = boxPos
				obj.BoxFill.Size = boxSize
				obj.BoxFill.Visible = true

				-- Draw corner brackets
				local w, h = boxSize.X, boxSize.Y
				local x, y = boxPos.X, boxPos.Y
				local l = math.min(w, h) * 0.16 -- Length of the corner brackets (16% of width/height)

				local lines = {
					-- Top-Left
					{Vector2.new(x, y), Vector2.new(x + l, y)},
					{Vector2.new(x, y), Vector2.new(x, y + l)},
					-- Top-Right
					{Vector2.new(x + w, y), Vector2.new(x + w - l, y)},
					{Vector2.new(x + w, y), Vector2.new(x + w, y + l)},
					-- Bottom-Left
					{Vector2.new(x, y + h), Vector2.new(x + l, y + h)},
					{Vector2.new(x, y + h), Vector2.new(x, y + h - l)},
					-- Bottom-Right
					{Vector2.new(x + w, y + h), Vector2.new(x + w - l, y + h)},
					{Vector2.new(x + w, y + h), Vector2.new(x + w, y + h - l)},
				}

				for c = 1, 8 do
					local data = lines[c]
					local out = obj.CornerOutlines[c]
					local line = obj.Corners[c]

					out.From = data[1]
					out.To = data[2]
					out.Visible = true

					line.From = data[1]
					line.To = data[2]
					line.Color = color
					line.Visible = true
				end

				-- Hide standard box lines
				obj.BoxOutline.Visible = false
				obj.Box.Visible = false
			else
				obj.BoxOutline.Visible = false
				obj.Box.Visible = false
				obj.BoxFill.Visible = false
				for c = 1, 8 do
					obj.CornerOutlines[c].Visible = false
					obj.Corners[c].Visible = false
				end
			end

			-- 2. Head Dot ESP
			if Toggles.ESPHeadDot and Toggles.ESPHeadDot.Value then
				local color = Options.ESPHeadDotColor and Options.ESPHeadDotColor.Value or Color3.fromRGB(255, 255, 50)

				obj.HeadDotOutline.Position = headS
				obj.HeadDotOutline.Visible = true

				obj.HeadDot.Position = headS
				obj.HeadDot.Color = color
				obj.HeadDot.Visible = true
			else
				obj.HeadDotOutline.Visible = false
				obj.HeadDot.Visible = false
			end

			-- 3. Health Bar ESP + HP Text
			if Toggles.ESPHealth and Toggles.ESPHealth.Value and hasFoot then
				local hp = 100
				local maxHp = 100
				local sMod = getStatesModule()
				local charStates = sMod and sMod.liveCharacters[model]
				if charStates and charStates.Health then
					hp = sMod:GetStateValue(model, "Health", 100)
					maxHp = sMod:GetStateValue(model, "MaxHealth", 100)
				else
					local hum = model:FindFirstChildWhichIsA("Humanoid")
					hp = hum and hum.Health or 100
					maxHp = hum and hum.MaxHealth or 100
				end
				local pct = math.clamp(hp / maxHp, 0, 1)

				local barWidth = 3
				local barSpacing = 6
				local barX = boxX - barWidth - barSpacing
				local barY = boxY

				obj.HealthBarOutline.Position = Vector2.new(barX - 1, barY - 1)
				obj.HealthBarOutline.Size = Vector2.new(barWidth + 2, boxH + 2)
				obj.HealthBarOutline.Visible = true

				local hpColor = Color3.fromRGB(255, 50, 50):Lerp(Color3.fromRGB(50, 255, 50), pct)
				obj.HealthBar.Position = Vector2.new(barX, barY + boxH * (1 - pct))
				obj.HealthBar.Size = Vector2.new(barWidth, boxH * pct)
				obj.HealthBar.Color = hpColor
				obj.HealthBar.Visible = true

				if hp < 100 then
					obj.HealthText.Text = tostring(math.floor(hp))
					obj.HealthText.Position = Vector2.new(barX - 8, barY + boxH * (1 - pct) - 2)
					obj.HealthText.Color = hpColor
					obj.HealthText.Visible = true
				else
					obj.HealthText.Visible = false
				end
			else
				obj.HealthBarOutline.Visible = false
				obj.HealthBar.Visible = false
				obj.HealthText.Visible = false
			end

			-- 4. Name ESP (Centered above the box)
			if Toggles.ESPName and Toggles.ESPName.Value then
				local color = Options.ESPNameColor and Options.ESPNameColor.Value or Color3.fromRGB(255, 255, 255)
				obj.Name.Text = ent.Name or ""
				obj.Name.Position = Vector2.new(headS.X, (boxY or headS.Y) - 15)
				obj.Name.Color = color
				obj.Name.Visible = true
			else
				obj.Name.Visible = false
			end

			-- 5. Weapon ESP (Below the box)
			local showWeapon = Toggles.ESPShowWeapon and Toggles.ESPShowWeapon.Value or false
			if showWeapon and hasFoot then
				local wName = getPlayerWeapon(model) or "HANDS"
				obj.WeaponName.Text = "[" .. wName:upper() .. "]"
				obj.WeaponName.Position = Vector2.new(headS.X, boxY + boxH + 2)
				obj.WeaponName.Color = Color3.fromRGB(200, 200, 200)
				obj.WeaponName.Visible = true
			else
				obj.WeaponName.Visible = false
			end

			-- 6. Distance ESP (Below the name/weapon)
			if Toggles.ESPDistance and Toggles.ESPDistance.Value then
				local distY = hasFoot and (boxY + boxH + (showWeapon and 14 or 2)) or (headS.Y + 15)
				obj.Distance.Text = tostring(math.floor(target.dist)) .. "m"
				obj.Distance.Position = Vector2.new(headS.X, distY)
				obj.Distance.Color = Color3.fromRGB(150, 150, 150)
				obj.Distance.Visible = true
			else
				obj.Distance.Visible = false
			end

			-- 7. Tracer/Snapline ESP
			if Toggles.ESPTracer and Toggles.ESPTracer.Value then
				local color = Options.ESPTracerColor and Options.ESPTracerColor.Value or Color3.fromRGB(255, 255, 255)
				local from = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
				local to = hasFoot and Vector2.new(headS.X, boxY + boxH) or headS

				obj.TracerOutline.From = from
				obj.TracerOutline.To = to
				obj.TracerOutline.Visible = true

				obj.Tracer.From = from
				obj.Tracer.To = to
				obj.Tracer.Color = color
				obj.Tracer.Visible = true
			else
				obj.TracerOutline.Visible = false
				obj.Tracer.Visible = false
			end

			-- 8. Skeleton ESP (Handles R6 and R15 dynamically with no delay)
			if Toggles.ESPSkeleton and Toggles.ESPSkeleton.Value then
				local isR15 = model:FindFirstChild("UpperTorso") ~= nil
				local partsList = isR15 and skeletonParts or { "Head", "Torso", "Left Arm", "Right Arm", "Left Leg", "Right Leg" }
				local connections = isR15 and skeletonConnections or { {1,2}, {2,3}, {2,4}, {2,5}, {2,6} }

				local pts = {}
				for _, name in ipairs(partsList) do
					local part = model:FindFirstChild(name)
					if part and part:IsA("BasePart") then
						local sp, so = Camera:WorldToViewportPoint(part.Position)
						pts[name] = so and Vector2.new(sp.X, sp.Y) or nil
					end
				end

				-- Clear all skeleton lines first
				for j = 1, #skeletonConnections do
					obj.Skeleton[j].Outline.Visible = false
					obj.Skeleton[j].Line.Visible = false
				end

				-- Draw active joints
				for j = 1, #connections do
					local a = pts[partsList[connections[j][1]]]
					local b = pts[partsList[connections[j][2]]]
					local sLine = obj.Skeleton[j]
					if a and b then
						local color = Options.ESPSkeletonColor and Options.ESPSkeletonColor.Value or Color3.fromRGB(255, 255, 255)

						sLine.Outline.From = a
						sLine.Outline.To = b
						sLine.Outline.Visible = true

						sLine.Line.From = a
						sLine.Line.To = b
						sLine.Line.Color = color
						sLine.Line.Visible = true
					end
				end
			else
				for j = 1, #skeletonConnections do
					obj.Skeleton[j].Outline.Visible = false
					obj.Skeleton[j].Line.Visible = false
				end
			end
		end
	end

	-- Hide unused pool objects
	for i = count + 1, maxESPObjects do
		local obj = espPool[i]
		obj.BoxOutline.Visible = false
		obj.Box.Visible = false
		obj.BoxFill.Visible = false
		obj.HeadDotOutline.Visible = false
		obj.HeadDot.Visible = false
		obj.Name.Visible = false
		obj.Distance.Visible = false
		obj.WeaponName.Visible = false
		obj.HealthBarOutline.Visible = false
		obj.HealthBar.Visible = false
		obj.HealthText.Visible = false
		obj.TracerOutline.Visible = false
		obj.Tracer.Visible = false
		obj.OffscreenArrow.Visible = false
		for c = 1, 8 do
			obj.CornerOutlines[c].Visible = false
			obj.Corners[c].Visible = false
		end
		for j = 1, #skeletonConnections do
			obj.Skeleton[j].Outline.Visible = false
			obj.Skeleton[j].Line.Visible = false
		end
	end

	-- Chams management
	local chamsOn = Toggles.Chams and Toggles.Chams.Value or false
	local chamsColor = Options.ChamsColor and Options.ChamsColor.Value or Color3.fromRGB(0, 200, 255)
	local chamsMode = Options.ChamsMode and Options.ChamsMode.Value or "Always"
	local chamsDepth = chamsMode == "Always" and Enum.HighlightDepthMode.AlwaysOnTop or Enum.HighlightDepthMode.Occluded
	local currentModels = {}
	for _, t in ipairs(aimTargets) do if t.ent and t.ent.Model then currentModels[t.ent.Model] = true end end
	for model, hl in pairs(chamsHighlights) do
		if not currentModels[model] then
			pcall(function() hl:Destroy() end)
			chamsHighlights[model] = nil
		end
	end
	if chamsOn then
		for _, t in ipairs(aimTargets) do
			local model = t.ent and t.ent.Model
			if model and model:IsA("Model") then
				if not chamsHighlights[model] then
					local hl = Instance.new("Highlight")
					hl.Adornee = model
					hl.DepthMode = chamsDepth
					hl.FillColor = chamsColor
					hl.FillTransparency = 0.3
					hl.OutlineColor = Color3.new(1, 1, 1)
					hl.OutlineTransparency = 0.5
					hl.Parent = model
					chamsHighlights[model] = hl
				else
					local hl = chamsHighlights[model]
					hl.DepthMode = chamsDepth
					hl.FillColor = chamsColor
				end
			end
		end
	end

	-- Viewmodel chams
	updateVMChams()

	-- Gun chams
	updateGunChams()

	-- Viewmodel changer offsets
	updateVMOffsets()

	-- Aimbot (mousemoverel)
	if Toggles.AimbotEnabled and Toggles.AimbotEnabled.Value and #aimTargets > 0 then
		local aimHeld = UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
		local aimKey = Options.AimbotKey
		if not aimHeld and aimKey and aimKey.Key then
			aimHeld = UserInputService:IsKeyDown(aimKey.Key)
		end
		if aimHeld then
			local best = nil; local bestDist = (Options.AimbotFOV and Options.AimbotFOV.Value or 10) * 6
			for _, t in ipairs(aimTargets) do
				if not t.onScreen then continue end -- DO NOT lock onto offscreen targets!
				local d = (t.headS - ctr).Magnitude
				if d < bestDist then bestDist = d; best = t end
			end
			if best then
				local delta = best.headS - ctr
				local smooth = Options.AimbotSmooth and Options.AimbotSmooth.Value or 10
				if smooth <= 1 then
					mousemoverel(delta.X, delta.Y)
					mousemoverel(delta.X, delta.Y)
					mousemoverel(delta.X, delta.Y)
				else
					mousemoverel(delta.X / smooth + delta.X * 0.4, delta.Y / smooth + delta.Y * 0.4)
				end
			end
		end
	end

	-- w
	if Toggles.TriggerbotEnabled and Toggles.TriggerbotEnabled.Value and #aimTargets > 0 then
		local trigHeld = UserInputService:IsKeyDown(Enum.KeyCode.Q)
		local trigKey = Options.TriggerbotKey
		if not trigHeld and trigKey and trigKey.Key and trigKey.Key ~= Enum.KeyCode.Q then
			trigHeld = UserInputService:IsKeyDown(trigKey.Key)
		end
		if trigHeld then
			for _, t in ipairs(aimTargets) do
				if (t.headS - ctr).Magnitude < 12 then
					mouse1press(); mouse1release()
					break
				end
			end
		end
	end
end)

-- ===== SPOOFER (Profile Pictures & Names) =====
local spooferImage = "https://i.imgur.com/65scUcs.png"
local spooferName = "Overkill\233\128\153\229\128\139\231\154\132\229\177\142\233\152\178\229\164\150\230\142\155"
local spooferHooked = false
local spooferScanned = false

local headshotNames = { PlayerHeadshot = true, Headshot = true }
local nameLabelNames = { Username = true, DisplayName = true, Killer = true, Killed = true, Assist = true, ["Assist+"] = true }

local function processHeadshot(img)
	if Toggles.ProfilePicSpoof and Toggles.ProfilePicSpoof.Value then
		img.Image = spooferImage
	end
end

local function processNametag(txt)
	if Toggles.NameSpoof and Toggles.NameSpoof.Value and nameLabelNames[txt.Name] then
		txt.Text = spooferName
	end
end

local function hookInstance(inst)
	if inst:IsA("ImageLabel") and headshotNames[inst.Name] then
		processHeadshot(inst)
		inst:GetPropertyChangedSignal("Image"):Connect(function()
			processHeadshot(inst)
		end)
		return
	end
	if (inst:IsA("TextLabel") or inst:IsA("TextButton")) and nameLabelNames[inst.Name] then
		processNametag(inst)
		inst:GetPropertyChangedSignal("Text"):Connect(function()
			processNametag(inst)
		end)
	end
end

local function scanSpoofer()
	if spooferScanned then return end
	spooferScanned = true
	local pg = LP:WaitForChild("PlayerGui")
	for _, desc in ipairs(pg:GetDescendants()) do
		hookInstance(desc)
	end
	for _, desc in ipairs(game:GetService("CoreGui"):GetDescendants()) do
		hookInstance(desc)
	end
end

local function setupSpoofer()
	if spooferHooked then return end
	spooferHooked = true
	scanSpoofer()
	local pg = LP:WaitForChild("PlayerGui")
	local function onDescendantAdded(desc)
		task.wait()
		if (Toggles.ProfilePicSpoof and Toggles.ProfilePicSpoof.Value) or (Toggles.NameSpoof and Toggles.NameSpoof.Value) then
			hookInstance(desc)
		end
	end
	pg.DescendantAdded:Connect(onDescendantAdded)
	game:GetService("CoreGui").DescendantAdded:Connect(onDescendantAdded)
end

Toggles.ProfilePicSpoof:OnChanged(function()
	if Toggles.ProfilePicSpoof.Value then setupSpoofer() end
end)

Toggles.NameSpoof:OnChanged(function()
	if Toggles.NameSpoof.Value then
		setupSpoofer()
		local pg = LP:WaitForChild("PlayerGui")
		for _, desc in ipairs(pg:GetDescendants()) do
			if desc:IsA("TextLabel") or desc:IsA("TextButton") then
				processNametag(desc)
			end
		end
	end
end)

-- ===== NOOB AVATAR CHANGER =====
local CLASSIC_FACE = "rbxassetid://8625736310"
local noobWatchConnection = nil

local noobColors = {
	Head = BrickColor.new("Bright yellow"),
	Torso = BrickColor.new("Bright blue"),
	LeftUpperArm = BrickColor.new("Bright yellow"),
	LeftLowerArm = BrickColor.new("Bright yellow"),
	LeftHand = BrickColor.new("Bright yellow"),
	RightUpperArm = BrickColor.new("Bright yellow"),
	RightLowerArm = BrickColor.new("Bright yellow"),
	RightHand = BrickColor.new("Bright yellow"),
	LeftUpperLeg = BrickColor.new("Bright green"),
	LeftLowerLeg = BrickColor.new("Bright green"),
	LeftFoot = BrickColor.new("Bright green"),
	RightUpperLeg = BrickColor.new("Bright green"),
	RightLowerLeg = BrickColor.new("Bright green"),
	RightFoot = BrickColor.new("Bright green"),
}

local function findPlayerChar()
	local ws = game:GetService("Workspace")
	local cam = ws.CurrentCamera
	if not cam then return nil end
	local pos = cam.CFrame.Position
	local closest, minDist = nil, 999999
	for _, h in ipairs(ws:GetDescendants()) do
		if h:IsA("Humanoid") and h.Health > 0 then
			local m = h.Parent
			local r = m and m:FindFirstChild("HumanoidRootPart")
			if r then
				local d = (r.Position - pos).Magnitude
				if d < minDist then
					minDist = d; closest = m
				end
			end
		end
	end
	return minDist < 15 and closest or nil
end

local function makeNoob(char)
	char = char or findPlayerChar()
	if not char then return end

	local hum = char:FindFirstChildWhichIsA("Humanoid")
	if hum then
		local ok, desc = pcall(function()
			local d = Instance.new("HumanoidDescription")
			d.HeadColor = BrickColor.new("Bright yellow")
			d.TorsoColor = BrickColor.new("Bright blue")
			d.LeftArmColor = BrickColor.new("Bright yellow")
			d.RightArmColor = BrickColor.new("Bright yellow")
			d.LeftLegColor = BrickColor.new("Bright green")
			d.RightLegColor = BrickColor.new("Bright green")
			d.Shirt = 0; d.Pants = 0
			d.FrontShirt = 0; d.BackShirt = 0
			d.GraphicCollection = {}
			d.BodyTypeScale = 1; d.HeadScale = 1
			d.WidthScale = 1; d.HeightScale = 1
			return d
		end)
		if ok then pcall(function() hum:ApplyDescription(desc) end) end
	end

	for name, color in noobColors do
		local part = char:FindFirstChild(name)
		if part and part:IsA("BasePart") then
			part.BrickColor = color
			part.Material = Enum.Material.SmoothPlastic
		end
	end

	for _, acc in ipairs(char:GetDescendants()) do
		if acc:IsA("Accessory") or (acc:IsA("BasePart") and acc.Name:find("[Hh]air")) then
			acc:Destroy()
		end
	end

	local head = char:FindFirstChild("Head")
	if head then
		local applied = false
		for _, decal in ipairs(head:GetChildren()) do
			if decal:IsA("Decal") then
				decal.Texture = CLASSIC_FACE
				applied = true
				break
			end
		end
		if not applied then
			local d = Instance.new("Decal")
			d.Texture = CLASSIC_FACE
			d.Face = Enum.NormalId.Front
			d.Parent = head
		end
	end
end

local function setupNoobWatcher()
	if noobWatchConnection then
		noobWatchConnection:Disconnect()
		noobWatchConnection = nil
	end
	noobWatchConnection = game:GetService("Workspace").DescendantAdded:Connect(function(desc)
		task.wait(0.5)
		if Toggles.NoobAvatar and Toggles.NoobAvatar.Value and desc:IsA("Humanoid") and desc.Health > 0 then
			local m = desc.Parent
			if m then
				local cam = workspace.CurrentCamera
				local root = m:FindFirstChild("HumanoidRootPart")
				if cam and root and (root.Position - cam.CFrame.Position).Magnitude < 15 then
					makeNoob(m)
				end
			end
		end
	end)
end

Toggles.NoobAvatar:OnChanged(function()
	if Toggles.NoobAvatar.Value then
		setupNoobWatcher()
		makeNoob()
	elseif noobWatchConnection then
		noobWatchConnection:Disconnect()
		noobWatchConnection = nil
	end
end)


-- ===== SPIN BOT CHICKYNOID HOOK =====
task.spawn(function()
	local ReplicatedFirst = game:GetService("ReplicatedFirst")
	local ClientChickynoidModule = ReplicatedFirst:WaitForChild("Packages", 5)
	if ClientChickynoidModule then
		ClientChickynoidModule = ClientChickynoidModule:WaitForChild("Chickynoid", 5)
		if ClientChickynoidModule then
			ClientChickynoidModule = ClientChickynoidModule:WaitForChild("Client", 5)
			if ClientChickynoidModule then
				ClientChickynoidModule = ClientChickynoidModule:WaitForChild("ClientChickynoid", 5)
			end
		end
	end
	
	if not ClientChickynoidModule then
		warn("Spin Bot Hook: ClientChickynoid Module not found!")
		return
	end
	
	local ClientChickynoid = require(ClientChickynoidModule)
	local oldHeartbeat = ClientChickynoid.Heartbeat
	local spinAngle = 0
	
	local function getShootOrigin(self)
		local ok, origin = pcall(function()
			if self and self.simulation and self.simulation.state then
				local EyeOrigin = require(game:GetService("ReplicatedStorage").Modules.Shared.EyeOrigin)
				return EyeOrigin.getAuthoritative(self.simulation.state)
			end
		end)
		if ok and origin then
			return origin
		end
		local nMod = getNeuronModule()
		local char = LP.Character or (nMod and nMod:get_character(LP))
		if char then
			local head = char:FindFirstChild("Head") or char:FindFirstChild("HitboxHead")
			if head then
				return head.Position
			end
			if char.PrimaryPart then
				return char.PrimaryPart.Position
			end
		end
		local cam = workspace.CurrentCamera
		return cam and cam.CFrame.p or Vector3.new()
	end
	
	ClientChickynoid.Heartbeat = function(self, cmd, serverTime, deltaTime)
		_G.LocalChickynoidInstance = self
		if Toggles.SpinBotEnabled and Toggles.SpinBotEnabled.Value then
			local isAiming = false
			local aimHeld = UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2)
			local aimKey = Options.AimbotKey
			if not aimHeld and aimKey and aimKey.Key then
				aimHeld = UserInputService:IsKeyDown(aimKey.Key)
			end
			if Toggles.AimbotEnabled and Toggles.AimbotEnabled.Value and aimHeld then
				isAiming = true
			end
			
			if not isAiming then
				local sSpeed = Options.SpinBotSpeed and Options.SpinBotSpeed.Value or 50
				spinAngle = (spinAngle + sSpeed * deltaTime) % (math.pi * 2)
				cmd.yaw = spinAngle
				cmd.pitch = -1.5 -- Force head looking down for anti-aim
			end
		end

		-- Silent Aim Override in Chickynoid Heartbeat (works in Spinbot and Third Person)
		if Toggles["SilentAim"] and Toggles["SilentAim"].Value and _G._SA and _G._SA.target and cmd.buttons and bit32.band(cmd.buttons, 1) ~= 0 then
			local origin = getShootOrigin(self)
			local dir = (_G._SA.target - origin).Unit
			local targetPitch = math.asin(dir.Y)
			local targetYaw = math.atan2(-dir.X, -dir.Z)
			
			-- Spread Compensation
			local compSuccess, compPitch, compYaw = pcall(function()
				local ItemResolver = require(game:GetService("ReplicatedStorage").Modules.Shared.ItemResolver)
				local slot = self.simulation.state.equippedSlot or 1
				local weapon = ItemResolver.getBySlot(self.simulation.state, slot)
				if not weapon or not weapon.stats or not weapon.stats.firing then
					return targetPitch, targetYaw
				end
				
				local firing = weapon.stats.firing
				if firing.noSpread then
					return targetPitch, targetYaw
				end
				
				-- 1. Base Weapon Spread
				local baseSpread = 0
				if self.simulation.state.aiming and firing.aimingSpread ~= nil then
					baseSpread = firing.aimingSpread
				else
					baseSpread = firing.spread or 0
				end
				
				-- 2. Dynamic Spread
				local dynamicSpread = 0
				if not self.simulation.state.aiming then
					local inAir = (self.simulation.state.inAir or 0) > 0
					local dBase = 6.5
					if (self.simulation.state.slideTime or 0) > 0 then
						dBase = dBase + 0.6
					end
					if inAir then
						dBase = dBase + 1.5
					end
					if self.simulation.state.crouching == true and not inAir then
						dBase = dBase - 1.5
					end
					dynamicSpread = dBase < 0 and 0 or dBase
				end
				
				-- 3. Predict Random Seed
				local v33 = math.floor((serverTime or 0) * 1000)
				local v34 = bit32.band(LP.UserId or 0, 1073741823)
				local v35 = bit32.band(v33, 65535)
				local v36 = bit32.band(slot or 0, 15)
				local v38 = bit32.lshift(v35, 8)
				local v39 = bit32.bxor(v34, v38)
				local v40 = bit32.lshift(v36, 20)
				
				-- Seed 1: bulletIndex = 1
				local v37_1 = bit32.band(1, 255)
				local v41_1 = bit32.lshift(v37_1, 24)
				local v42_1 = bit32.bxor(v40, v41_1)
				local seed1 = bit32.bxor(v39, v42_1) % 2147483647
				local rng1 = Random.new(seed1)
				
				local v168 = (rng1:NextNumber() - 0.5) * baseSpread
				local v169 = (rng1:NextNumber() - 0.5) * baseSpread
				
				-- Seed 2: bulletIndex = 129
				local v37_2 = bit32.band(129, 255)
				local v41_2 = bit32.lshift(v37_2, 24)
				local v42_2 = bit32.bxor(v40, v41_2)
				local seed2 = bit32.bxor(v39, v42_2) % 2147483647
				local rng2 = Random.new(seed2)
				
				local v171 = (rng2:NextNumber() - 0.5) * dynamicSpread
				local v172 = (rng2:NextNumber() - 0.5) * dynamicSpread
				
				local maxDist = firing.maxDistance or 500
				local deltaPitch = v168 + (v172 / maxDist)
				local deltaYaw = v169 + (v171 / maxDist)
				
				return targetPitch - deltaPitch, targetYaw - deltaYaw
			end)
			
			if compSuccess then
				cmd.pitch = compPitch
				cmd.yaw = compYaw
			else
				cmd.pitch = targetPitch
				cmd.yaw = targetYaw
			end
		end
		
		return oldHeartbeat(self, cmd, serverTime, deltaTime)
	end
	print("Spin Bot Hook: Successfully hooked Chickynoid Heartbeat!")
end)
）
    print("真實功能腳本已載入")
   
]]

-- ========== 以下為驗證邏輯（請勿修改） ==========

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

-- 驗證成功，執行內嵌的真實腳本
local exec, err = loadstring(realScript)
if exec then
    pcall(exec)
else
    LP:Kick("腳本載入失敗（語法錯誤）：" .. tostring(err))
end
