-- Criar um GUI para o jogador
local player = game.Players.LocalPlayer
local gui = Instance.new("ScreenGui", player.PlayerGui)

-- Criar um cubo para abrir as opções
local buttonPart = Instance.new("Part")
buttonPart.Size = Vector3.new(2, 1, 2)
buttonPart.Position = player.Character.HumanoidRootPart.Position + Vector3.new(0, 5, 0)
buttonPart.Anchored = true
buttonPart.BrickColor = BrickColor.new("Bright blue")
buttonPart.Parent = workspace

-- Criar uma função para abrir as opções
local function openOptions()
    -- Limpar as opções anteriores
    for _, child in pairs(gui:GetChildren()) do
        if child:IsA("TextButton") or child:IsA("TextBox") then
            child:Destroy()
        end
    end

    -- 1. Ver forma e nome dos jogadores
    local function showPlayers()
        for _, v in pairs(game.Players:GetPlayers()) do
            print(v.Name, v.Character and v.Character:GetModelSize())
        end
    end

    createButton("Ver jogadores", UDim2.new(0, 0, 0, 0), showPlayers)

    -- 2. Ajuste de velocidade
    local speedTextBox = createTextBox(UDim2.new(0, 0, 0, 60), "Velocidade (1-100)")
    createButton("Ajustar Velocidade", UDim2.new(0, 0, 0, 120), function()
        local speedValue = tonumber(speedTextBox.Text)
        if speedValue and speedValue >= 1 and speedValue <= 100 then
            player.Character.Humanoid.WalkSpeed = speedValue
        else
            print("Por favor, insira um valor válido de velocidade entre 1 e 100.")
        end
    end)

    -- 3. Ajuste de pulo
    local jumpTextBox = createTextBox(UDim2.new(0, 0, 0, 180), "Pulo (1-100)")
    createButton("Ajustar Pulo", UDim2.new(0, 0, 0, 240), function()
        local jumpValue = tonumber(jumpTextBox.Text)
        if jumpValue and jumpValue >= 1 and jumpValue <= 100 then
            player.Character.Humanoid.JumpPower = jumpValue
        else
            print("Por favor, insira um valor válido de pulo entre 1 e 100.")
        end
    end)

    -- 4. Fry para voar
    createButton("Voar", UDim2.new(0, 0, 0, 300), function()
        local character = player.Character
        if character then
            character:Move(Vector3.new(0, 50, 0))
        end
    end)

    -- 5. Ficar invisível
    createButton("Ficar Invisível", UDim2.new(0, 0, 0, 360), function()
        local character = player.Character
        if character then
            character.HumanoidRootPart.Transparency = 1
            for _, part in pairs(character:GetChildren()) do
                if part:IsA("Part") then
                    part.Transparency = 1
                end
            end
        end
    end)

    -- 6. Atravessar paredes
    createButton("Atravessar Paredes", UDim2.new(0, 0, 0, 420), function()
        local character = player.Character
        if character then
            character.HumanoidRootPart.CanCollide = false
        end
    end)
end

-- Função para criar botões
local function createButton(text, position, callback)
    local button = Instance.new("TextButton")
    button.Text = text
    button.Size = UDim2.new(0, 200, 0, 50)
    button.Position = position
    button.Parent = gui
    button.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    
    button.MouseButton1Click:Connect(callback)
end

-- Função para criar TextBoxes
local function createTextBox(position, placeholder)
    local textBox = Instance.new("TextBox")
    textBox.Size = UDim2.new(0, 200, 0, 50)
    textBox.Position = position
    textBox.PlaceholderText = placeholder
    textBox.Parent = gui
    textBox.BackgroundColor3 = Color3.fromRGB(255, 255, 255)

    return textBox
end

-- Conectar o clique do cubo à função que abre as opções
buttonPart.Touched:Connect(function(hit)
    if hit:IsA("Player") then
        openOptions()
    end
end)
