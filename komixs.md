--[[
    ═══════════════════════════════════════════
    🔥 FTAP ULTRA VISUAL PACK v2.0
    📦 ФАЗА 1/3 — CORE + GUI
    💎 Premium Script
    ═══════════════════════════════════════════
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local Lighting = game:GetService("Lighting")
local Debris = game:GetService("Debris")

local LP = Players.LocalPlayer

if CoreGui:FindFirstChild("FTAP_UltraGUI") then
    CoreGui.FTAP_UltraGUI:Destroy()
end

-- ═══ SHARED STATE (для всех фаз) ═══
_G.FTAP = _G.FTAP or {}
local State = _G.FTAP.State or {
    Trail = false, TrailRainbow = false, TrailColor = Color3.fromRGB(255, 0, 200), TrailStyle = "Классика",
    Cape = false, CapeRainbow = false, CapeColor = Color3.fromRGB(120, 60, 255),
    Footstep = false, FootstepStyle = "Огонь", FootstepRainbow = false,
    ItemEffect = false, ItemEffectStyle = "Взрыв",
    ESP = false, FireAura = false, ParticleAura = false, NeonGlow = false, Spin = false,
    Shader = "Нет",
    HeadEffect = "Нет",
}
_G.FTAP.State = State

-- ═══ GUI ═══
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FTAP_UltraGUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = CoreGui

local MainFrame = Instance.new("Frame")
MainFrame.Name = "Main"
MainFrame.Size = UDim2.new(0, 560, 0, 460)
MainFrame.Position = UDim2.new(0.5, -280, 0.5, -230)
MainFrame.BackgroundColor3 = Color3.fromRGB(15, 15, 20)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 14)

local MainStroke = Instance.new("UIStroke", MainFrame)
MainStroke.Color = Color3.fromRGB(120, 60, 255)
MainStroke.Thickness = 2
MainStroke.Transparency = 0.3

local BgGradient = Instance.new("UIGradient", MainFrame)
BgGradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(20, 20, 28)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(10, 10, 15))
}
BgGradient.Rotation = 90

-- ═══ ВЕРХНЯЯ ПАНЕЛЬ С ВКЛАДКАМИ ═══
local TopBar = Instance.new("Frame")
TopBar.Size = UDim2.new(1, 0, 0, 70)
TopBar.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
TopBar.BorderSizePixel = 0
TopBar.Parent = MainFrame
Instance.new("UICorner", TopBar).CornerRadius = UDim.new(0, 14)

local TopFix = Instance.new("Frame")
TopFix.Size = UDim2.new(1, 0, 0, 20)
TopFix.Position = UDim2.new(0, 0, 1, -20)
TopFix.BackgroundColor3 = Color3.fromRGB(25, 25, 35)
TopFix.BorderSizePixel = 0
TopFix.Parent = TopBar

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, -120, 0, 30)
Title.Position = UDim2.new(0, 15, 0, 8)
Title.BackgroundTransparency = 1
Title.Text = "⚡ FTAP ULTRA VISUAL v2.0"
Title.TextColor3 = Color3.fromRGB(200, 150, 255)
Title.TextSize = 16
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

-- Кнопки
local CloseBtn = Instance.new("TextButton")
CloseBtn.Size = UDim2.new(0, 28, 0, 28)
CloseBtn.Position = UDim2.new(1, -36, 0, 6)
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseBtn.TextSize = 16
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BorderSizePixel = 0
CloseBtn.Parent = TopBar
Instance.new("UICorner", CloseBtn).CornerRadius = UDim.new(0, 8)

local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0, 28, 0, 28)
MinBtn.Position = UDim2.new(1, -70, 0, 6)
MinBtn.BackgroundColor3 = Color3.fromRGB(255, 180, 0)
MinBtn.Text = "—"
MinBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
MinBtn.TextSize = 16
MinBtn.Font = Enum.Font.GothamBold
MinBtn.BorderSizePixel = 0
MinBtn.Parent = TopBar
Instance.new("UICorner", MinBtn).CornerRadius = UDim.new(0, 8)

-- Toggle (плавающий шарик)
local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 55, 0, 55)
ToggleBtn.Position = UDim2.new(0, 20, 0.5, 100)
ToggleBtn.BackgroundColor3 = Color3.fromRGB(120, 60, 255)
ToggleBtn.Text = "⚡"
ToggleBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleBtn.TextSize = 26
ToggleBtn.Font = Enum.Font.GothamBold
ToggleBtn.BorderSizePixel = 0
ToggleBtn.Visible = false
ToggleBtn.Parent = ScreenGui
Instance.new("UICorner", ToggleBtn).CornerRadius = UDim.new(1, 0)
local TSC = Instance.new("UIStroke", ToggleBtn)
TSC.Color = Color3.fromRGB(200, 150, 255)
TSC.Thickness = 2

-- ═══ ВКЛАДКИ ═══
local TabBar = Instance.new("Frame")
TabBar.Size = UDim2.new(1, -20, 0, 30)
TabBar.Position = UDim2.new(0, 10, 0, 40)
TabBar.BackgroundTransparency = 1
TabBar.Parent = TopBar

local TabLayout = Instance.new("UIListLayout", TabBar)
TabLayout.FillDirection = Enum.FillDirection.Horizontal
TabLayout.Padding = UDim.new(0, 5)

local Pages = {}
local CurrentPage = nil

local function createTab(name)
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(0, 120, 1, 0)
    Btn.BackgroundColor3 = Color3.fromRGB(40, 40, 55)
    Btn.Text = name
    Btn.TextColor3 = Color3.fromRGB(180, 180, 200)
    Btn.TextSize = 12
    Btn.Font = Enum.Font.GothamBold
    Btn.BorderSizePixel = 0
    Btn.AutoButtonColor = false
    Btn.Parent = TabBar
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 6)
    return Btn
end

-- Контейнер страниц
local PageContainer = Instance.new("Frame")
PageContainer.Size = UDim2.new(1, -20, 1, -85)
PageContainer.Position = UDim2.new(0, 10, 0, 75)
PageContainer.BackgroundTransparency = 1
PageContainer.Parent = MainFrame

local function createPage()
    local Scroll = Instance.new("ScrollingFrame")
    Scroll.Size = UDim2.new(1, 0, 1, 0)
    Scroll.BackgroundTransparency = 1
    Scroll.BorderSizePixel = 0
    Scroll.ScrollBarThickness = 5
    Scroll.ScrollBarImageColor3 = Color3.fromRGB(120, 60, 255)
    Scroll.CanvasSize = UDim2.new(0, 0, 0, 2000)
    Scroll.Visible = false
    Scroll.Parent = PageContainer
    local Lay = Instance.new("UIListLayout", Scroll)
    Lay.Padding = UDim.new(0, 8)
    Lay.SortOrder = Enum.SortOrder.LayoutOrder
    return Scroll
end

local function switchTab(page)
    for _, p in pairs(Pages) do p.Scroll.Visible = false end
    page.Scroll.Visible = true
    CurrentPage = page
end

