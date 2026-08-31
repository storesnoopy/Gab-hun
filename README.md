--============================================================--
--                         GAB RP HUB                         --
--                         FLUENT UI                          --
--============================================================--

local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")

local LocalPlayer = Players.LocalPlayer

--============================================================--
-- FLUENT
--============================================================--

local Fluent = loadstring(game:HttpGet(
    "https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"
))()

local Window = Fluent:CreateWindow({
    Title = "Gab RP Hub",
    SubTitle = "VOLK ANGB 🇺🇸",
    TabWidth = 160,
    Size = UDim2.fromOffset(600, 500),
    Acrylic = true,
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl
})

--============================================================--
-- ABAS
--============================================================--

local Tabs = {
    Personagem = Window:AddTab({
        Title = "Personagem",
        Icon = "user"
    }),

    ComandoRP = Window:AddTab({
        Title = "Comando RP",
        Icon = "terminal"
    }),

    ESP = Window:AddTab({
        Title = "ESP",
        Icon = "eye"
    }),

    Config = Window:AddTab({
        Title = "Configurações",
        Icon = "settings"
    })
}

--============================================================--
-- CONFIGURAÇÃO
--============================================================--

local BackgroundImage =
    "rbxassetid://116020967072095"

--============================================================--
-- NOTIFICAÇÃO
--============================================================--

local function Notify(Title, Content)

    Fluent:Notify({
        Title = Title,
        Content = Content,
        Duration = 3
    })

end

--============================================================--
-- COPIAR
--============================================================--

local function CopyText(Text)

    if not Text or Text == "" then
        return
    end

    if setclipboard then

        setclipboard(Text)

        Notify(
            "Gab RP Hub",
            "Texto copiado!"
        )

    elseif toclipboard then

        toclipboard(Text)

        Notify(
            "Gab RP Hub",
            "Texto copiado!"
        )

    else

        Notify(
            "Gab RP Hub",
            "Seu ambiente não suporta copiar."
        )

    end
end

--============================================================--
--                    PERSONAGEM                              --
--============================================================--

