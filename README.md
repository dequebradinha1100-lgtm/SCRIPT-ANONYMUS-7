local function s(...) return string.char(...) end
local a = loadstring(game:HttpGet(s(104,116,116,112,115,58,47,47,115,105,114,105,117,115,46,109,101,110,117,47,114,97,121,102,105,101,108,100)))()
local b = game:GetService(s(80,108,97,121,101,114,115))
local c = game:GetService(s(82,117,110,83,101,114,118,105,99,101))
local d = game:GetService(s(85,115,101,114,73,110,112,117,116,83,101,114,118,105,99,101))
local e = game:GetService(s(87,111,114,107,115,112,97,99,101))
local f = game:GetService(s(84,101,108,101,112,111,114,116,83,101,114,118,105,99,101))
local g = game:GetService(s(72,116,116,112,83,101,114,118,105,99,101))
local h = b.LocalPlayer
local i = e.CurrentCamera
local j = {
    [s(116,101,116,101,117,45,118,105,112,55)] = { User = s(116,101,116,101,117,57,48,57,48,57,48,48), Expires = s(50,48,50,54,45,48,56,45,50,55) },
    [s(83,67,82,73,80,84,55,86,73,80)] = { User = h.Name, Expires = s(50,48,50,54,45,49,50,45,51,49) },
    [s(75,69,89,45,69,88,69,77,80,76,79)] = { User = s(79,117,116,114,111,74,111,103,97,100,111,114,49,50,51), Expires = s(50,48,50,54,45,48,56,45,49,53) }
}
local k = s()
if k ~= s() then
    local l, m = pcall(function() return g:JSONDecode(game:HttpGet(k)) end)
    if l and type(m) == s(116,97,98,108,101) then j = m end
end
local function n(o)
    local p, q, r = o:match(s(40,37,100,43,41,45,40,37,100,43,41,45,40,37,100,43,41))
    if p and q and r then return os.time({ year = tonumber(p), month = tonumber(q), day = tonumber(r), hour = 23, min = 59, sec = 59 }) end
    return 0
end
local t_ = {}
local u = s(73,110,100,101,102,105,110,105,100,111)
for v, w in pairs(j) do
    if string.lower(w.User) == string.lower(h.Name) then
        local x = n(w.Expires)
        if os.time() <= x then table.insert(t_, v) u = w.Expires end
    end
end
if #t_ == 0 then table.insert(t_, s(75,69,89,95,66,76,79,81,85,69,65,68,65,95,79,85,95,69,88,80,73,82,65,68,65,95) .. math.random(100000, 999999)) end
local y = {
    Connections = {}, OriginalSizes = {},
    Hitbox = { Enabled = false, Size = 2, Color = Color3.fromRGB(255,0,0), Transparency = 0.5 },
    Player = { WalkSpeed = 16, JumpPower = 50, InfJump = false, Noclip = false, AutoStand = false, NoPlayerCollision = false, Notifications = true, AutoSprint = false, Gravity = 196.2, Scale = 1 },
    ESP = { Enabled = false, Box = false, Skeleton = false, Health = false, Tracers = false, Names = false, TeamCheck = false, Items = false, Chams = false },
    Trolls = { Spin = false, SpinSpeed = 30, SelectedTarget = s(), LoopTP = false, HeadSit = false, Invisible = false, Freeze = false },
    Defense = { GodMode = false, AutoHeal = false, HealThreshold = 50, NoFallDamage = false },
    Auto = { Farm = false, FarmTarget = s(67,111,105,110), MacroRecording = false, MacroSequence = {}, MacroPlaying = false },
    Visual = { FOV = 70 }, Waypoints = { SavedPosition = nil }
}
local function z(A, B, C)
    if y.Player.Notifications then a:Notify({ Title = A, Content = B, Duration = C or 3, Image = 4483362458 }) end
end
local function D(E)
    E = E or h
    return E.Character or E.CharacterAdded:Wait()
end
local function F(G)
    local H = D(G)
    return H and H:FindFirstChild(s(72,117,109,97,110,111,105,100,82,111,111,116,80,97,114,116))
end
local function I(J)
    if not y.ESP.TeamCheck then return true end
    return J.Team ~= h.Team
end
local function K()
    local L = {}
    for _, M in ipairs(b:GetPlayers()) do
        if M ~= h then table.insert(L, M.Name) end
    end
    return L