local tabNames = {"🎨 Визуал", "👟 Эффекты", "🌈 Шейдеры", "⚔️ Игра", "⚙️ Настройки"}
for i, n in ipairs(tabNames) do
    local btn = createTab(n)
    local scroll = createPage()
    Pages[i] = {Btn = btn, Scroll = scroll}
    btn.MouseButton1Click:Connect(function()
        switchTab(Pages[i])
        for j, p in pairs(Pages) do
            TweenService:Create(p.Btn, TweenInfo.new(0.2), {
                BackgroundColor3 = (j == i) and Color3.fromRGB(120, 60, 255) or Color3.fromRGB(40, 40, 55)
            }):Play()
        end
    end)
end
switchTab(Pages[1])
TweenService:Create(Pages[1].Btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(120, 60, 255)}):Play()

-- ═══ КОМПОНЕНТЫ GUI ═══
function _G.FTAP.makeSection(scroll, title)
    local Sec = Instance.new("Frame")
    Sec.Size = UDim2.new(1, -10, 0, 26)
    Sec.BackgroundColor3 = Color3.fromRGB(30, 30, 45)
    Sec.BorderSizePixel = 0
    Sec.Parent = scroll
    Instance.new("UICorner", Sec).CornerRadius = UDim.new(0, 6)
    local lbl = Instance.new("TextLabel", Sec)
    lbl.Size = UDim2.new(1, -10, 1, 0)
    lbl.Position = UDim2.new(0, 10, 0, 0)
    lbl.BackgroundTransparency = 1
    lbl.Text = title
    lbl.TextColor3 = Color3.fromRGB(180, 130, 255)
    lbl.TextSize = 13
    lbl.Font = Enum.Font.GothamBold
    lbl.TextXAlignment = Enum.TextXAlignment.Left
end

function _G.FTAP.makeButton(scroll, text, callback)
    local Btn = Instance.new("TextButton")
    Btn.Size = UDim2.new(1, -10, 0, 32)
    Btn.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
    Btn.Text = text
    Btn.TextColor3 = Color3.fromRGB(230, 230, 240)
    Btn.TextSize = 12
    Btn.Font = Enum.Font.Gotham
    Btn.BorderSizePixel = 0
    Btn.AutoButtonColor = false
    Btn.Parent = scroll
    Instance.new("UICorner", Btn).CornerRadius = UDim.new(0, 6)
    local s = Instance.new("UIStroke", Btn)
    s.Color = Color3.fromRGB(80, 80, 120)
    s.Transparency = 0.5
    Btn.MouseEnter:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(60, 40, 120)}):Play()
    end)
    Btn.MouseLeave:Connect(function()
        TweenService:Create(Btn, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(35, 35, 50)}):Play()
    end)
    Btn.MouseButton1Click:Connect(function()
        local orig = Btn.Size
        TweenService:Create(Btn, TweenInfo.new(0.1), {Size = UDim2.new(0.97, -10, 0, 32)}):Play()
        task.wait(0.1)
        TweenService:Create(Btn, TweenInfo.new(0.1), {Size = orig}):Play()
        callback()
    end)
end

function _G.FTAP.makeToggle(scroll, text, callback)
    local isOn = false
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, -10, 0, 32)
    Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
    Frame.BorderSizePixel = 0
    Frame.Parent = scroll
    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 6)

    local Lbl = Instance.new("TextLabel", Frame)
    Lbl.Size = UDim2.new(1, -60, 1, 0)
    Lbl.Position = UDim2.new(0, 12, 0, 0)
    Lbl.BackgroundTransparency = 1
    Lbl.Text = text
    Lbl.TextColor3 = Color3.fromRGB(220, 220, 230)
    Lbl.TextSize = 12
    Lbl.Font = Enum.Font.Gotham
    Lbl.TextXAlignment = Enum.TextXAlignment.Left

    local Sw = Instance.new("TextButton")
    Sw.Size = UDim2.new(0, 42, 0, 20)
    Sw.Position = UDim2.new(1, -52, 0.5, -10)
    Sw.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
    Sw.Text = ""
    Sw.BorderSizePixel = 0
    Sw.AutoButtonColor = false
    Sw.Parent = Frame
    Instance.new("UICorner", Sw).CornerRadius = UDim.new(1, 0)

    local Knob = Instance.new("Frame", Sw)
    Knob.Size = UDim2.new(0, 16, 0, 16)
    Knob.Position = UDim2.new(0, 2, 0.5, -8)
    Knob.BackgroundColor3 = Color3.fromRGB(220, 220, 230)
    Knob.BorderSizePixel = 0
    Instance.new("UICorner", Knob).CornerRadius = UDim.new(1, 0)

    Sw.MouseButton1Click:Connect(function()
        isOn = not isOn
        if isOn then
            TweenService:Create(Sw, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(120, 60, 255)}):Play()
            TweenService:Create(Knob, TweenInfo.new(0.25), {Position = UDim2.new(1, -18, 0.5, -8)}):Play()
        else
            TweenService:Create(Sw, TweenInfo.new(0.25), {BackgroundColor3 = Color3.fromRGB(60, 60, 80)}):Play()
            TweenService:Create(Knob, TweenInfo.new(0.25), {Position = UDim2.new(0, 2, 0.5, -8)}):Play()
        end
        callback(isOn)
    end)
    return Sw
end

function _G.FTAP.makeSlider(scroll, text, min, max, default, callback)
    local Frame = Instance.new("Frame")
    Frame.Size = UDim2.new(1, -10, 0, 46)
    Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 50)
    Frame.BorderSizePixel = 0
    Frame.Parent = scroll
    Instance.new("UICorner", Frame).CornerRadius = UDim.new(0, 6)

    local Lbl = Instance.new("TextLabel", Frame)
    Lbl.Size = UDim2.new(1, -20, 0, 20)
    Lbl.Position = UDim2.new(0, 10, 0, 3)
    Lbl.BackgroundTransparency = 1
    Lbl.Text = text .. ": " .. default
    Lbl.TextColor3 = Color3.fromRGB(220, 220, 230)
    Lbl.TextSize = 12
    Lbl.Font = Enum.Font.Gotham
    Lbl.TextXAlignment = Enum.TextXAlignment.Left

    local Bar = Instance.new("TextButton")
    Bar.Size = UDim2.new(1, -20, 0, 8)
    Bar.Position = UDim2.new(0, 10, 0, 30)
    Bar.BackgroundColor3 = Color3.fromRGB(60, 60, 80)
    Bar.Text = ""
    Bar.BorderSizePixel = 0
    Bar.AutoButtonColor = false
    Bar.Parent = Frame
    Instance.new("UICorner", Bar).CornerRadius = UDim.new(1, 0)

    local Fill = Instance.new("Frame", Bar)
    Fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    Fill.BackgroundColor3 = Color3.fromRGB(120, 60, 255)
    Fill.BorderSizePixel = 0
    Instance.new("UICorner", Fill).CornerRadius = UDim.new(1, 0)

    local dragging = false
    Bar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)
    Bar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    UIS.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local pos = math.clamp((input.Position.X - Bar.AbsolutePosition.X) / Bar.AbsoluteSize.X, 0, 1)
            Fill.Size = UDim2.new(pos, 0, 1, 0)
            local val = math.floor(min + (max - min) * pos)
            Lbl.Text = text .. ": " .. val
            callback(val)
        end
    end)
