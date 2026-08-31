--============================================================--
--                      GAB HUB RP                            --
--                 WINDUI + BACKGROUND                        --
--============================================================--

local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")

local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

--============================================================--
-- WINDUI
--============================================================--

local WindUI = loadstring(game:HttpGet(
    "https://github.com/Footagesus/WindUI/releases/latest/download/main.lua"
))()

local Window = WindUI:CreateWindow({
    Title = "GAB Hub RP",
    Icon = "shield",
    Author = "Gab",
    Folder = "GABHubRP",

    Size = UDim2.fromOffset(580, 490),
    Theme = "Dark",
    Resizable = true,
    SideBarWidth = 200,

    Background = "rbxassetid://116020967072095",

    User = {
        Enabled = true,
        Anonymous = true
    }
})

Window:SetBackgroundImageTransparency(0.5)

--============================================================--
-- NOTIFICAÇÃO
--============================================================--

local function Notify(title, content)
    WindUI:Notify({
        Title = title,
        Content = content,
        Duration = 3
    })
end

--============================================================--
-- CHAT
--============================================================--

local function SendChat(text)

    if not text or text == "" then
        return
    end

    local Channels =
        TextChatService:FindFirstChild("TextChannels")

    if not Channels then
        Notify("GAB Hub", "TextChannels não encontrado.")
        return
    end

    local General =
        Channels:FindFirstChild("RBXGeneral")

    if not General then
        Notify("GAB Hub", "RBXGeneral não encontrado.")
        return
    end

    General:SendAsync(text)

end

--============================================================--
-- ABAS
--============================================================--

local Personagem = Window:Tab({
    Title = "Personagem",
    Icon = "user"
})

local ComandoRP = Window:Tab({
    Title = "Comando RP",
    Icon = "terminal"
})

local Revistar = Window:Tab({
    Title = "RP Revistar",
    Icon = "search"
})

local ESP = Window:Tab({
    Title = "ESP",
    Icon = "eye"
})

local View = Window:Tab({
    Title = "View",
    Icon = "video"
})

local Config = Window:Tab({
    Title = "Config",
    Icon = "settings"
})

--============================================================--
-- SEÇÕES
--============================================================--

local BioSection = Personagem:Section({
    Title = "BIOS",
    Opened = true
})

local CustomBioSection = Personagem:Section({
    Title = "Personalização",
    Opened = true
})

local CommandSection = ComandoRP:Section({
    Title = "Comandos RP",
    Opened = true
})

local ShotSection = ComandoRP:Section({
    Title = "Tiro RP",
    Opened = true
})

local CustomCommandSection = ComandoRP:Section({
    Title = "Meus Comandos",
    Opened = true
})

local RevistarSection = Revistar:Section({
    Title = "Revistar Jogador",
    Opened = true
})

local ResultadoSection = Revistar:Section({
    Title = "Resultado da Revista",
    Opened = true
})

local ESPSection = ESP:Section({
    Title = "ESP",
    Opened = true
})

local ViewSection = View:Section({
    Title = "View Player",
    Opened = true
})

local ConfigSection = Config:Section({
    Title = "GAB Hub RP",
    Opened = true
})

--============================================================--
-- COPIAR
--============================================================--

local function CopyText(text)

    if not text or text == "" then
        return
    end

    if setclipboard then
        setclipboard(text)
        Notify("GAB Hub", "Texto copiado!")

    elseif toclipboard then
        toclipboard(text)
        Notify("GAB Hub", "Texto copiado!")

    else
        Notify(
            "GAB Hub",
            "Seu executor não suporta copiar."
        )
    end
end

--============================================================--
-- BIOS
--============================================================--

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

local SelectedBio = Bios[1]

BioSection:Dropdown({
    Title = "Selecionar Bio",
    Values = Bios,
    Value = Bios[1],
    SearchBarEnabled = true,

    Callback = function(value)
        SelectedBio = value
    end
})

