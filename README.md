local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local root = char:WaitForChild("HumanoidRootPart")

local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "NearbyItemScanner"

local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0, 10, 0.5, -200)
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.BorderSizePixel = 0

local title = Instance.new("TextLabel", frame)
title.Text = "Itens Próximos (Interativos)"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20

local scroll = Instance.new("ScrollingFrame", frame)
scroll.Position = UDim2.new(0, 0, 0, 30)
scroll.Size = UDim2.new(1, 0, 1, -30)
scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
scroll.ScrollBarThickness = 6
scroll.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

local UIList = Instance.new("UIListLayout", scroll)
UIList.SortOrder = Enum.SortOrder.LayoutOrder

-- Lista de palavras-chave que indicam itens "pegáveis"
local keywords = {"bush", "brush", "berry", "apple", "fruit", "plant", "food", "collect", "leaf", "crop"}

-- Distância máxima para exibir
local maxDist = 100

-- Atualiza a lista
local function updateNearbyItems()
    scroll:ClearAllChildren()
    UIList.Parent = scroll

    local count = 0
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            local nameLower = string.lower(obj.Name)
            for _, key in pairs(keywords) do
                if nameLower:find(key) then
                    local dist = (obj.Position - root.Position).Magnitude
                    if dist <= maxDist then
                        local label = Instance.new("TextLabel", scroll)
                        label.Size = UDim2.new(1, -10, 0, 20)
                        label.BackgroundTransparency = 1
                        label.TextColor3 = Color3.new(1,1,1)
                        label.Font = Enum.Font.SourceSans
                        label.TextSize = 14
                        label.TextXAlignment = Enum.TextXAlignment.Left
                        label.Text = obj.Name .. " [".. math.floor(dist) .." studs]"
                        count += 1
                        break
                    end
                end
            end
        end
    end

    scroll.CanvasSize = UDim2.new(0, 0, 0, count * 20)
end

-- Loop de atualização
while true do
    updateNearbyItems()
    wait(3)
end
