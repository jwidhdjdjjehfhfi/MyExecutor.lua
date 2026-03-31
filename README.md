

local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local lp = Players.LocalPlayer

-- Cleanup old versions
if CoreGui:FindFirstChild("AdminPro") then CoreGui.AdminPro:Destroy() end

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "AdminPro"
ScreenGui.Parent = (gethui and gethui()) or CoreGui

-- [1. THE REAL EXECUTOR WINDOW (Hidden at start)]
local RealExecFrame = Instance.new("Frame", ScreenGui)
RealExecFrame.Name = "RealExecutor"
RealExecFrame.Size = UDim2.new(0, 300, 0, 300)
RealExecFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
RealExecFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
RealExecFrame.Visible = false -- Only shows when command is run
RealExecFrame.Active = true; RealExecFrame.Draggable = true
Instance.new("UICorner", RealExecFrame).CornerRadius = UDim.new(0, 15)

-- Rainbow Outline for Real Executor
local RealStroke = Instance.new("UIStroke", RealExecFrame)
RealStroke.Thickness = 4; RealStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
local RealGrad = Instance.new("UIGradient", RealStroke)
RealGrad.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.new(1,0,0)), ColorSequenceKeypoint.new(1, Color3.new(0,0,1))}
task.spawn(function() while task.wait() do RealGrad.Rotation = RealGrad.Rotation + 5 end end)

local RealTitle = Instance.new("TextLabel", RealExecFrame)
RealTitle.Size = UDim2.new(1, 0, 0, 40); RealTitle.Text = "REAL EXECUTOR"; RealTitle.TextColor3 = Color3.new(1,1,1); RealTitle.BackgroundTransparency = 1

local ScriptBox = Instance.new("TextBox", RealExecFrame)
ScriptBox.Size = UDim2.new(0.9, 0, 0.5, 0); ScriptBox.Position = UDim2.new(0.05, 0, 0.15, 0)
ScriptBox.ClearTextOnFocus = false; ScriptBox.TextXAlignment = Enum.TextXAlignment.Left; ScriptBox.TextYAlignment = Enum.TextYAlignment.Top
ScriptBox.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1); ScriptBox.TextColor3 = Color3.new(1,1,1); ScriptBox.MultiLine = true
Instance.new("UICorner", ScriptBox)

local RunBtn = Instance.new("TextButton", RealExecFrame)
RunBtn.Size = UDim2.new(0.4, 0, 0, 40); RunBtn.Position = UDim2.new(0.05, 0, 0.7, 0)
RunBtn.Text = "Execute"; RunBtn.BackgroundColor3 = Color3.fromRGB(0, 150, 0)

local ClearBtn = Instance.new("TextButton", RealExecFrame)
ClearBtn.Size = UDim2.new(0.4, 0, 0, 40); ClearBtn.Position = UDim2.new(0.55, 0, 0.7, 0)
ClearBtn.Text = "clear"; ClearBtn.BackgroundColor3 = Color3.fromRGB(150, 0, 0)

local RealX = Instance.new("TextButton", RealExecFrame)
RealX.Size = UDim2.new(0, 25, 0, 25); RealX.Position = UDim2.new(1, -30, 0, 5)
RealX.Text = "X"; RealX.BackgroundColor3 = Color3.new(0.6, 0, 0)
RealX.MouseButton1Click:Connect(function() RealExecFrame.Visible = false end)

-- [2. LOGIC FOR REAL EXECUTOR]
RunBtn.MouseButton1Click:Connect(function()
    local code = ScriptBox.Text
    local success, err = pcall(function()
        loadstring(code)() -- Executes the text in the box
    end)
    if not success then warn("Script Error: " .. err) end
end)

ClearBtn.MouseButton1Click:Connect(function() ScriptBox.Text = "" end)

-- [3. ADDING COMMAND TO YOUR EXISTING SYSTEM]
-- In your command runner function, add this:
local function runCommand(cmd)
    cmd = cmd:lower()
    if cmd == "real executor" then
        RealExecFrame.Visible = true
    elseif cmd == "fly" then 
        -- [Your existing fly logic]
    end
end