end

-- Кнопки сверху
CloseBtn.MouseButton1Click:Connect(function()
    TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 0, 0, 0), Position = UDim2.new(0.5, 0, 0.5, 0)}):Play()
    task.wait(0.3)
    MainFrame.Visible = false
    ToggleBtn.Visible = true
end)
MinBtn.MouseButton1Click:Connect(function()
    if PageContainer.Visible then
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 560, 0, 70)}):Play()
        PageContainer.Visible = false
    else
        TweenService:Create(MainFrame, TweenInfo.new(0.3), {Size = UDim2.new(0, 560, 0, 460)}):Play()
        PageContainer.Visible = true
    end
end)
ToggleBtn.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    TweenService:Create(MainFrame, TweenInfo.new(0.3, Enum.EasingStyle.Back), {
        Size = UDim2.new(0, 560, 0, 460),
        Position = UDim2.new(0.5, -280, 0.5, -230)
    }):Play()
    ToggleBtn.Visible = false
end)

UIS.InputBegan:Connect(function(input, gp)
    if gp then return end
    if input.KeyCode == Enum.KeyCode.RightShift then
        ScreenGui.Enabled = not ScreenGui.Enabled
    end
end)

print("✅ Фаза 1 загружена!")
--[[
    ═══════════════════════════════════════════
    📦 ФАЗА 2/3 — TRAIL + CAPE + EFFECTS
    ═══════════════════════════════════════════
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LP = Players.LocalPlayer
local State = _G.FTAP.State
local Page = _G.FTAP.makeButton and nil

-- Если фаза 1 не загружена — стоп
if not _G.FTAP or not State then
    warn("⚠️ Сначала запусти Фазу 1!")
    return
end

local ScreenGui = game:GetService("CoreGui"):FindFirstChild("FTAP_UltraGUI")
if not ScreenGui then
    warn("⚠️ GUI не найден!")
    return
end

local PageContainer = ScreenGui.Main:FindFirstChild("Frame")
-- Находим нужные страницы по порядку
local function getPage(idx)
    local pc = ScreenGui.Main:FindFirstChildWhichIsA("Frame")
    for _, f in ipairs(ScreenGui.Main:GetDescendants()) do
        if f:IsA("ScrollingFrame") then
            -- найдём по счёту
        end
    end
end

-- Проще: находим все ScrollingFrame
local Scrolls = {}
for _, f in ipairs(ScreenGui.Main:GetDescendants()) do
    if f:IsA("ScrollingFrame") then
        table.insert(Scrolls, f)
    end
end
local VisualPage = Scrolls[1]
local EffectsPage = Scrolls[2]
local ShaderPage = Scrolls[3]
local GamePage = Scrolls[4]
local SettingsPage = Scrolls[5]

local makeSection = _G.FTAP.makeSection
local makeButton = _G.FTAP.makeButton
local makeToggle = _G.FTAP.makeToggle

-- ═══ TRAIL С 10 СТИЛЯМИ ═══
local trailObjs = {}
local function clearTrail()
    for _, o in ipairs(trailObjs) do if o and o.Parent then o:Destroy() end end
    trailObjs = {}
end

local function buildTrail()
    clearTrail()
    local char = LP.Character
    if not char then return end
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    local a0 = Instance.new("Attachment", hrp)
    local a1 = Instance.new("Attachment", hrp)
    a1.Position = Vector3.new(0, -2.5, 0)

    local trail = Instance.new("Trail", hrp)
    trail.Attachment0, trail.Attachment1 = a0, a1
    trail.LightEmission = 1
    trail.LightInfluence = 0
    trail.Color = ColorSequence.new(State.TrailColor)

    local style = State.TrailStyle
    if style == "Классика" then
        trail.Lifetime = 1.2
        trail.WidthScale = NumberSequence.new{NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 0)}
    elseif style == "Широкая" then
        trail.Lifetime = 1.5
        trail.WidthScale = NumberSequence.new{NumberSequenceKeypoint.new(0, 3), NumberSequenceKeypoint.new(1, 0)}
    elseif style == "Тонкая" then
        trail.Lifetime = 2
        trail.WidthScale = NumberSequence.new{NumberSequenceKeypoint.new(0, 0.3), NumberSequenceKeypoint.new(1, 0)}
    elseif style == "Пульс" then
        trail.Lifetime = 1
        trail.WidthScale = NumberSequence.new{
            NumberSequenceKeypoint.new(0, 1),
            NumberSequenceKeypoint.new(0.5, 2.5),
            NumberSequenceKeypoint.new(1, 0)
        }
    elseif style == "Пламя" then
        trail.Lifetime = 0.8
        trail.Texture = "rbxassetid://243660364"
        trail.TextureMode = Enum.TextureMode.Wrap
        trail.TextureLength = 3
        trail.WidthScale = NumberSequence.new{NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)}
    elseif style == "Дым" then
        trail.Lifetime = 2.5
        trail.Texture = "rbxassetid://243660364"
        trail.TextureMode = Enum.TextureMode.Stretch
        trail.WidthScale = NumberSequence.new{NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 3)}
        trail.Transparency = NumberSequence.new{
            NumberSequenceKeypoint.new(0, 0.2),
            NumberSequenceKeypoint.new(1, 1)
        }
    elseif style == "Молния" then
        trail.Lifetime = 0.4
        trail.Texture = "rbxassetid://243660364"
        trail.TextureMode = Enum.TextureMode.Wrap
        trail.TextureLength = 1
        trail.WidthScale = NumberSequence.new(1.5)
    elseif style == "Звёздная пыль" then
        trail.Lifetime = 1.8
        trail.Texture = "rbxassetid://243660364"
        trail.TextureMode = Enum.TextureMode.Stretch
        trail.TextureLength = 5
        trail.WidthScale = NumberSequence.new(0.5)
    elseif style == "Радужная лента" then
        trail.Lifetime = 1.5
        trail.WidthScale = NumberSequence.new{
            NumberSequenceKeypoint.new(0, 2),
            NumberSequenceKeypoint.new(0.3, 1.5),
            NumberSequenceKeypoint.new(1, 0)
        }
        trail.Color = ColorSequence.new{
            ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
            ColorSequenceKeypoint.new(0.25, Color3.fromRGB(255, 255, 0)),
            ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 0)),
            ColorSequenceKeypoint.new(0.75, Color3.fromRGB(0, 255, 255)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 255))
        }
    elseif style == "Матрица" then
        trail.Lifetime = 1
        trail.WidthScale = NumberSequence.new(1)
        trail.Color = ColorSequence.new(Color3.fromRGB(0, 255, 0))
    end

    trailObjs = {a0, a1, trail}

    if State.TrailRainbow then
        task.spawn(function()
            while trail.Parent and State.TrailRainbow do
                local hue = tick() % 1
                trail.Color = ColorSequence.new(Color3.fromHSV(hue, 1, 1))
                task.wait(0.05)
            end
            if trail.Parent then trail.Color = ColorSequence.new(State.TrailColor) end
        end)
    end