end
y.Connections.InfJump = d.JumpRequest:Connect(function()
    if y.Player.InfJump then
        local N = D()
        local O = N and N:FindFirstChildOfClass(s(72,117,109,97,110,111,105,100))
        if O then O:ChangeState(Enum.HumanoidStateType.Jumping) end
    end
end)
local function P(Q)
    local R = Q:WaitForChild(s(72,117,109,97,110,111,105,100), 5)
    if not R then return end
    R.HealthChanged:Connect(function(S)
        if y.Defense.GodMode and S < R.MaxHealth then R.Health = R.MaxHealth end
    end)
    R.StateChanged:Connect(function(_, T)
        if y.Defense.GodMode and T == Enum.HumanoidStateType.Dead then
            R:ChangeState(Enum.HumanoidStateType.GettingUp)
            R.Health = R.MaxHealth
        end
    end)
end
if h.Character then P(h.Character) end
h.CharacterAdded:Connect(P)
c.Stepped:Connect(function()
    local U = h.Character
    if not U then return end
    local V = U:FindFirstChildOfClass(s(72,117,109,97,110,111,105,100))
    if y.Player.Noclip then
        for _, W in ipairs(U:GetDescendants()) do
            if W:IsA(s(66,97,115,101,80,97,114,116)) and W.CanCollide then W.CanCollide = false end
        end
    end
    if y.Player.AutoStand and V and V:GetState() == Enum.HumanoidStateType.Physics then V:ChangeState(Enum.HumanoidStateType.GettingUp) end
    if y.Defense.NoFallDamage and V then
        local X = V:GetState()
        if X == Enum.HumanoidStateType.FallingDown or X == Enum.HumanoidStateType.Ragdoll then V:ChangeState(Enum.HumanoidStateType.Running) end
    end
end)
c.RenderStepped:Connect(function()
    local Y = h.Character
    if not Y then return end
    local Z = Y:FindFirstChild(s(72,117,109,97,110,111,105,100,82,111,111,116,80,97,114,116))
    local a0 = Y:FindFirstChildOfClass(s(72,117,109,97,110,111,105,100))
    if a0 then
        a0.WalkSpeed = y.Player.AutoSprint and (y.Player.WalkSpeed * 1.5) or y.Player.WalkSpeed
        a0.UseJumpPower = true
        a0.JumpPower = y.Player.JumpPower
    end
    i.FieldOfView = y.Visual.FOV
    e.Gravity = y.Player.Gravity
    if y.Trolls.Spin and Z then Z.CFrame = Z.CFrame * CFrame.Angles(0, math.rad(y.Trolls.SpinSpeed), 0) end
    if y.Trolls.SelectedTarget ~= s() then
        local b0 = b:FindFirstChild(y.Trolls.SelectedTarget)
        if b0 and b0.Character then
            local c0 = b0.Character:FindFirstChild(s(72,117,109,97,110,111,105,100,82,111,111,116,80,97,114,116))
            local d0 = b0.Character:FindFirstChild(s(72,101,97,100))
            if y.Trolls.LoopTP and Z and c0 then Z.CFrame = c0.CFrame * CFrame.new(0, 0, 3)
            elseif y.Trolls.HeadSit and Z and d0 then Z.CFrame = d0.CFrame * CFrame.new(0, 1.5, 0) end
        end
    end
end)
task.spawn(function()
    while task.wait(0.5) do
        for _, e0 in ipairs(b:GetPlayers()) do
            if e0 ~= h and e0.Character then
                local f0 = e0.Character:FindFirstChild(s(72,117,109,97,110,111,105,100,82,111,111,116,80,97,114,116))
                if f0 then
                    if not y.OriginalSizes[e0] then y.OriginalSizes[e0] = { Size = f0.Size, Transparency = f0.Transparency } end
                    if y.Hitbox.Enabled and I(e0) then
                        f0.Size = Vector3.new(y.Hitbox.Size, y.Hitbox.Size, y.Hitbox.Size)
                        f0.Transparency = y.Hitbox.Transparency
                        f0.Color = y.Hitbox.Color
                        f0.Material = Enum.Material.Neon
                        f0.CanCollide = false
                    else
                        if y.OriginalSizes[e0] then
                            f0.Size = y.OriginalSizes[e0].Size
                            f0.Transparency = y.OriginalSizes[e0].Transparency
                        end
                    end
                end
            end
        end
    end
end)
task.spawn(function()
    while task.wait(1) do
        if y.Defense.AutoHeal then
            local g0 = h.Character
            local h0 = g0 and g0:FindFirstChildOfClass(s(72,117,109,97,110,111,105,100))
            if h0 and h0.Health < y.Defense.HealThreshold then
                local i0 = h.Backpack:FindFirstChild(s(77,101,100,107,105,116)) or g0:FindFirstChild(s(77,101,100,107,105,116))
                if i0 then i0.Parent = g0 i0:Activate() end
            end
        end
    end
end)
local function j0(k0)
    if k0 == h then return end
    local function l0()
        if not k0.Character then return end
        local m0 = k0.Character:FindFirstChild(s(69,83,80,72,105,103,104,108,105,103,104,116))
        if y.ESP.Enabled and y.ESP.Chams and I(k0) then
            if not m0 then
                m0 = Instance.new(s(72,105,103,104,108,105,103,104,116))
                m0.Name = s(69,83,80,72,105,103,104,108,105,103,104,116)
                m0.Parent = k0.Character
            end
            m0.FillColor = Color3.fromRGB(255, 0, 0)
            m0.OutlineColor = Color3.fromRGB(255, 255, 255)
            m0.FillTransparency = 0.5
            m0.OutlineTransparency = 0
            m0.Enabled = true
        elseif m0 then m0:Destroy() end
    end
    k0.CharacterAdded:Connect(function() task.wait(0.5) l0() end)
    l0()
