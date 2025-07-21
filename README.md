local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local uis = game:GetService("UserInputService")

-- Noclip
local noclipEnabled = false

-- Speed Boost
local boosted = false
local defaultSpeed = 16 -- Ajuste se o padrão do seu jogo for diferente
local boostMultiplier = 3

-- Função para ativar/desativar noclip
local function setNoclip(state)
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = not state
        end
    end
end

-- Função para atualizar velocidade
local function setSpeed(state)
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        if state then
            humanoid.WalkSpeed = defaultSpeed * boostMultiplier
        else
            humanoid.WalkSpeed = defaultSpeed
        end
    end
end

-- Loop do noclip
game:GetService("RunService").Stepped:Connect(function()
    if noclipEnabled then
        setNoclip(true)
    end
end)

-- Atualiza character ao renascer
player.CharacterAdded:Connect(function(char)
    character = char
end)

-- Teclas
uis.InputBegan:Connect(function(input, processed)
    if processed then return end
    -- Noclip (F)
    if input.KeyCode == Enum.KeyCode.F then
        noclipEnabled = not noclipEnabled
        setNoclip(noclipEnabled)
    end
    -- Speed Boost (V)
    if input.KeyCode == Enum.KeyCode.V then
        boosted = not boosted
        setSpeed(boosted)
    end
end)