BioSection:Button({
    Title = "Copiar Bio",
    Icon = "copy",

    Callback = function()
        CopyText(SelectedBio)
    end
})

--============================================================--
-- BIO PERSONALIZADA
--============================================================--

local CustomBio = ""

CustomBioSection:Input({
    Title = "Bio Personalizada",
    Placeholder = "Digite sua bio...",
    Default = "",

    Callback = function(value)
        CustomBio = value or ""
    end
})

CustomBioSection:Button({
    Title = "Copiar Bio Personalizada",
    Icon = "copy",

    Callback = function()

        if CustomBio == "" then
            Notify("GAB Hub", "Digite uma bio primeiro.")
            return
        end

        CopyText(CustomBio)

    end
})

--============================================================--
-- PATENTE
--============================================================--

local RankText = ""

CustomBioSection:Input({
    Title = "Personalizar Patente",
    Placeholder = "Ex: RCT - Recruta",
    Default = "",

    Callback = function(value)
        RankText = value or ""
    end
})

CustomBioSection:Button({
    Title = "Gerar Bio da Patente",
    Icon = "badge",

    Callback = function()

        if RankText == "" then
            Notify("GAB Hub", "Digite a patente.")
            return
        end

        local Upper = string.upper(RankText)

        local HasRCT = string.find(
            Upper,
            "RCT",
            1,
            true
        )

        local RankName =
            RankText:match("%-%s*(.+)") or RankText

        local Result

        if HasRCT then
            Result =
                RankName ..
                " THE VOLK ANGB [RCT] 🇺🇸"
        else
            Result =
                RankName ..
                " THE VOLK ANGB 🇺🇸"
        end

        CopyText(Result)

    end
})

--============================================================--
-- COMANDOS RP
--============================================================--

local Commands = {
    "/Render THE VOLK ANGB 🇺🇸",
    "/KILL PRECISÃO 90% VENTO 30 KM POR HR",
    "/Algemar PERDEU PRA THE VOLK🤣🤣🇺🇸",
    "/FURA PNEU THE VOLK ANGB"
}

for _, Command in ipairs(Commands) do

    CommandSection:Button({
        Title = Command,
        Icon = "send",

        Callback = function()
            SendChat(Command)
        end
    })

end

--============================================================--
-- TIROS RP
--============================================================--

local Tiros = {
    "/TIRO PÉ",
    "/TIRO PERNA",
    "/TIRO TÓRAX",
    "/TIRO BRAÇO",
    "/TIRO PEITO",
    "/TIRO BARRIGA"
}

for _, Command in ipairs(Tiros) do

    ShotSection:Button({
        Title = Command,
        Icon = "crosshair",

        Callback = function()
            SendChat(Command)
        end
    })

end

--============================================================--
-- COMANDOS PERSONALIZADOS
--============================================================--

local CustomCommandText = ""
local CustomCommands = {}

CustomCommandSection:Input({
    Title = "Novo Comando",
    Placeholder = "Ex: /PATROL THE VOLK ANGB 🇺🇸",
    Default = "",

    Callback = function(value)
        CustomCommandText = value or ""
    end
})

CustomCommandSection:Button({

    Title = "Adicionar Comando",
    Icon = "plus",

    Callback = function()

        if CustomCommandText == "" then
            Notify("GAB Hub", "Digite um comando.")
            return
        end

        if #CustomCommands >= 10 then
            Notify(
                "GAB Hub",
                "Limite de 10 comandos."
            )
            return
        end

        local Command = CustomCommandText

        table.insert(
            CustomCommands,
            Command
        )

        CustomCommandSection:Button({

            Title = Command,
            Icon = "send",

            Callback = function()
                SendChat(Command)
            end

        })

        CustomCommandText = ""

        Notify(
            "GAB Hub",
            "Comando adicionado!"
        )

    end
})

--============================================================--
-- RP REVISTAR
--============================================================--

local SelectedRevistar = nil
local UltimoResultado = "Nenhuma revista realizada."