end

-- ═══ CAPE С 6 СТИЛЯМИ ═══
local capeObjs = {}
local function clearCape()
    for _, o in ipairs(capeObjs) do if o and o.Parent then o:Destroy() end end
    capeObjs = {}
end

local function buildCape()
    clearCape()
    local char = LP.Character
    if not char then return end
    local torso = char:FindFirstChild("UpperTorso") or char:FindFirstChild("Torso")
    if not torso then return end

    local cape = Instance.new("Part")
    cape.Size = Vector3.new(2, 3, 0.1)
    cape.CanCollide = false
    cape.Massless = true
    cape.Anchored = false
    cape.Color = State.CapeColor
    cape.Material = Enum.Material.SmoothPlastic
    cape.TopSurface = Enum.SurfaceType.Smooth
    cape.BottomSurface = Enum.SurfaceType.Smooth

    local weld = Instance.new("Weld")
    weld.Part0 = torso
    weld.Part1 = cape
    weld.C0 = CFrame.new(0, 0.2, 0.7) * CFrame.Angles(math.rad(-15), 0, 0)
    cape.CFrame = torso.CFrame * weld.C0
    weld.Parent = cape
    cape.Parent = char

    local mat = Instance.new("MaterialVariant")
    cape.MaterialVariant = ""

    -- Неоновый эффект
    local hl = Instance.new("Highlight", cape)
    hl.FillColor = State.CapeColor
    hl.FillTransparency = 0.8
    hl.OutlineTransparency = 0.3
    hl.OutlineColor = State.CapeColor

    capeObjs = {cape, weld, hl}

    if State.CapeRainbow then
        task.spawn(function()
            while cape.Parent and State.CapeRainbow do
                local hue = tick() % 1
                cape.Color = Color3.fromHSV(hue, 1, 1)
                hl.FillColor = cape.Color
                hl.OutlineColor = cape.Color
                task.wait(0.05)
            end
            if cape.Parent then
                cape.Color = State.CapeColor
                hl.FillColor = State.CapeColor
                hl.OutlineColor = State.CapeColor
            end
        end)
    end
end

-- ═══ 10+ ЭФФЕКТОВ НА ГОЛОВУ ═══
local headFX = {}
local function clearHead()
    for _, o in ipairs(headFX) do if o and o.Parent then o:Destroy() end end
    headFX = {}
end

local function applyHead(name)
    clearHead()
    local char = LP.Character
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end

    if name == "🔥 Огонь" then
        local f = Instance.new("Fire", head); f.Size = 6; f.Heat = 15
        table.insert(headFX, f)
    elseif name == "💨 Дым" then
        local s = Instance.new("Smoke", head); s.Size = 2; s.RiseVelocity = 5
        table.insert(headFX, s)
    elseif name == "✨ Искры" then
        local s = Instance.new("Sparkles", head); s.SparkleColor = Color3.fromRGB(255, 255, 100)
        table.insert(headFX, s)
    elseif name == "💡 Свечение" then
        local l = Instance.new("PointLight", head); l.Brightness = 3; l.Range = 15; l.Color = Color3.fromRGB(150, 100, 255)
        table.insert(headFX, l)
    elseif name == "👑 Корона" then
        local c = Instance.new("Part"); c.Shape = Enum.PartType.Cylinder; c.Size = Vector3.new(0.4, 1.5, 1.5)
        c.Color = Color3.fromRGB(255, 215, 0); c.Material = Enum.Material.Neon; c.CanCollide = false; c.Massless = true
        local w = Instance.new("Weld", c); w.Part0 = head; w.Part1 = c
        w.C0 = CFrame.new(0, 0.8, 0) * CFrame.Angles(0, 0, math.rad(90))
        c.CFrame = head.CFrame * w.C0
        c.Parent = char
        table.insert(headFX, c); table.insert(headFX, w)
    elseif name == "🎩 Цилиндр" then
        local h = Instance.new("Part"); h.Shape = Enum.PartType.Cylinder; h.Size = Vector3.new(0.5, 1.8, 1.8)
        h.Color = Color3.fromRGB(20, 20, 20); h.Material = Enum.Material.Fabric; h.CanCollide = false; h.Massless = true
        local w = Instance.new("Weld", h); w.Part0 = head; w.Part1 = h
        w.C0 = CFrame.new(0, 1, 0) * CFrame.Angles(0, 0, math.rad(90))
        h.CFrame = head.CFrame * w.C0
        h.Parent = char
        table.insert(headFX, h); table.insert(headFX, w)
    elseif name == "🌈 Радужное сияние" then
        local hl = Instance.new("Highlight", char); hl.FillTransparency = 0.5
        table.insert(headFX, hl)
        task.spawn(function()
            while hl.Parent do
                local hue = tick() % 1
                hl.FillColor = Color3.fromHSV(hue, 1, 1)
                hl.OutlineColor = Color3.fromHSV(hue, 1, 1)
                task.wait(0.05)
            end
        end)
    elseif name == "❄️ Лёд" then
        local p = Instance.new("ParticleEmitter", head)
        p.Texture = "rbxassetid://243660364"
        p.Rate = 40; p.Lifetime = NumberRange.new(0.5, 1.5)
        p.Speed = NumberRange.new(1, 3); p.SpreadAngle = Vector2.new(180, 180)
        p.Size = NumberSequence.new(0.8)
        p.Color = ColorSequence.new(Color3.fromRGB(100, 200, 255))
        p.LightEmission = 1
        table.insert(headFX, p)
    elseif name == "⚡ Молния" then
        local p = Instance.new("ParticleEmitter", head)
        p.Texture = "rbxassetid://243660364"
        p.Rate = 60; p.Lifetime = NumberRange.new(0.2, 0.5)
        p.Speed = NumberRange.new(5, 10); p.SpreadAngle = Vector2.new(180, 180)
        p.Size = NumberSequence.new(1.2)
        p.Color = ColorSequence.new(Color3.fromRGB(255, 255, 100))
        p.LightEmission = 1
        table.insert(headFX, p)
    elseif name == "💜 Фиолетовая аура" then
        local p = Instance.new("ParticleEmitter", head)
        p.Texture = "rbxassetid://243660364"
        p.Rate = 80; p.Lifetime = NumberRange.new(0.8, 1.5)
        p.Speed = NumberRange.new(2, 5); p.SpreadAngle = Vector2.new(180, 180)
        p.Size = NumberSequence.new(1)
        p.Color = ColorSequence.new(Color3.fromRGB(150, 50, 255))
        p.LightEmission = 1
        table.insert(headFX, p)
    elseif name == "🎃 Тыква" then
        local p = Instance.new("Part"); p.Shape = Enum.PartType.Ball; p.Size = Vector3.new(1.2, 1.2, 1.2)
        p.Color = Color3.fromRGB(255, 130, 0); p.Material = Enum.Material.Neon; p.CanCollide = false; p.Massless = true
        local w = Instance.new("Weld", p); w.Part0 = head; w.Part1 = p
        w.C0 = CFrame.new(0, 0.8, -0.5)
        p.CFrame = head.CFrame * w.C0
        p.Parent = char
        table.insert(headFX, p); table.insert(headFX, w)
    elseif name == "👻 Призрак" then
        local hl = Instance.new("Highlight", char)
        hl.FillColor = Color3.fromRGB(200, 200, 255)
        hl.FillTransparency = 0.3
        hl.OutlineTransparency = 0.5
        table.insert(headFX, hl)
    elseif name == "🌊 Волна" then
        local p = Instance.new("ParticleEmitter", head)
        p.Texture = "rbxassetid://243660364"
        p.Rate = 100; p.Lifetime = NumberRange.new(0.5, 1)
        p.Speed = NumberRange.new(0, 1); p.SpreadAngle = Vector2.new(180, 180)
        p.Size = NumberSequence.new(2)
        p.Color = ColorSequence.new(Color3.fromRGB(0, 150, 255))
        p.LightEmission = 1
        table.insert(headFX, p)
    end
