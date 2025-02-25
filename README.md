local Players = game:GetService("Players")

local function onPlayerAdded(player)
    player.CharacterAdded:Connect(function(character)
        local head = character:FindFirstChild("Head")
        if head then
            -- Identificar se o jogador usa R6 ou R15
            local humanoid = character:FindFirstChildOfClass("Humanoid")
            local rigType = "Desconhecido"
            
            if humanoid then
                if humanoid.RigType == Enum.HumanoidRigType.R6 then
                    rigType = "R6"
                elseif humanoid.RigType == Enum.HumanoidRigType.R15 then
                    rigType = "R15"
                end
            end

            -- Criar BillboardGui
            local billboard = Instance.new("BillboardGui")
            billboard.Size = UDim2.new(4, 0, 1, 0)
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            billboard.AlwaysOnTop = true
            billboard.Parent = head

            -- Criar TextoLabel
            local textLabel = Instance.new("TextLabel")
            textLabel.Size = UDim2.new(1, 0, 1, 0)
            textLabel.BackgroundTransparency = 1
            textLabel.TextScaled = true
            textLabel.TextColor3 = Color3.fromRGB(0, 255, 0) -- Verde
            textLabel.Font = Enum.Font.SourceSansBold
            textLabel.Text = player.Name .. " (" .. rigType .. ")" -- Nome + Tipo de corpo
            textLabel.Parent = billboard
        end
    end)
end

-- Conectar evento para novos jogadores
Players.PlayerAdded:Connect(onPlayerAdded)

-- Aplicar aos jogadores já no jogo
for _, player in pairs(Players:GetPlayers()) do
    onPlayerAdded(player)
end