RevistarSection:Paragraph({

    Title = "Revista RP",

    Desc =
        "Selecione o jogador e envie /Revistar Username. " ..
        "A resposta dele será detectada pelo chat."

})

local function GetPlayerNamesRevistar()

    local List = {}

    for _, Player in ipairs(
        Players:GetPlayers()
    ) do

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

RevistarSection:Dropdown({

    Title = "Selecionar Jogador",

    Values = GetPlayerNamesRevistar(),

    SearchBarEnabled = true,

    Callback = function(value)

        SelectedRevistar =
            Players:FindFirstChild(value)

    end
})

RevistarSection:Button({

    Title = "Revistar Jogador",

    Icon = "search",

    Callback = function()

        if not SelectedRevistar then

            Notify(
                "RP Revistar",
                "Selecione um jogador."
            )

            return
        end

        UltimoResultado =
            "Aguardando resposta de @" ..
            SelectedRevistar.Name ..
            "..."

        SendChat(
            "/Revistar " ..
            SelectedRevistar.Name
        )

        Notify(
            "RP Revistar",
            "Revista iniciada. Aguardando os itens no chat."
        )

    end
})

--============================================================--
-- RESULTADO DA REVISTA
--============================================================--

ResultadoSection:Paragraph({

    Title = "Resultado",

    Desc = UltimoResultado

})

--============================================================--
-- DETECTOR DE RESPOSTA NO CHAT
--============================================================--

local function ProcessarMensagem(Player, Message)

    if not SelectedRevistar then
        return
    end

    if Player ~= SelectedRevistar then
        return
    end

    local Lower =
        string.lower(Message)

    local Itens = nil

    -- Aceita:
    -- Itens: celular, carteira
    -- Itens - celular, carteira
    -- Itens = celular, carteira

    Itens =
        Message:match(
            "[Ii]tens%s*:%s*(.+)"
        )

    if not Itens then

        Itens =
            Message:match(
                "[Ii]tens%s*%-%s*(.+)"
            )

    end

    if not Itens then

        Itens =
            Message:match(
                "[Ii]tens%s*=%s*(.+)"
            )

    end

    if not Itens then
        return
    end

    UltimoResultado =
        "@" ..
        Player.Name ..
        " informou: " ..
        Itens

    Notify(
        "RP Revistar",
        "Itens encontrados: " .. Itens
    )

end

-- TextChatService
if TextChatService.MessageReceived then

    TextChatService.MessageReceived:Connect(
        function(Message)

            local Text =
                Message.Text or ""

            local Source =
                Message.TextSource

            if not Source then
                return
            end

            local UserId =
                Source.UserId

            local Player =
                Players:GetPlayerByUserId(
                    UserId
                )

            if Player then

                ProcessarMensagem(
                    Player,
                    Text
                )

            end

        end
    )

end

--============================================================--
-- ESP
--============================================================--

local ESPEnabled = false
local ESPObjects = {}

local function RemoveESP(Player)

    local Data =
        ESPObjects[Player]

    if not Data then
        return
    end

    if Data.Highlight then
        Data.Highlight:Destroy()
    end

    if Data.Billboard then
        Data.Billboard:Destroy()
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

    local Character =
        Player.Character

    if not Character then
        return
    end

    local Head =
        Character:FindFirstChild("Head")

    if not Head then
        return
    end

    RemoveESP(Player)

    local Highlight =
        Instance.new("Highlight")

    Highlight.Name =
        "GABHubESP"

    Highlight.Adornee =
        Character

    Highlight.DepthMode =
        Enum.HighlightDepthMode.AlwaysOnTop

    Highlight.FillTransparency =
        0.75

    Highlight.OutlineTransparency =
        0

    Highlight.Parent =
        Character

    local Billboard =
        Instance.new("BillboardGui")

    Billboard.Name =
        "GABHubESPName"

    Billboard.Adornee =
        Head

    Billboard.Size =
        UDim2.fromOffset(220, 40)

    Billboard.StudsOffset =
        Vector3.new(0, 3, 0)

    Billboard.AlwaysOnTop =
        true

    Billboard.Parent =
        Head

    local Text =
        Instance.new("TextLabel")

    Text.BackgroundTransparency =
        1

    Text.Size =
        UDim2.fromScale(1, 1)

    Text.Text =
        "@" .. Player.Name

    Text.TextScaled =
        true

    Text.Font =
        Enum.Font.GothamBold

    Text.TextStrokeTransparency =
        0.2

    Text.Parent =
        Billboard

    ESPObjects[Player] = {

        Highlight = Highlight,

        Billboard = Billboard

    }

