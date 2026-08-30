--============================================================--
--                         GAB RP HUB                         --
--                           WINDUI                           --
--============================================================--

local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")

local LocalPlayer = Players.LocalPlayer

--============================================================--
-- WINDUI
--============================================================--

local WindUI = loadstring(game:HttpGet(
    "https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"
))()

local Window = WindUI:CreateWindow({
    Title = "Gab RP Hub",
    Icon = "user",
    Author = "Gab",
    Folder = "Gab RP Hub",

    Size = UDim2.fromOffset(580, 460),
    MinSize = Vector2.new(500, 350),
    MaxSize = Vector2.new(850, 650),

    ToggleKey = Enum.KeyCode.RightShift,
    Theme = "Dark",
    Resizable = true,

    OpenButton = {
        Title = "Abrir Gab RP",
        Icon = "user",
        Enabled = true,
        Draggable = true
    }
})

--============================================================--
-- ABA PERSONAGEM
--============================================================--

local CharacterTab = Window:Tab({
    Title = "Personagem",
    Icon = "user"
})

local function CopyText(Text)

    if not Text or Text == "" then
        return
    end

    if setclipboard then
        setclipboard(Text)

        WindUI:Notify({
            Title = "Gab RP Hub",
            Content = "Copiado! Cole na bio do Brookhaven.",
            Duration = 3
        })

    elseif toclipboard then
        toclipboard(Text)

        WindUI:Notify({
            Title = "Gab RP Hub",
            Content = "Copiado! Cole na bio do Brookhaven.",
            Duration = 3
        })

    else
        WindUI:Notify({
            Title = "Gab RP Hub",
            Content = "Seu ambiente não suporta copiar.",
            Duration = 4
        })
    end
end

--============================================================--
-- BIO
--============================================================--

local BioSection = CharacterTab:Section({
    Title = "Bio",
    Box = true,
    BoxBorder = true,
    Opened = true
})

BioSection:Paragraph({
    Title = "Bio do RP",
    Desc = "Obs: Será copiado e coloque na bio."
})

local Bios = {
    "RECRUTA THE VOLK ANGB [RCT] 🇺🇸",
    "RECRUTA | THE VOLK ANGB [RCT] 🇺🇸",
    "THE VOLK ANGB • RECRUTA [RCT] 🇺🇸",
    "VOLK ANGB | RCT - RECRUTA 🇺🇸",
    "RECRUTA VOLK ANGB 🇺🇸 [RCT]",
    "THE VOLK ANGB 🇺🇸 | RECRUTA",
    "VOLK ANGB 🇺🇸 • RCT • RECRUTA",
    "RECRUTA | VOLK AIR NATIONAL GUARD 🇺🇸",
    "VOLK AIR NATIONAL GUARD | RCT 🇺🇸",
    "THE VOLK ANGB 🇺🇸 | RCT",
    "VOLK ANGB RECRUIT [RCT] 🇺🇸",
    "RCT | VOLK ANGB 🇺🇸",
    "VOLK ANGB | RECRUIT 🇺🇸",
    "THE VOLK ANGB | RECRUTA 🇺🇸",
    "🇺🇸 VOLK ANGB | RCT - RECRUTA"
}

local SelectedBio

BioSection:Dropdown({
    Title = "Selecionar Bio",
    Desc = "Escolha uma bio da VOLK ANGB.",
    Values = Bios,

    Callback = function(Value)
        SelectedBio = Value
    end
})

BioSection:Button({
    Title = "Copiar Bio Selecionada",
    Desc = "Obs: Será copiado e coloque na bio.",
    Icon = "copy",

    Callback = function()

        if not SelectedBio then
            WindUI:Notify({
                Title = "Gab RP Hub",
                Content = "Selecione uma bio primeiro.",
                Duration = 3
            })
            return
        end

        CopyText(SelectedBio)
    end
})

--============================================================--
-- BIOS RÁPIDAS
--============================================================--

local QuickSection = CharacterTab:Section({
    Title = "Bios Rápidas",
    Box = true,
    BoxBorder = true,
    Opened = false
})

for Index, Bio in ipairs(Bios) do

    QuickSection:Button({
        Title = "Bio " .. Index,
        Desc = Bio,
        Icon = "copy",

        Callback = function()
            CopyText(Bio)
        end
    })

end

--============================================================--
-- PERSONALIZAR BIO
--============================================================--

local CustomSection = CharacterTab:Section({
    Title = "Personalizar Bio",
    Box = true,
    BoxBorder = true,
    Opened = true
})

local CustomBio = ""

CustomSection:Input({
    Title = "Sua Bio",
    Desc = "Digite uma bio personalizada.",
    Placeholder = "Digite sua bio aqui...",

    Callback = function(Value)
        CustomBio = Value
    end
})

CustomSection:Button({
    Title = "Copiar Bio Personalizada",
    Desc = "Obs: Será copiado e coloque na bio.",
    Icon = "copy",

    Callback = function()

        if CustomBio == "" then
            return
        end

        CopyText(CustomBio)
    end
})

