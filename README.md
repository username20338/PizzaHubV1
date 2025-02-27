local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoids = game:GetService("Workspace"):WaitForChild("Enemies"):GetChildren()
local PathfindingService = game:GetService("PathfindingService")

-- Função para encontrar um inimigo com bounty (exemplo básico)
function findBounty()
    for _, enemy in pairs(humanoids) do
        if enemy:FindFirstChild("Humanoid") then
            local humanoid = enemy:FindFirstChild("Humanoid")
            if humanoid.Health > 0 then
                -- Exemplo de como atacar o inimigo
                attackEnemy(enemy)
                break
            end
        end
    end
end

-- Função para mover o personagem até o inimigo
function moveToEnemy(enemy)
    local targetPosition = enemy.HumanoidRootPart.Position
    local path = PathfindingService:CreatePath({
        AgentRadius = 2,
        AgentHeight = 5,
        AgentCanJump = true,
        AgentJumpHeight = 10,
        AgentMaxSlope = 45
    })
    
    path:ComputeAsync(character.HumanoidRootPart.Position, targetPosition)
    path:MoveTo(character)

    -- Espera até que o caminho seja concluído
    path.Completed:Wait()
end

-- Função para atacar o inimigo
function attackEnemy(enemy)
    -- Aqui você pode colocar a lógica para atacar o inimigo com suas habilidades
    -- Por exemplo, você pode usar uma habilidade vinculada à tecla 'E'
    print("Atacando " .. enemy.Name)

    -- Exemplificando o uso de uma habilidade (mude conforme necessário)
    local abilityKey = Enum.KeyCode.E -- Altere conforme sua configuração de habilidade
    game:GetService("UserInputService"):InputBegan({
        UserInputType = Enum.UserInputType.Keyboard,
        KeyCode = abilityKey
    }, true)
end

-- Função para monitorar continuamente a presença de inimigos e ataques
while true do
    findBounty()
    wait(5) -- Espera 5 segundos antes de procurar outro inimigo
end