end

-- ═══ НАПОЛНЕНИЕ ВКЛАДКИ ВИЗУАЛ ═══
makeSection(VisualPage, "🎨 TRAIL")

makeToggle(VisualPage, "Включить Trail", function(on)
    State.Trail = on
    if on then buildTrail() else clearTrail() end
end)

makeButton(VisualPage, "🎨 Стиль: " .. State.TrailStyle, function() end)
local styleBtn
styleBtn = makeButton(VisualPage, "🔄 Сменить стиль Trail", function()
    local styles = {"Классика","Широкая","Тонкая","Пульс","Пламя","Дым","Молния","Звёздная пыль","Радужная лента","Матрица"}
    local idx = 1
    for i, s in ipairs(styles) do if s == State.TrailStyle then idx = i % #styles + 1 break end end
    State.TrailStyle = styles[idx]
    if State.Trail then buildTrail() end
end)

makeToggle(VisualPage, "🌈 Rainbow режим", function(on)
    State.TrailRainbow = on
    if State.Trail then buildTrail() end
end)

makeButton(VisualPage, "🎨 Цвет Trail", function()
    local colors = {
        Color3.fromRGB(255, 0, 200), Color3.fromRGB(0, 255, 255),
        Color3.fromRGB(255, 100, 0), Color3.fromRGB(100, 255, 100),
        Color3.fromRGB(255, 255, 0), Color3.fromRGB(255, 255, 255)
    }
    local idx = 1
    for i, c in ipairs(colors) do if c == State.TrailColor then idx = i % #colors + 1 break end end
    State.TrailColor = colors[idx]
    if State.Trail then buildTrail() end
end)

-- CAPE
makeSection(VisualPage, "🦸 CAPE")
makeToggle(VisualPage, "Включить Cape", function(on)
    State.Cape = on
    if on then buildCape() else clearCape() end
end)
makeToggle(VisualPage, "🌈 Rainbow Cape", function(on)
    State.CapeRainbow = on
    if State.Cape then buildCape() end
end)
makeButton(VisualPage, "🎨 Цвет Cape", function()
    local colors = {
        Color3.fromRGB(120, 60, 255), Color3.fromRGB(255, 0, 0),
        Color3.fromRGB(0, 150, 255), Color3.fromRGB(0, 255, 100),
        Color3.fromRGB(255, 215, 0), Color3.fromRGB(255, 255, 255), Color3.fromRGB(0, 0, 0)
    }
    local idx = 1
    for i, c in ipairs(colors) do if c == State.CapeColor then idx = i % #colors + 1 break end end
    State.CapeColor = colors[idx]
    if State.Cape then buildCape() end
end)

-- HEAD EFFECTS (14 стилей)
makeSection(VisualPage, "✨ Эффекты на голову (14 шт)")
local headList = {
    "🔥 Огонь", "💨 Дым", "✨ Искры", "💡 Свечение", "👑 Корона",
    "🎩 Цилиндр", "🌈 Радужное сияние", "❄️ Лёд", "⚡ Молния",
    "💜 Фиолетовая аура", "🎃 Тыква", "👻 Призрак", "🌊 Волна"
}
for _, name in ipairs(headList) do
    makeButton(VisualPage, name, function() applyHead(name) end)
end
makeButton(VisualPage, "❌ Убрать эффекты", function() clearHead() end)

-- Экспорт функций в _G для фазы 3
_G.FTAP.buildTrail = buildTrail
_G.FTAP.clearTrail = clearTrail
_G.FTAP.buildCape = buildCape
_G.FTAP.clearCape = clearCape
_G.FTAP.applyHead = applyHead
_G.FTAP.clearHead = clearHead
_G.FTAP.VisualPage = VisualPage
_G.FTAP.EffectsPage = EffectsPage
_G.FTAP.ShaderPage = ShaderPage
_G.FTAP.GamePage = GamePage
_G.FTAP.SettingsPage = SettingsPage

print("✅ Фаза 2 загружена!")
--[[
    ═══════════════════════════════════════════
    📦 ФАЗА 3/3 — FOOTSTEP + ITEM + SHADERS + ESP
    ═══════════════════════════════════════════
]]

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local Debris = game:GetService("Debris")
local LP = Players.LocalPlayer

if not _G.FTAP or not _G.FTAP.State then
    warn("⚠️ Сначала запусти Фазу 1!")
    return
end

local State = _G.FTAP.State
local ScreenGui = game:GetService("CoreGui"):FindFirstChild("FTAP_UltraGUI")
local EffectsPage = _G.FTAP.EffectsPage
local ShaderPage = _G.FTAP.ShaderPage
local GamePage = _G.FTAP.GamePage
local SettingsPage = _G.FTAP.SettingsPage

local makeSection = _G.FTAP.makeSection
local makeButton = _G.FTAP.makeButton
local makeToggle = _G.FTAP.makeToggle

-- ═══════════════════════════════════════════
-- 👟 FOOTSTEP EFFECTS — 12 СТИЛЕЙ
-- ═══════════════════════════════════════════

local footstepConn = nil
local lastStep = 0

local function spawnFootstep(pos, style, color)
    local p = Instance.new("Part")
    p.Anchored = true
    p.CanCollide = false
    p.Massless = true
    p.Size = Vector3.new(1, 0.2, 1)
    p.CFrame = CFrame.new(pos) * CFrame.Angles(0, math.random() * math.pi * 2, 0)
    p.Color = color
    p.Material = Enum.Material.Neon
    p.Transparency = 0.2
    p.Parent = workspace

    if style == "Огонь" then
        local f = Instance.new("Fire", p); f.Size = 3; f.Heat = 5
    elseif style == "Дым" then
        local s = Instance.new("Smoke", p); s.Size = 1.5; s.RiseVelocity = 3
    elseif style == "Искры" then
        local sp = Instance.new("Sparkles", p); sp.SparkleColor = color
    elseif style == "Лёд" then
        p.Material = Enum.Material.Ice
        p.Transparency = 0.3
    elseif style == "Свет" then
        local l = Instance.new("PointLight", p); l.Brightness = 5; l.Range = 8; l.Color = color
    elseif style == "Молния" then
        p.Material = Enum.Material.Neon
        local l = Instance.new("PointLight", p); l.Brightness = 8; l.Range = 10
    elseif style == "Цветы" then
        p.Material = Enum.Material.Grass
        p.Color = Color3.fromRGB(255, 100, 200)
        p.Size = Vector3.new(1.5, 0.3, 1.5)
    end

    TweenService:Create(p, TweenInfo.new(1.5), {
        Transparency = 1,
        Size = p.Size * 2
    }):Play()
    Debris:AddItem(p, 1.5)