Tabs.Personagem:AddParagraph({
    Title = "Bio do RP",
    Content = "Escolha uma bio e copie para colocar no Brookhaven."
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

local SelectedBio = nil

Tabs.Personagem:AddDropdown("BioDropdown", {
    Title = "Selecionar Bio",
    Values = Bios,
    Multi = false
}):OnChanged(function(Value)

    SelectedBio = Value

end)

Tabs.Personagem:AddButton({
    Title = "Copiar Bio Selecionada",
    Description = "Obs: será copiado e coloque na bio.",

    Callback = function()

        if not SelectedBio then

            Notify(
                "Gab RP Hub",
                "Selecione uma bio primeiro."
            )

            return
        end

        CopyText(SelectedBio)

    end
})

--============================================================--
-- BIO PERSONALIZADA
--============================================================--

Tabs.Personagem:AddParagraph({
    Title = "Personalizar Bio",
    Content = "Crie sua própria bio."
})

local CustomBio = ""

Tabs.Personagem:AddInput("CustomBio", {
    Title = "Sua Bio",
    Placeholder = "Digite sua bio...",
    Default = ""
}):OnChanged(function(Value)

    CustomBio = Value

end)

Tabs.Personagem:AddButton({
    Title = "Copiar Bio Personalizada",

    Callback = function()

        if CustomBio == "" then

            Notify(
                "Gab RP Hub",
                "Digite uma bio primeiro."
            )

            return
        end

        CopyText(CustomBio)

    end
})

--============================================================--
-- PATENTE
--============================================================--

Tabs.Personagem:AddParagraph({
    Title = "Personalizar Patente",
    Content = "Exemplo: RCT - Recruta"
})

local RankText = ""

Tabs.Personagem:AddInput("RankInput", {
    Title = "Patente",
    Placeholder = "RCT - Recruta",
    Default = ""
}):OnChanged(function(Value)

    RankText = Value

end)

Tabs.Personagem:AddButton({
    Title = "Gerar e Copiar Bio",

    Callback = function()

        if RankText == "" then
            return
        end

        local UpperText =
            string.upper(RankText)

        local HasRCT =
            string.find(
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

--============================================================--
--                    COMANDO RP                              --
--============================================================--

Tabs.ComandoRP:AddParagraph({
    Title = "Comandos RP",
    Content = "Clique em um comando para enviá-lo ao chat."
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

            Notify(
                "Gab RP Hub",
                "Comando enviado!"
            )

            return
        end
    end

    Notify(
        "Gab RP Hub",
        "Canal de chat não encontrado."
    )
end

--============================================================--
-- COMANDOS
--============================================================--

local Commands = {
    {
        Name = "/Render",
        Command = "/Render THE VOLK ANGB 🇺🇸"
    },

    {
        Name = "/KILL",
        Command = "/KILL PRECISÃO 90% VENTO 30 KM POR HR"
    },

    {
        Name = "/Algemar",
        Command = "/Algemar PERDEU PRA THE VOLK🤣🤣🇺🇸"
    },

    {
        Name = "/FURA PNEU",
        Command = "/FURA PNEU THE VOLK ANGB"
    }
}

for _, Data in ipairs(Commands) do

    Tabs.ComandoRP:AddButton({

        Title = Data.Name,

        Description = Data.Command,

        Callback = function()

            SendRPCommand(Data.Command)

        end
    })

end

--============================================================--
-- TIROS
--============================================================--

Tabs.ComandoRP:AddParagraph({
    Title = "Comandos de Tiro RP",
    Content = "Selecione uma região."
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

    Tabs.ComandoRP:AddButton({

        Title = Command,

        Description = "Enviar para o chat.",

        Callback = function()

            SendRPCommand(Command)

        end
    })

end

--============================================================--
-- COMANDOS PERSONALIZADOS
--============================================================--

Tabs.ComandoRP:AddParagraph({
    Title = "Comando Personalizado",
    Content = "Você pode criar no máximo 10 comandos."
})

local CustomCommand = ""
local CustomCommands = {}

Tabs.ComandoRP:AddInput("CustomCommandInput", {

    Title = "Comando",

    Description =
        "Digite o comando que deseja criar.",

    Placeholder =
        "Ex: /PATROL THE VOLK ANGB 🇺🇸",

    Default = ""

}):OnChanged(function(Value)

    CustomCommand = Value

end)

Tabs.ComandoRP:AddButton({

    Title = "Adicionar Comando",

    Description =
        "Adiciona o comando à lista.",

    Callback = function()

        if not CustomCommand
            or CustomCommand:match("^%s*$") then

            Notify(
                "Gab RP Hub",
                "Digite um comando primeiro."
            )

            return
        end

        if #CustomCommands >= 10 then

            Notify(
                "Gab RP Hub",
                "Limite de 10 comandos atingido."
            )

            return
        end

        local Command = CustomCommand

        table.insert(
            CustomCommands,
            Command
        )

        Tabs.ComandoRP:AddButton({

            Title = Command,

            Description =
                "Clique para enviar ao chat.",

            Callback = function()

                SendRPCommand(Command)

            end
        })

        CustomCommand = ""

        Notify(
            "Gab RP Hub",
            "Comando adicionado!"
        )
    end
})

Tabs.ComandoRP:AddParagraph({

    Title = "Meus Comandos",

    Content =
        "Cada comando criado aparece aqui automaticamente."
})

--============================================================--
--                         ESP                                --
--============================================================--

Tabs.ESP:AddParagraph({
    Title = "ESP",
    Content = "Nome + marcação do corpo dos jogadores."
})

local ESPEnabled = false
local ESPObjects = {}

--============================================================--
-- REMOVER ESP
--============================================================--

local function RemoveESP(Player)

    local Data = ESPObjects[Player]

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

--============================================================--
-- CRIAR ESP
--============================================================--

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

    -- MARCAÇÃO DO CORPO
    local Highlight =
        Instance.new("Highlight")

    Highlight.Name =
        "GabESP"

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

    -- NOME
    local Billboard =
        Instance.new("BillboardGui")

    Billboard.Name =
        "GabESPName"

    Billboard.Adornee =
        Head

    Billboard.Size =
        UDim2.fromOffset(220, 45)

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

--============================================================--
-- ATUALIZAR ESP
--============================================================--

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

--============================================================--
-- TOGGLE ESP
--============================================================--

Tabs.ESP:AddToggle(
    "ESPUsernameBody",
    {
        Title = "ESP Nome + Corpo",
        Description =
            "Mostra o nome e marca o personagem.",
        Default = false
    }
):OnChanged(function(Value)

    ESPEnabled = Value

    UpdateESP()

end)

--============================================================--
-- VIEW
--============================================================--

Tabs.ESP:AddParagraph({
    Title = "View Player",
    Content =
        "Visualize a câmera de outro jogador."
})

local SelectedPlayer = nil
local ViewingPlayer = nil

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

Tabs.ESP:AddDropdown(
    "PlayerDropdown",
    {
        Title = "Selecionar Jogador",
        Values = GetPlayerNames(),
        Multi = false
    }
):OnChanged(function(Value)

    SelectedPlayer =
        Players:FindFirstChild(Value)

end)

Tabs.ESP:AddButton({

    Title = "View Player",

    Description =
        "Acompanhar o jogador.",

    Callback = function()

        if not SelectedPlayer then

            Notify(
                "Gab RP Hub",
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
                "Gab RP Hub",
                "Personagem não encontrado."
            )

            return
        end

        workspace.CurrentCamera.CameraSubject =
            Humanoid

        ViewingPlayer =
            SelectedPlayer

        Notify(
            "Gab RP Hub",
            "Visualizando @" ..
            SelectedPlayer.Name
        )
    end
})

Tabs.ESP:AddButton({

    Title = "Parar View",

    Description =
        "Voltar para seu personagem.",

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

        Notify(
            "Gab RP Hub",
            "View encerrado."
        )
    end
})

--============================================================--
-- EVENTOS DOS JOGADORES
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
-- CONFIGURAÇÕES
--============================================================--

Tabs.Config:AddParagraph({

    Title = "Gab RP Hub",

    Content =
        "Gab RP Hub • Fluent UI • VOLK ANGB 🇺🇸"
})

Tabs.Config:AddButton({

    Title = "Recarregar ESP",

    Callback = function()

        UpdateESP()

        Notify(
            "Gab RP Hub",
            "ESP atualizado."
        )

    end
})

--============================================================--
-- FUNDO
--============================================================--

-- ID da imagem:
-- 116020967072095
--
-- Referência:
local GabBackgroundImage =
    "rbxassetid://116020967072095"

-- A aplicação como fundo depende do ScreenGui
-- interno da versão da Fluent utilizada.

--============================================================--
-- FINAL
--============================================================--

Window:SelectTab(1)

Notify(
    "Gab RP Hub",
    "Carregado com sucesso!"
)

print("================================")
print("          GAB RP HUB")
print("================================")
print("Fluent UI .............. OK")
print("Personagem ............. OK")
print("Bios ................... OK")
print("Comandos RP ............ OK")
print("Tiros RP ............... OK")
print("Personalizados 10 ...... OK")
print("ESP Nome + Corpo ....... OK")
print("View Player ............ OK")
print("================================")
