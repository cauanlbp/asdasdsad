local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humRoot = char:WaitForChild("HumanoidRootPart")

-- Altere isso se o nome dos arbustos for diferente
local bushNames = {"Bush", "BerryBush", "FoodBush"} -- tente deixar genérico

-- Delay entre as ações
local delayTime = 3

-- Função principal
while wait(delayTime) do
    for _, bush in pairs(workspace:GetDescendants()) do
        if table.find(bushNames, bush.Name) and bush:IsA("Part") then
            local dist = (bush.Position - humRoot.Position).Magnitude
            if dist < 100 then
                -- Simula clique ou ativa o arbusto
                fireclickdetector(bush:FindFirstChildOfClass("ClickDetector"))
                wait(1)
            end
        end
    end
end

for _, obj in pairs(workspace:GetDescendants()) do
    if string.lower(obj.Name):find("bush") or string.lower(obj.Name):find("brush") then
        print("Nome:", obj.Name, "| Classe:", obj.ClassName, "| Caminho:", obj:GetFullName())
    end
end
