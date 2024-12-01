-- Exemplo avançado de script Lua para Blox Fruits
local player = game.Players.LocalPlayer
local humanoid = player.Character:FindFirstChildOfClass("Humanoid")

-- Função para teleportar o jogador
function teleportTo(position)
    player.Character.HumanoidRootPart.CFrame = CFrame.new(position)
end

-- Função para atacar inimigos
function attackEnemy(enemy)
    while enemy.Humanoid.Health > 0 do
        player.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame
        game:GetService("VirtualUser"):CaptureController()
        game:GetService("VirtualUser"):Button1Down(Vector2.new(0,0), workspace.CurrentCamera.CFrame)
        wait(0.1)
    end
end

-- Função para coletar itens
function collectItems()
    for _, item in pairs(game.Workspace.Items:GetChildren()) do
        if item:IsA("Tool") then
            teleportTo(item.Position)
            wait(0.5)
            fireclickdetector(item.ClickDetector)
        end
    end
end

-- Auto farm loop
while true do
    if humanoid.Health > 0 then
        -- Encontrar inimigo mais próximo
        local closestEnemy = nil
        local shortestDistance = math.huge
        for _, enemy in pairs(game.Workspace.Enemies:GetChildren()) do
            local distance = (player.Character.HumanoidRootPart.Position - enemy.HumanoidRootPart.Position).magnitude
            if distance < shortestDistance then
                closestEnemy = enemy
                shortestDistance = distance
            end
        end

        -- Atacar inimigo mais próximo
        if closestEnemy then
            attackEnemy(closestEnemy)
        end

        -- Coletar itens
        collectItems()
    end
    wait(1)
end