end

local function UpdateESP()

    for _, Player in ipairs(
        Players:GetPlayers()
    ) do

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

    Title = "ESP Nome + Corpo",

    Desc =
        "Mostra o username e marca o personagem.",

    Value = false,

    Callback = function(value)

        ESPEnabled = value

        UpdateESP()

    end

})

ESPSection:Button({

    Title = "Atualizar ESP",

    Icon = "refresh-cw",

    Callback = function()
        UpdateESP()
    end

})

--============================================================--
-- VIEW
--============================================================--

local SelectedPlayer = nil

local function GetPlayerNames()

    local List = {}

    for _, Player in ipairs(
        Players:GetPlayers()
    ) do

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

ViewSection:Dropdown({

    Title = "Selecionar Jogador",

    Values = GetPlayerNames(),

    SearchBarEnabled = true,

    Callback = function(value)

        SelectedPlayer =
            Players:FindFirstChild(value)

    end

})

ViewSection:Button({

    Title = "View Player",

    Icon = "video",

    Callback = function()

        if not SelectedPlayer then

            Notify(
                "GAB Hub",
                "Selecione um jogador."
            )

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

            Notify(
                "GAB Hub",
                "Personagem não encontrado."
            )

            return
        end

        Camera.CameraSubject =
            Humanoid

    end

})

ViewSection:Button({

    Title = "Parar View",

    Icon = "square",

    Callback = function()

        local Character =
            LocalPlayer.Character

        local Humanoid =
            Character and
            Character:FindFirstChildOfClass(
                "Humanoid"
            )

        if Humanoid then

            Camera.CameraSubject =
                Humanoid

        end

    end

})

--============================================================--
-- EVENTOS DE PLAYER
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

    if SelectedRevistar == Player then
        SelectedRevistar = nil
    end

    if SelectedPlayer == Player then
        SelectedPlayer = nil
    end

end)

--============================================================--
-- CONFIG
--============================================================--

ConfigSection:Paragraph({

    Title = "GAB Hub RP 🇺🇸",

    Desc =
        "WindUI • THE VOLK ANGB • Background personalizado"

})

ConfigSection:Slider({

    Title = "Transparência do Fundo",

    Desc = "Ajuste a imagem de fundo.",

    Value = {
        Min = 0,
        Max = 1,
        Default = 0.5
    },

    Step = 0.05,

    Callback = function(value)

        Window:SetBackgroundImageTransparency(
            value
        )

    end

})

ConfigSection:Button({

    Title = "Recarregar ESP",

    Icon = "refresh-cw",

    Callback = function()

        UpdateESP()

        Notify(
            "GAB Hub",
            "ESP atualizado."
        )

    end

})

--============================================================--
-- FINAL
--============================================================--

Window:SelectTab(1)

Notify(
    "GAB Hub RP",
    "Carregado com sucesso!"
)

print("======================================")
print("             GAB HUB RP")
print("======================================")
print("WindUI             OK")
print("Background         OK")
print("Personagem         OK")
print("Comandos RP        OK")
print("Tiros RP           OK")
print("Custom Commands    10 MAX")
print("RP Revistar        OK")
print("Chat Detection     OK")
print("ESP Nome + Corpo   OK")
print("View Player        OK")
print("======================================")
