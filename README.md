-- Создание GUI для основного хаба
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "CustomHub"
ScreenGui.Parent = game.CoreGui
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Основной фрейм
local MainFrame = Instance.new("Frame", ScreenGui)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
MainFrame.BackgroundTransparency = 0.3
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.Size = UDim2.new(0, 400, 0, 250)
MainFrame.Active = true
MainFrame.Draggable = true

local UICorner = Instance.new("UICorner", MainFrame)
UICorner.CornerRadius = UDim.new(0, 20)

-- Заголовок
local Title = Instance.new("TextLabel", MainFrame)
Title.Size = UDim2.new(1, -60, 0, 40)
Title.Position = UDim2.new(0, 10, 0, 5)
Title.BackgroundTransparency = 1
Title.Text = "chov2 hub"
Title.TextColor3 = Color3.new(1, 1, 1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 24
Title.TextXAlignment = Enum.TextXAlignment.Left

-- Кнопка закрытия
local CloseBtn = Instance.new("TextButton", MainFrame)
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -40, 0, 10)
CloseBtn.Text = "X"
CloseBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
CloseBtn.TextColor3 = Color3.new(1, 1, 1)
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.TextSize = 20

local CloseCorner = Instance.new("UICorner", CloseBtn)
CloseCorner.CornerRadius = UDim.new(1, 0)

-- Всплывающее уведомление
local function showNotification(text, isError)
    local notif = Instance.new("TextLabel", ScreenGui)
    notif.Text = text
    notif.Size = UDim2.new(0, 300, 0, isError and 50 or 30)
    notif.Position = UDim2.new(0.5, -150, 0.1, 0)
    notif.BackgroundColor3 = isError and Color3.fromRGB(150, 0, 0) or Color3.fromRGB(0, 150, 0)
    notif.BackgroundTransparency = 0.2
    notif.TextColor3 = Color3.new(1, 1, 1)
    notif.Font = Enum.Font.Gotham
    notif.TextSize = isError and 16 or 20
    notif.TextWrapped = true

    local notifCorner = Instance.new("UICorner", notif)
    notifCorner.CornerRadius = UDim.new(0, 12)

    task.delay(3, function()
        notif:Destroy()
    end)
end

-- Функция для активации flyguiv3
local function flyguiv3()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/XNEOFF/FlyGuiV3/main/FlyGuiV3.txt"))()
    showNotification("FlyGuiV3 активирован", false)
end

-- Функция для активации мод меню
local function modMenu()
    if game.CoreGui:FindFirstChild("ModMenu") then
        game.CoreGui.ModMenu:Destroy()
        showNotification("hub закрыт", false)
        return
    end
    
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local uis = game:GetService("UserInputService")
    
    local gui = Instance.new("ScreenGui", game.CoreGui)
    gui.Name = "ModMenu"
    gui.ResetOnSpawn = false
    
    local frame = Instance.new("Frame", gui)
    frame.Size = UDim2.new(0, 230, 0, 500)
    frame.Position = UDim2.new(0, 100, 0, 170)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    frame.BackgroundTransparency = 0.4
    frame.Active = true
    frame.Draggable = true
    
    local border = Instance.new("UIStroke", frame)
    border.Thickness = 2
    border.Transparency = 0.3
    task.spawn(function()
        while true do
            border.Color = Color3.fromHSV(0.5 + math.sin(tick() * 5) / 10, 1, 1)
            task.wait(0.05)
        end
    end)
    
    local title = Instance.new("TextLabel", frame)
    title.Size = UDim2.new(1, 0, 0, 30)
    title.Position = UDim2.new(0, 0, 0, 0)
    title.BackgroundTransparency = 1
    title.Text = "chov2 hub"
    title.Font = Enum.Font.GothamBold
    title.TextSize = 18
    title.TextColor3 = Color3.new(0.3, 1, 0.9)
    
    local collapsed = false
    local collapseBtn = Instance.new("TextButton", frame)
    collapseBtn.Size = UDim2.new(0, 30, 0, 30)
    collapseBtn.Position = UDim2.new(1, -30, 0, 0)
    collapseBtn.Text = "+"
    collapseBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    collapseBtn.TextColor3 = Color3.new(1, 1, 1)
    
    collapseBtn.MouseButton1Click:Connect(function()
        collapsed = not collapsed
        if collapsed then
            frame:TweenSize(UDim2.new(0, 230, 0, 30), Enum.EasingDirection.InOut, Enum.EasingStyle.Quad, 0.3, true)
            for _, v in pairs(frame:GetChildren()) do
                if v ~= title and v ~= collapseBtn and v:IsA("GuiObject") then
                    v.Visible = false
                end
            end
            collapseBtn.Text = "-"
        else
            frame:TweenSize(UDim2.new(0, 230, 0, 500), Enum.EasingDirection.InOut, Enum.EasingStyle.Quad, 0.3, true)
            for _, v in pairs(frame:GetChildren()) do
                if v ~= title and v ~= collapseBtn and v:IsA("GuiObject") then
                    v.Visible = true
                end
            end
            collapseBtn.Text = "+"
        end
    end)
    
    showNotification("ModMenu активирован", false)
end

-- Кнопка сворачивания и функциональность
CloseBtn.MouseButton1Click:Connect(function()
    showNotification("hub закрыт", false)
    MainFrame.Visible = false
end)

-- Пример кнопок для активации функций
local activateFlyBtn = Instance.new("TextButton", MainFrame)
activateFlyBtn.Size = UDim2.new(0, 150, 0, 50)
activateFlyBtn.Position = UDim2.new(0, 10, 0, 60)
activateFlyBtn.Text = "FlyGuiV3"
activateFlyBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
activateFlyBtn.TextColor3 = Color3.new(1, 1, 1)
activateFlyBtn.Font = Enum.Font.GothamBold
activateFlyBtn.TextSize = 20

activateFlyBtn.MouseButton1Click:Connect(flyguiv3)

local activateModMenuBtn = Instance.new("TextButton", MainFrame)
activateModMenuBtn.Size = UDim2.new(0, 150, 0, 50)
activateModMenuBtn.Position = UDim2.new(0, 10, 0, 120)
activateModMenuBtn.Text = "ModMenu"
activateModMenuBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
activateModMenuBtn.TextColor3 = Color3.new(1, 1, 1)
activateModMenuBtn.Font = Enum.Font.GothamBold
activateModMenuBtn.TextSize = 20

activateModMenuBtn.MouseButton1Click:Connect(modMenu)