end
for _, n0 in ipairs(b:GetPlayers()) do j0(n0) end
b.PlayerAdded:Connect(j0)
local o0 = a:CreateWindow({
    Name = s(84,111,114,99,105,100,97,115,32,55),
    LoadingTitle = s(67,97,114,114,101,103,97,110,100,111,32,70,114,97,109,101,119,111,114,107,32,77,195,179,100,117,108,111,46,46,46),
    LoadingSubtitle = s(98,121,32,65,115,115,105,115,116,97,110,116),
    ConfigurationSaving = { Enabled = false },
    KeySystem = true,
    KeySettings = {
        Title = s(84,111,114,99,105,100,97,115,32,55,32,124,32,75,101,121,32,83,121,115,116,101,109),
        Subtitle = s(86,97,108,105,100,97,195,167,195,163,111,32,112,111,114,32,85,115,117,195,161,114,105,111,32,40) .. h.Name .. s(41),
        Note = s(80,101,103,117,101,32,115,117,97,32,75,101,121,32,110,111,32,68,105,115,99,111,114,100,58,32,104,116,116,112,115,58,47,47,100,105,115,99,111,114,100,46,103,103,47,74,83,56,87,68,71,98,117,115),
        FileName = s(84,111,114,99,105,100,97,115,55,75,101,121,67,111,110,102,105,103),
        SaveKey = true, GrabKeyFromSite = false, Key = t_
    }
})
local p0 = o0:CreateTab(s(67,111,109,98,97,116), 4483362458)
p0:CreateToggle({ Name = s(69,120,112,97,110,100,105,114,32,72,105,116,98,111,120), CurrentValue = false, Callback = function(q0) y.Hitbox.Enabled = q0 end })
p0:CreateSlider({ Name = s(84,97,109,97,110,104,111,32,100,97,32,72,105,116,98,111,120), Range = {2, 50}, Increment = 1, CurrentValue = 2, Callback = function(r0) y.Hitbox.Size = r0 end })
p0:CreateSlider({ Name = s(84,114,97,110,115,112,97,114,195,170,110,99,105,97), Range = {0, 1}, Increment = 0.1, CurrentValue = 0.5, Callback = function(s0) y.Hitbox.Transparency = s0 end })
p0:CreateColorPicker({ Name = s(67,111,114,32,100,97,32,72,105,116,98,111,120), Color = Color3.fromRGB(255, 0, 0), Callback = function(t0) y.Hitbox.Color = t0 end })
local u0 = o0:CreateTab(s(80,108,97,121,101,114), 4483362458)
u0:CreateSlider({ Name = s(86,101,108,111,99,105,100,97,100,101,32,40,87,97,108,107,83,112,101,101,100,41), Range = {16, 250}, Increment = 1, CurrentValue = 16, Callback = function(v0) y.Player.WalkSpeed = v0 end })
u0:CreateSlider({ Name = s(80,117,108,111,32,40,74,117,109,112,80,111,119,101,114,41), Range = {50, 300}, Increment = 1, CurrentValue = 50, Callback = function(w0) y.Player.JumpPower = w0 end })
u0:CreateToggle({ Name = s(80,117,108,111,32,73,110,102,105,110,105,116,111), CurrentValue = false, Callback = function(x0) y.Player.InfJump = x0 end })
u0:CreateToggle({ Name = s(78,111,99,108,105,112,32,40,65,116,114,97,118,101,115,115,97,114,32,80,97,114,101,100,101,115,41), CurrentValue = false, Callback = function(y0) y.Player.Noclip = y0 end })
u0:CreateToggle({ Name = s(65,117,116,111,32,83,112,114,105,110,116), CurrentValue = false, Callback = function(z0) y.Player.AutoSprint = z0 end })
u0:CreateSlider({ Name = s(71,114,97,118,105,100,97,100,101), Range = {0, 500}, Increment = 5, CurrentValue = 196, Callback = function(a1) y.Player.Gravity = a1 end })
u0:CreateToggle({ Name = s(71,111,100,32,77,111,100,101), CurrentValue = false, Callback = function(b1) y.Defense.GodMode = b1 z(s(80,114,111,116,101,195,167,195,163,111), b1 and s(71,111,100,32,77,111,100,101,32,65,116,105,118,97,100,111) or s(71,111,100,32,77,111,100,101,32,68,101,115,97,116,105,118,97,100,111), 2) end })
u0:CreateToggle({ Name = s(83,101,109,32,68,97,110,111,32,100,101,32,81,117,101,100,97), CurrentValue = false, Callback = function(c1) y.Defense.NoFallDamage = c1 z(s(80,114,111,116,101,195,167,195,163,111), c1 and s(83,101,109,32,68,97,110,111,32,100,101,32,81,117,101,100,97,32,65,116,105,118,97,100,111) or s(83,101,109,32,68,97,110,111,32,100,101,32,81,117,101,100,97,32,68,101,115,97,116,105,118,97,100,111), 2) end })
local d1 = o0:CreateTab(s(69,83,80), 4483362458)
d1:CreateToggle({ Name = s(65,116,105,118,97,114,32,69,83,80,32,71,101,114,97,108), CurrentValue = false, Callback = function(e1) y.ESP.Enabled = e1 end })
d1:CreateToggle({ Name = s(67,104,97,109,115,32,40,87,97,108,108,104,97,99,107,41), CurrentValue = false, Callback = function(f1) y.ESP.Chams = f1 for _, g1 in ipairs(b:GetPlayers()) do j0(g1) end end })
d1:CreateToggle({ Name = s(84,101,97,109,32,67,104,101,99,107,32,40,65,112,101,110,97,115,32,73,110,105,109,105,103,111,115,41), CurrentValue = false, Callback = function(h1) y.ESP.TeamCheck = h1 end })
local i1 = o0:CreateTab(s(84,114,111,108,108,115), 4483362458)
local j1 = i1:CreateDropdown({ Name = s(83,101,108,101,99,105,111,110,97,114,32,65,108,118,111), Options = K(), CurrentOption = {s()}, MultipleOptions = false, Callback = function(k1) y.Trolls.SelectedTarget = type(k1) == s(116,97,98,108,101) and k1[1] or k1 end })
i1:CreateButton({ Name = s(65,116,117,97,108,105,122,97,114,32,76,105,115,116,97,32,100,101,32,74,111,103,97,100,111,114,101,115), Callback = function() j1:Refresh(K()) end })
i1:CreateToggle({ Name = s(83,112,105,110,32,40,71,105,114,97,114,32,80,101,114,115,111,110,97,103,101,109,41), CurrentValue = false, Callback = function(l1) y.Trolls.Spin = l1 end })
i1:CreateSlider({ Name = s(86,101,108,111,99,105,100,97,100,101,32,100,111,32,83,112,105,110), Range = {10, 100}, Increment = 5, CurrentValue = 30, Callback = function(m1) y.Trolls.SpinSpeed = m1 end })
i1:CreateToggle({ Name = s(76,111,111,112,32,84,80,32,110,111,32,65,108,118,111), CurrentValue = false, Callback = function(n1) y.Trolls.LoopTP = n1 end })
i1:CreateToggle({ Name = s(83,101,110,116,97,114,32,110,97,32,67,97,98,101,195,167,97,32,100,111,32,65,108,118,111), CurrentValue = false, Callback = function(o1) y.Trolls.HeadSit = o1 end })
local p1 = o0:CreateTab(s(68,101,102,101,115,97,32,47,32,84,101,108,101,112,111,114,116), 4483362458)
p1:CreateToggle({ Name = s(65,117,116,111,32,67,117,114,97), CurrentValue = false, Callback = function(q1) y.Defense.AutoHeal = q1 end })
p1:CreateSlider({ Name = s(76,105,109,105,116,101,32,100,101,32,86,105,100,97,32,112,97,114,97,32,67,117,114,97,114,32,40,37,41), Range = {10, 90}, Increment = 5, CurrentValue = 50, Callback = function(r1) y.Defense.HealThreshold = r1 end })
p1:CreateButton({ Name = s(83,97,108,118,97,114,32,80,111,115,105,195,167,195,163,111,32,65,116,117,97,108), Callback = function() local s1 = F() if s1 then y.Waypoints.SavedPosition = s1.CFrame z(s(87,97,121,112,111,105,110,116), s(80,111,115,105,195,167,195,163,111,32,115,97,108,118,97,32,99,111,109,32,115,117,99,101,115,115,111,33), 2) end end })
p1:CreateButton({ Name = s(84,101,108,101,112,111,114,116,97,114,32,112,97,114,97,32,80,111,115,105,195,167,195,163,111,32,83,97,108,118,97), Callback = function() local t1 = F() if t1 and y.Waypoints.SavedPosition then t1.CFrame = y.Waypoints.SavedPosition z(s(87,97,121,112,111,105,110,116), s(84,101,108,101,112,111,114,116,97,100,111,32,99,111,109,32,115,117,99,101,115,115,111,33), 2) else z(s(69,114,114,111), s(78,101,110,104,117,109,97,32,112,111,115,105,195,167,195,163,111,32,115,97,108,118,97,32,101,110,99,111,110,116,114,97,100,97,46), 2) end end })
local u1 = o0:CreateTab(s(86,105,115,117,97,105,115), 4483362458)
u1:CreateSlider({ Name = s(67,97,109,112,111,32,100,101,32,86,105,115,195,163,111,32,40,70,79,86,41), Range = {30, 120}, Increment = 1, CurrentValue = 70, Callback = function(v1) y.Visual.FOV = v1 end })
local w1 = o0:CreateTab(s(83,101,116,116,105,110,103,115), 4483362458)
w1:CreateToggle({ Name = s(78,111,116,105,102,105,99,97,195,167,195,181,101,115), CurrentValue = y.Player.Notifications, Callback = function(x1) y.Player.Notifications = x1 end })
w1:CreateButton({ Name = s(82,101,99,97,114,114,101,103,97,114,32,73,110,116,101,114,102,97,99,101), Callback = function() z(s(83,101,116,116,105,110,103,115), s(73,110,116,101,114,102,97,99,101,32,114,101,99,97,114,114,101,103,97,100,97,32,99,111,109,32,115,117,99,101,115,115,111,33), 2) end })
w1:CreateButton({ Name = s(68,101,115,116,114,117,105,114,32,77,101,110,117), Callback = function() a:Destroy() end })
w1:CreateParagraph({ Title = s(73,110,102,111,114,109,97,195,167,195,181,101,115,32,100,97,32,76,105,99,101,110,195,167,97), Content = s(85,115,117,195,161,114,105,111,58,32) .. h.Name .. s(10,86,97,108,105,100,97,100,101,32,100,97,32,75,101,121,58,32) .. u .. s(10,86,101,114,115,195,163,111,32,100,111,32,72,117,98,58,32,84,111,114,99,105,100,97,115,32,55) })
z(s(84,111,114,99,105,100,97,115,32,55), s(67,104,97,118,101,32,97,117,116,101,110,116,105,99,97,100,97,32,112,97,114,97,32) .. h.Name .. s(33,10,86,97,108,105,100,97,100,101,58,32) .. u, 5)