--============================================================--
-- PERSONALIZAR PATENTE
--============================================================--

local RankSection = CharacterTab:Section({
    Title = "Personalizar Patente",
    Box = true,
    BoxBorder = true,
    Opened = true
})

local RankText = ""

RankSection:Input({
    Title = "Patente",
    Desc = "Exemplo: RCT - Recruta",
    Placeholder = "RCT - Recruta",

    Callback = function(Value)
        RankText = Value
    end
})

RankSection:Button({
    Title = "Gerar e Copiar Bio",
    Icon = "badge",

    Callback = function()

        if RankText == "" then
            return
        end

        local UpperText = string.upper(RankText)

        local HasRCT = string.find(
            UpperText,
            "RCT",
            1,
            true
        )

        local RankName =
            RankText:match("%-%s*(.+)") or RankText

        local GeneratedBio

        if HasRCT then
            GeneratedBio =
                RankName ..
                " THE VOLK ANGB [RCT] 🇺🇸"
        else
            GeneratedBio =
                RankName ..
                " THE VOLK ANGB 🇺🇸"
        end

        CopyText(GeneratedBio)
    end
})

RankSection:Paragraph({
    Title = "Sistema RCT",
    Desc =
        "Se a patente tiver RCT, será colocado [RCT]. "
        .. "Caso contrário, a sigla não será adicionada."
})

--============================================================--
--                    ABA COMANDO RP                          --
--============================================================--

local RPCommandTab = Window:Tab({
    Title = "Comando RP",
    Icon = "terminal"
})

local CommandsSection = RPCommandTab:Section({
    Title = "Comandos RP",
    Box = true,
    BoxBorder = true,
    Opened = true
})

local function SendRPCommand(Command)

    if not Command or Command == "" then
        return
    end

    local Channels =
        TextChatService:FindFirstChild("TextChannels")

    if Channels then

        local General =
            Channels:FindFirstChild("RBXGeneral")

        if General then
            General:SendAsync(Command)

            WindUI:Notify({
                Title = "Gab RP Hub",
                Content = "Comando enviado!",
                Duration = 2
            })

            return
        end
    end

    WindUI:Notify({
        Title = "Gab RP Hub",
        Content = "Chat não encontrado.",
        Duration = 3
    })
end

CommandsSection:Button({
    Title = "/Render",
    Desc = "THE VOLK ANGB 🇺🇸",
    Icon = "send",

    Callback = function()
        SendRPCommand(
            "/Render THE VOLK ANGB 🇺🇸"
        )
    end
})

CommandsSection:Button({
    Title = "/KILL",
    Desc = "PRECISÃO 90% • VENTO 30 KM POR HR",
    Icon = "target",

    Callback = function()
        SendRPCommand(
            "/KILL PRECISÃO 90% VENTO 30 KM POR HR"
        )
    end
})

CommandsSection:Button({
    Title = "/Algemar",
    Desc = "PERDEU PRA THE VOLK 🤣🤣🇺🇸",
    Icon = "lock",

    Callback = function()
        SendRPCommand(
            "/Algemar PERDEU PRA THE VOLK🤣🤣🇺🇸"
        )
    end
})

CommandsSection:Button({
    Title = "/FURA PNEU",
    Desc = "THE VOLK ANGB",
    Icon = "circle",

    Callback = function()
        SendRPCommand(
            "/FURA PNEU THE VOLK ANGB"
        )
    end
})

--============================================================--
-- TIROS RP
--============================================================--

local TiroSection = RPCommandTab:Section({
    Title = "Comandos de Tiro RP",
    Box = true,
    BoxBorder = true,
    Opened = true
})

local Tiros = {
    "/TIRO PÉ",
    "/TIRO PERNA",
    "/TIRO TÓRAX",
    "/TIRO BRAÇO",
    "/TIRO PEITO",
    "/TIRO BARRIGA"
}

for _, Command in ipairs(Tiros) do

    TiroSection:Button({
        Title = Command,
        Desc = "Enviar comando ao chat.",
        Icon = "crosshair",

        Callback = function()
            SendRPCommand(Command)
        end
    })

end

--============================================================--
--                        ABA ESP                             --
--============================================================--

local ESPTab = Window:Tab({
    Title = "ESP",
    Icon = "eye"
})

local ESPSection = ESPTab:Section({
    Title = "ESP Username",
    Box = true,
    BoxBorder = true,
    Opened = true
})

local ESPEnabled = false
local ESPObjects = {}

local function RemoveESP(Player)

    if not ESPObjects[Player] then
        return
    end

    for _, Object in ipairs(ESPObjects[Player]) do

        if Object and Object.Parent then
            Object:Destroy()
        end

    end

    ESPObjects[Player] = nil
end