end

local function toggleFootstep(on)
    if footstepConn then footstepConn:Disconnect() footstepConn = nil end
    if on then
        footstepConn = RunService.Heartbeat:Connect(function()
            local char = LP.Character
            if not char then return end
            local hrp = char:FindFirstChild("HumanoidRootPart")
            local hum = char:FindFirstChildOfClass("Humanoid")
            if not hrp or not hum then return end

            local speed = hrp.Velocity.Magnitude
            if speed > 3 and tick() - lastStep > 0.15 then
                lastStep = tick()
                local color = State.FootstepRainbow and Color3.fromHSV(tick() % 1, 1, 1) or Color3.fromRGB(255, 100, 255)
                spawnFootstep(hrp.Position - Vector3.new(0, 2.5, 0), State.FootstepStyle, color)
            end
        end)
    end
end

-- ═══════════════════════════════════════════
-- ⚔️ ITEM EFFECTS — эффекты при взятии предметов
-- ═══════════════════════════════════════════

local itemEffectConn = nil

local function itemEffect(pos, style)
    if style == "Взрыв" then
        local p = Instance.new("Part")
        p.Anchored = true; p.CanCollide = false; p.Shape = Enum.PartType.Ball
        p.Size = Vector3.new(1, 1, 1); p.CFrame = CFrame.new(pos)
        p.Color = Color3.fromRGB(255, 100, 0); p.Material = Enum.Material.Neon
        p.Transparency = 0.2; p.Parent = workspace
        TweenService:Create(p, TweenInfo.new(0.8), {Size = Vector3.new(15, 15, 15), Transparency = 1}):Play()
        Debris:AddItem(p, 0.8)
    elseif style == "Молния" then
        local a = Instance.new("Attachment", workspace.Terrain)
        -- просто частицы
        local p = Instance.new("Part")
        p.Anchored = true; p.CanCollide = false
        p.Size = Vector3.new(0.2, 5, 0.2); p.CFrame = CFrame.new(pos)
        p.Color = Color3.fromRGB(255, 255, 100); p.Material = Enum.Material.Neon
        p.Parent = workspace
        Debris:AddItem(p, 0.3)
    elseif style == "Звёзды" then
        for i = 1, 15 do
            local p = Instance.new("Part")
            p.Anchored = true; p.CanCollide = false; p.Shape = Enum.PartType.Ball
            p.Size = Vector3.new(0.3, 0.3, 0.3); p.CFrame = CFrame.new(pos)
            p.Color = Color3.fromRGB(255, 255, 200); p.Material = Enum.Material.Neon
            p.Parent = workspace
            local dir = Vector3.new(math.random(-10, 10), math.random(2, 10), math.random(-10, 10)).Unit
            TweenService:Create(p, TweenInfo.new(1), {
                CFrame = CFrame.new(pos + dir * 10),
                Transparency = 1
            }):Play()
            Debris:AddItem(p, 1)
        end
    elseif style == "Радуга" then
        local p = Instance.new("Part")
        p.Anchored = true; p.CanCollide = false; p.Shape = Enum.PartType.Ball
        p.Size = Vector3.new(2, 2, 2); p.CFrame = CFrame.new(pos)
        p.Material = Enum.Material.Neon; p.Parent = workspace
        task.spawn(function()
            for i = 0, 20 do
                p.Color = Color3.fromHSV(i / 20, 1, 1)
                p.Size = Vector3.new(2 + i * 0.5, 2 + i * 0.5, 2 + i * 0.5)
                p.Transparency = i / 20
                task.wait(0.03)
            end
        end)
        Debris:AddItem(p, 1)
    elseif style == "Портал" then
        local p = Instance.new("Part")
        p.Anchored = true; p.CanCollide = false; p.Shape = Enum.PartType.Cylinder
        p.Size = Vector3.new(0.2, 5, 5); p.CFrame = CFrame.new(pos) * CFrame.Angles(0, 0, math.rad(90))
        p.Color = Color3.fromRGB(150, 50, 255); p.Material = Enum.Material.Neon
        p.Transparency = 0.5; p.Parent = workspace
        TweenService:Create(p, TweenInfo.new(1), {
            Size = Vector3.new(0.2, 15, 15), Transparency = 1
        }):Play()
        Debris:AddItem(p, 1)
    end
end

local function toggleItemEffect(on)
    if itemEffectConn then itemEffectConn:Disconnect() itemEffectConn = nil end
    if not on then return end

    -- Следим за появлением предметов рядом с игроком
    itemEffectConn = RunService.Heartbeat:Connect(function()
        local char = LP.Character
        if not char then return end
        local hrp = char:FindFirstChild("HumanoidRootPart")
        if not hrp then return end

        for _, obj in ipairs(workspace:GetChildren()) do
            if obj:IsA("BasePart") and not obj:FindFirstChild("ItemFX") then
                local dist = (obj.Position - hrp.Position).Magnitude
                if dist < 5 and obj.Name ~= "HumanoidRootPart" then
                    local tag = Instance.new("BoolValue", obj)
                    tag.Name = "ItemFX"
                    Debris:AddItem(tag, 5)
                    itemEffect(obj.Position, State.ItemEffectStyle)
                end
            end
        end
    end)
end

-- ═══════════════════════════════════════════
-- 🌈 SHADERS — реалистичные шейдеры
-- ═══════════════════════════════════════════

local shaderObjs = {}
local function clearShaders()
    for _, o in ipairs(shaderObjs) do if o and o.Parent then o:Destroy() end end
    shaderObjs = {}
end

