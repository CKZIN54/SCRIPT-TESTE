-- Script de Invisibilidade para Roblox (para colocar dentro de um menu)
-- Esse script alterna entre invisível/visível ao clicar em um botão

--[[
Como usar:
1. Adicione esse script como 'LocalScript' dentro do seu menu.
2. Crie um botão (ex: ButtonInvisibilidade) dentro do menu (ScreenGui).
3. Altere o nome do botão na variável 'invisButton' se necessário.
]]

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local invisButton = script.Parent:WaitForChild("ButtonInvisibilidade") -- ajuste o nome do botão aqui

local isInvisible = false

-- Função para alterar a visibilidade
local function setInvisibility(state)
    character = player.Character or player.CharacterAdded:Wait()
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
            part.Transparency = state and 1 or 0
            if part:FindFirstChildOfClass("Decal") then
                for _, decal in pairs(part:GetChildren()) do
                    if decal:IsA("Decal") then
                        decal.Transparency = state and 1 or 0
                    end
                end
            end
        elseif part:IsA("Accessory") and part:FindFirstChild("Handle") then
            part.Handle.Transparency = state and 1 or 0
        end
    end
    -- Opcional: esconder nome acima da cabeça
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.NameDisplayDistance = state and 0 or 100
    end
end

-- Evento do botão
invisButton.MouseButton1Click:Connect(function()
    isInvisible = not isInvisible
    setInvisibility(isInvisible)
    -- Opcional: alterar texto do botão
    invisButton.Text = isInvisible and "Ficar Visível" or "Ficar Invisível"
end)