local function CreateESP(Player)

    if Player == LocalPlayer then
        return
    end

    if not ESPEnabled then
        return
    end

    local Character = Player.Character

    if not Character then
        return
    end

    local Head = Character:FindFirstChild("Head")

    if not Head then
        return
    end

    RemoveESP(Player)

    local Billboard = Instance.new("BillboardGui")

    Billboard.Name = "GabESP"
    Billboard.Adornee = Head
    Billboard.Size = UDim2.fromOffset(220, 45)
    Billboard.StudsOffset = Vector3.new(0, 3, 0)
    Billboard.AlwaysOnTop = true
    Billboard.Parent = Head

    local Text = Instance.new("TextLabel")

    Text.BackgroundTransparency = 1
    Text.Size = UDim2.fromScale(1, 1)
    Text.Text = "@" .. Player.Name
    Text.TextScaled = true
    Text.Font = Enum.Font.GothamBold
    Text.TextStrokeTransparency = 0.3
    Text.Parent = Billboard

    ESPObjects[Player] = {
        Billboard
    }
end

local function UpdateESP()

    for _, Player in ipairs(Players:GetPlayers()) do

        if Player ~= LocalPlayer then

            if ESPEnabled then
                CreateESP(Player)
            else
                RemoveESP(Player)
            end

        end
    end
end

ESPSection:Toggle({
    Title = "ESP Username",
    Desc = "Mostra o @username acima dos jogadores.",
    Default = false,

    Callback = function(Value)

        ESPEnabled = Value
        UpdateESP()

    end
})

--============================================================--
-- VIEW PLAYER
--============================================================--

local ViewSection = ESPTab:Section({
    Title = "View Player",
    Box = true,
    BoxBorder = true,
    Opened = true
})

local SelectedPlayer = nil
local ViewingPlayer = nil

local function GetPlayerNames()

    local List = {}

    for _, Player in ipairs(Players:GetPlayers()) do

        if Player ~= LocalPlayer then
            table.insert(
                List,
                Player.Name
            )
        end

    end

    table.sort(List)

    return List
end

local PlayerDropdown = ViewSection:Dropdown({

    Title = "Selecionar Jogador",

    Desc = "Escolha um jogador.",

    Values = GetPlayerNames(),

    Callback = function(Value)

        SelectedPlayer =
            Players:FindFirstChild(Value)

    end
})

ViewSection:Button({

    Title = "Atualizar Lista",

    Icon = "refresh-cw",

    Callback = function()

        PlayerDropdown:Refresh(
            GetPlayerNames()
        )

        WindUI:Notify({
            Title = "Gab RP Hub",
            Content = "Lista atualizada.",
            Duration = 2
        })

    end
})

ViewSection:Button({

    Title = "View Player",

    Desc = "Acompanhar a câmera do jogador selecionado.",

    Icon = "eye",

    Callback = function()

        if not SelectedPlayer then

            WindUI:Notify({
                Title = "Gab RP Hub",
                Content = "Selecione um jogador.",
                Duration = 3
            })

            return
        end

        local Character =
            SelectedPlayer.Character

        local Humanoid =
            Character and
            Character:FindFirstChildOfClass(
                "Humanoid"
            )

        if not Humanoid then

            WindUI:Notify({
                Title = "Gab RP Hub",
                Content = "Personagem não encontrado.",
                Duration = 3
            })

            return
        end

        workspace.CurrentCamera.CameraSubject =
            Humanoid

        ViewingPlayer = SelectedPlayer

        WindUI:Notify({
            Title = "Gab RP Hub",
            Content =
                "Visualizando @" ..
                SelectedPlayer.Name,
            Duration = 3
        })
    end
})

ViewSection:Button({

    Title = "Parar View",

    Desc = "Voltar a câmera para seu personagem.",

    Icon = "camera",

    Callback = function()

        local Character =
            LocalPlayer.Character

        local Humanoid =
            Character and
            Character:FindFirstChildOfClass(
                "Humanoid"
            )

        if Humanoid then

            workspace.CurrentCamera.CameraSubject =
                Humanoid

        end

        ViewingPlayer = nil

        WindUI:Notify({
            Title = "Gab RP Hub",
            Content = "View encerrado.",
            Duration = 2
        })
    end
})

--============================================================--
-- EVENTOS ESP
--============================================================--

Players.PlayerAdded:Connect(function(Player)

    Player.CharacterAdded:Connect(function()

        task.wait(1)

        if ESPEnabled then
            CreateESP(Player)
        end

    end)
end)

Players.PlayerRemoving:Connect(function(Player)

    RemoveESP(Player)

    if ViewingPlayer == Player then

        local Character =
            LocalPlayer.Character

        local Humanoid =
            Character and
            Character:FindFirstChildOfClass(
                "Humanoid"
            )

        if Humanoid then

            workspace.CurrentCamera.CameraSubject =
                Humanoid

        end

        ViewingPlayer = nil
    end
end)

--============================================================--
-- FINAL
--============================================================--

Window:Open()

WindUI:Notify({
    Title = "Gab RP Hub",
    Content = "Gab RP Hub carregado!",
    Duration = 4
})

print("================================")
print("          GAB RP HUB             ")
print("================================")
print("Personagem: OK")
print("Comando RP: OK")
print("ESP Username: OK")
print("View Player: OK")
print("================================")