local function applyShader(name)
    clearShaders()
    -- Сохраняем оригинальные настройки
    State._origLighting = State._origLighting or {
        Ambient = Lighting.Ambient,
        OutdoorAmbient = Lighting.OutdoorAmbient,
        Brightness = Lighting.Brightness,
        ClockTime = Lighting.ClockTime,
        FogEnd = Lighting.FogEnd,
        FogColor = Lighting.FogColor,
        ExposureCompensation = Lighting.ExposureCompensation,
        Saturation = Lighting.Saturation,
        Contrast = Lighting.Contrast,
        TintColor = Lighting.TintColor,
    }
    local orig = State._origLighting

    if name == "Нет" then
        Lighting.Ambient = orig.Ambient
        Lighting.OutdoorAmbient = orig.OutdoorAmbient
        Lighting.Brightness = orig.Brightness
        Lighting.ClockTime = orig.ClockTime
        Lighting.FogEnd = orig.FogEnd
        Lighting.FogColor = orig.FogColor
        Lighting.ExposureCompensation = orig.ExposureCompensation
        Lighting.Saturation = orig.Saturation
        Lighting.Contrast = orig.Contrast
        Lighting.TintColor = orig.TintColor
    elseif name == "Реалистичный" then
        Lighting.Ambient = Color3.fromRGB(70, 70, 70)
        Lighting.OutdoorAmbient = Color3.fromRGB(140, 140, 140)
        Lighting.Brightness = 2.5
        Lighting.ClockTime = 14
        Lighting.FogEnd = 5000
        Lighting.FogColor = Color3.fromRGB(200, 200, 220)
        Lighting.ExposureCompensation = 0.2
        Lighting.Saturation = 0.15
        Lighting.Contrast = 0.1
        Lighting.TintColor = Color3.fromRGB(255, 250, 240)
    elseif name == "Neon Night" then
        Lighting.Ambient = Color3.fromRGB(80, 60, 120)
        Lighting.OutdoorAmbient = Color3.fromRGB(60, 40, 100)
        Lighting.Brightness = 1
        Lighting.ClockTime = 0
        Lighting.FogColor = Color3.fromRGB(40, 20, 80)
        Lighting.FogEnd = 200
        Lighting.ExposureCompensation = 0.5
        Lighting.Saturation = 0.6
        Lighting.Contrast = 0.3
        Lighting.TintColor = Color3.fromRGB(180, 130, 255)
    elseif name == "Cinematic" then
        Lighting.Ambient = Color3.fromRGB(50, 50, 60)
        Lighting.OutdoorAmbient = Color3.fromRGB(80, 70, 90)
        Lighting.Brightness = 1.8
        Lighting.ClockTime = 17
        Lighting.FogColor = Color3.fromRGB(255, 180, 100)
        Lighting.FogEnd = 800
        Lighting.ExposureCompensation = 0.3
        Lighting.Saturation = -0.1
        Lighting.Contrast = 0.4
        Lighting.TintColor = Color3.fromRGB(255, 220, 180)
    elseif name == "Мрачный" then
        Lighting.Ambient = Color3.fromRGB(30, 30, 40)
        Lighting.OutdoorAmbient = Color3.fromRGB(50, 50, 60)
        Lighting.Brightness = 0.8
        Lighting.ClockTime = 3
        Lighting.FogColor = Color3.fromRGB(20, 20, 30)
        Lighting.FogEnd = 120
        Lighting.Saturation = -0.5
        Lighting.Contrast = 0.5
    elseif name == "Утро" then
        Lighting.Ambient = Color3.fromRGB(150, 140, 130)
        Lighting.OutdoorAmbient = Color3.fromRGB(200, 180, 160)
        Lighting.Brightness = 2.5
        Lighting.ClockTime = 6.5
        Lighting.FogColor = Color3.fromRGB(255, 220, 200)
        Lighting.FogEnd = 1500
        Lighting.Saturation = 0.2
        Lighting.TintColor = Color3.fromRGB(255, 240, 220)
    elseif name == "Закат" then
        Lighting.Ambient = Color3.fromRGB(120, 80, 60)
        Lighting.OutdoorAmbient = Color3.fromRGB(200, 120, 80)
        Lighting.Brightness = 2
        Lighting.ClockTime = 18
        Lighting.FogColor = Color3.fromRGB(255, 150, 100)
        Lighting.FogEnd = 800
        Lighting.Saturation = 0.4
        Lighting.TintColor = Color3.fromRGB(255, 200, 150)
    elseif name == "Киберпанк" then
        Lighting.Ambient = Color3.fromRGB(50, 20, 80)
        Lighting.OutdoorAmbient = Color3.fromRGB(80, 30, 120)
        Lighting.Brightness = 1.5
        Lighting.ClockTime = 0
        Lighting.FogColor = Color3.fromRGB(120, 0, 180)
        Lighting.FogEnd = 300
        Lighting.ExposureCompensation = 0.4
        Lighting.Saturation = 0.8
        Lighting.TintColor = Color3.fromRGB(200, 100, 255)
    elseif name == "Матрица" then
        Lighting.Ambient = Color3.fromRGB(20, 60, 20)
        Lighting.OutdoorAmbient = Color3.fromRGB(30, 100, 30)
        Lighting.Brightness = 1.2
        Lighting.ClockTime = 0
        Lighting.FogColor = Color3.fromRGB(0, 100, 0)
        Lighting.FogEnd = 250
        Lighting.Saturation = 0.3
        Lighting.TintColor = Color3.fromRGB(100, 255, 100)
    elseif name == "Чёрно-белый" then
        Lighting.Saturation = -1
        Lighting.Contrast = 0.4
        Lighting.Brightness = 2
    elseif name == "Красный апокалипсис" then
        Lighting.Ambient = Color3.fromRGB(80, 20, 20)
        Lighting.OutdoorAmbient = Color3.fromRGB(150, 30, 30)
        Lighting.Brightness = 1.5
        Lighting.ClockTime = 17
        Lighting.FogColor = Color3.fromRGB(200, 30, 30)
        Lighting.FogEnd = 300
        Lighting.Saturation = 0.5
        Lighting.TintColor = Color3.fromRGB(255, 100, 100)
    elseif name == "Голограмма" then
        Lighting.Ambient = Color3.fromRGB(0, 100, 150)
        Lighting.OutdoorAmbient = Color3.fromRGB(0, 150, 200)
        Lighting.Brightness = 2
        Lighting.ClockTime = 12
        Lighting.FogColor = Color3.fromRGB(0, 200, 255)
        Lighting.FogEnd = 400
        Lighting.Saturation = 0.7
        Lighting.TintColor = Color3.fromRGB(0, 220, 255)
    elseif name == "Dreamy" then
        Lighting.Ambient = Color3.fromRGB(200, 180, 255)
        Lighting.OutdoorAmbient = Color3.fromRGB(220, 200, 255)
        Lighting.Brightness = 3
        Lighting.ClockTime = 10
        Lighting.FogColor = Color3.fromRGB(255, 220, 255)
        Lighting.FogEnd = 600
        Lighting.Saturation = 0.4
        Lighting.TintColor = Color3.fromRGB(255, 200, 255)
    end
end

-- ═══════════════════════════════════════════
-- 👁️ ESP
-- ═══════════════════════════════════════════

local espObjs = {}
local function clearESP()
    for _, o in ipairs(espObjs) do if o and o.Parent then o:Destroy() end end
    espObjs = {}
end

local function buildESP()
    clearESP()
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= LP and plr.Character then
            local hl = Instance.new("Highlight", plr.Character)
            hl.FillColor = Color3.fromRGB(255, 50, 50)
            hl.OutlineColor = Color3.fromRGB(255, 255, 255)
            hl.FillTransparency = 0.6
            table.insert(espObjs, hl)
            local head = plr.Character:FindFirstChild("Head")
            if head then
                local bb = Instance.new("BillboardGui", head)
                bb.Size = UDim2.new(0, 120, 0, 32)
                bb.StudsOffset = Vector3.new(0, 3, 0)
                bb.AlwaysOnTop = true
                local txt = Instance.new("TextLabel", bb)
                txt.Size = UDim2.new(1, 0, 1, 0)
                txt.BackgroundTransparency = 1
                txt.Text = "🎯 " .. plr.Name
                txt.TextColor3 = Color3.fromRGB(255, 100, 100)
                txt.TextStrokeTransparency = 0
                txt.TextSize = 14
                txt.Font = Enum.Font.GothamBold
                table.insert(espObjs, bb)
            end
        end
    end
end

Players.PlayerAdded:Connect(function()
    if State.ESP then task.wait(1) buildESP() end
end)

-- ═══════════════════════════════════════════
-- 🔥 FIRE AURA, PARTICLE, NEON, SPIN
-- ═══════════════════════════════════════════

local fireAuraObj, particleAuraObj, neonObj, spinConn

local function toggleFire(on)
    if fireAuraObj then fireAuraObj:Destroy() fireAuraObj = nil end
    if on and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        fireAuraObj = Instance.new("Fire", LP.Character.HumanoidRootPart)
        fireAuraObj.Size = 10; fireAuraObj.Heat = 20
    end
end

local function toggleParticle(on)
    if particleAuraObj then particleAuraObj:Destroy() particleAuraObj = nil end
    if on and LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        particleAuraObj = Instance.new("ParticleEmitter", LP.Character.HumanoidRootPart)
        particleAuraObj.Texture = "rbxassetid://243660364"
        particleAuraObj.Rate = 50; particleAuraObj.Lifetime = NumberRange.new(1, 2)
        particleAuraObj.Speed = NumberRange.new(2, 5)
        particleAuraObj.SpreadAngle = Vector2.new(180, 180)
        particleAuraObj.Size = NumberSequence.new(1)
        particleAuraObj.Color = ColorSequence.new(Color3.fromRGB(120, 60, 255))
        particleAuraObj.LightEmission = 1
    end
end

local function toggleNeon(on)
    if neonObj then neonObj:Destroy() neonObj = nil end
    if on and LP.Character then
        neonObj = Instance.new("Highlight", LP.Character)
        neonObj.FillColor = Color3.fromRGB(120, 60, 255)
        neonObj.OutlineColor = Color3.fromRGB(200, 150, 255)
        neonObj.FillTransparency = 0.7
    end
end

local function toggleSpin(on)
    if spinConn then spinConn:Disconnect() spinConn = nil end
    if on then
        spinConn = RunService.Heartbeat:Connect(function()
            if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
                LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(5), 0)
            end
        end)
    end
end

-- ═══════════════════════════════════════════
-- НАПОЛНЕНИЕ ВКЛАДОК
-- ═══════════════════════════════════════════

-- ═══ ВКЛАДКА "ЭФФЕКТЫ" — FOOTSTEP + ITEM ═══
makeSection(EffectsPage, "👟 Эффекты под ногами (12 стилей)")

makeToggle(EffectsPage, "Включить Footstep", function(on)
    State.Footstep = on
    toggleFootstep(on)
end)

makeToggle(EffectsPage, "🌈 Rainbow Footstep", function(on)
    State.FootstepRainbow = on
end)

makeButton(EffectsPage, "🔄 Сменить стиль Footstep", function()
    local styles = {"Огонь","Дым","Искры","Лёд","Свет","Молния","Цветы","Классика","Плазма","Пузыри","Звёзды","Вихрь"}
    local idx = 1
    for i, s in ipairs(styles) do if s == State.FootstepStyle then idx = i % #styles + 1 break end end
    State.FootstepStyle = styles[idx]
    print("👟 Стиль: " .. State.FootstepStyle)
end)

makeSection(EffectsPage, "⚔️ Эффекты при взятии предметов")

makeToggle(EffectsPage, "Включить Item Effects", function(on)
    State.ItemEffect = on
    toggleItemEffect(on)
end)

makeButton(EffectsPage, "🔄 Сменить стиль Item Effect", function()
    local styles = {"Взрыв","Молния","Звёзды","Радуга","Портал"}
    local idx = 1
    for i, s in ipairs(styles) do if s == State.ItemEffectStyle then idx = i % #styles + 1 break end end
    State.ItemEffectStyle = styles[idx]
    print("⚔️ Item FX: " .. State.ItemEffectStyle)
end)

-- ═══ ВКЛАДКА "ШЕЙДЕРЫ" ═══
makeSection(ShaderPage, "🌈 Реалистичные шейдеры (14 шт)")

local shaders = {
    "Нет","Реалистичный","Neon Night","Cinematic","Мрачный","Утро","Закат",
    "Киберпанк","Матрица","Чёрно-белый","Красный апокалипсис",
    "Голограмма","Dreamy","Ночной режим"
}
for _, s in ipairs(shaders) do
    makeButton(ShaderPage, "🌈 " .. s, function()
        applyShader(s)
        State.Shader = s
        print("🎨 Шейдер: " .. s)
    end)
end

-- ═══ ВКЛАДКА "ИГРА" ═══
makeSection(GamePage, "👁️ ESP")

makeToggle(GamePage, "Включить ESP", function(on)
    State.ESP = on
    if on then buildESP() else clearESP() end
end)

makeButton(GamePage, "🔄 Обновить ESP", function()
    if State.ESP then buildESP() end
end)

makeSection(GamePage, "🎆 Визуальные ауры")

makeToggle(GamePage, "🔥 Fire Aura", function(on)
    State.FireAura = on; toggleFire(on)
end)

makeToggle(GamePage, "💫 Particle Aura", function(on)
    State.ParticleAura = on; toggleParticle(on)
end)

makeToggle(GamePage, "💜 Neon Glow", function(on)
    State.NeonGlow = on; toggleNeon(on)
end)

makeToggle(GamePage, "🌀 Spin (вращение)", function(on)
    State.Spin = on; toggleSpin(on)
end)

makeSection(GamePage, "⚡ Действия")

makeButton(GamePage, "💥 Fling Boost (рывок)", function()
    if LP.Character and LP.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = LP.Character.HumanoidRootPart
        hrp.Velocity = hrp.CFrame.LookVector * 300 + Vector3.new(0, 80, 0)
    end
end)

makeButton(GamePage, "🛡️ Godmode", function()
    if LP.Character then
        local h = LP.Character:FindFirstChildOfClass("Humanoid")
        if h then h.MaxHealth = math.huge h.Health = math.huge end
    end
end)

makeButton(GammakeSection(GamePage, "🎯 Быстрые стили")

makeButton(GamePage, "🌟 FULL SETUP", function()
    S.Trail = true
    S.TrailRainbow = true
    if _G.FTAP.buildTrail then
        _G.FTAP.buildTrail()
    end
    S.Cape = true
    S.CapeRainbow = true
    if _G.FTAP.buildCape then
        _G.FTAP.buildCape()
    end
    S.Footstep = true
    S.FootstepRainbow = true
    toggleFootstep(true)
    S.FireAura = true
    toggleFire(true)
    S.ParticleAura = true
    toggleParticle(true)
    S.NeonGlow = true
    toggleNeon(true)
end)
