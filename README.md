--============================================================--
--                       GAB RP HUB                           --
--                       FLUENT UI                            --
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

local SaveManager = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"
))()

local InterfaceManager = loadstring(game:HttpGet(
    "https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"
))()

--============================================================--
-- CONFIGURAÇÃO
--============================================================--

local BackgroundImage =
    "rbxassetid://116020967072095"

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
-- COPIAR TEXTO
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
-- PERSONAGEM
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
    Multi = false,
    Default = nil
}):OnChanged(function(Value)

    SelectedBio = Value

end)

Tabs.Personagem:AddButton({
    Title = "Copiar Bio Selecionada",

    Description = "Obs: Será copiado e coloque na bio.",

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
-- PERSONALIZAR BIO
--============================================================--

Tabs.Personagem:AddParagraph({
    Title = "Personalizar Bio",
    Content = "Crie sua própria bio."
})

local CustomBio = ""

Tabs.Personagem:AddInput("CustomBio", {
    Title = "Sua Bio",
    Description = "Digite sua bio personalizada.",
    Placeholder = "Digite aqui...",
    Default = "",
    Numeric = false,
    Finished = false
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
-- PERSONALIZAR PATENTE
--============================================================--

Tabs.Personagem:AddParagraph({
    Title = "Personalizar Patente",
    Content = "Exemplo: RCT - Recruta"
})

local RankText = ""

Tabs.Personagem:AddInput("RankInput", {
    Title = "Patente",
    Description = "Digite a patente.",
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
-- COMANDO RP
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

Tabs.ComandoRP:AddButton({
    Title = "/Render",

    Description = "THE VOLK ANGB 🇺🇸",

    Callback = function()

        SendRPCommand(
            "/Render THE VOLK ANGB 🇺🇸"
        )

    end
})

Tabs.ComandoRP:AddButton({
    Title = "/KILL",

    Description = "PRECISÃO 90% • VENTO 30 KM POR HR",

    Callback = function()

        SendRPCommand(
            "/KILL PRECISÃO 90% VENTO 30 KM POR HR"
        )

    end
})

Tabs.ComandoRP:AddButton({
    Title = "/Algemar",

    Description = "PERDEU PRA THE VOLK 🤣🤣🇺🇸",

    Callback = function()

        SendRPCommand(
            "/Algemar PERDEU PRA THE VOLK🤣🤣🇺🇸"
        )

    end
})

Tabs.ComandoRP:AddButton({
    Title = "/FURA PNEU",

    Description = "THE VOLK ANGB",

    Callback = function()

        SendRPCommand(
            "/FURA PNEU THE VOLK ANGB"
        )

    end
})

--============================================================--
-- TIROS RP
--============================================================--

Tabs.ComandoRP:AddParagraph({
    Title = "Comandos de Tiro RP",
    Content = "Selecione a região."
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

        Description = "Enviar comando ao chat.",

        Callback = function()

            SendRPCommand(Command)

        end
    })

end

--============================================================--
-- COMANDO PERSONALIZADO
--============================================================--

Tabs.ComandoRP:AddParagraph({
    Title = "Criar Comando Personalizado",
    Content =
        "Os comandos ficam somente durante esta execução."
})

local CustomCommand = ""

Tabs.ComandoRP:AddInput("CustomCommand", {

    Title = "Novo Comando",

    Description = "Digite o comando.",

    Placeholder =
        "Ex: /PATROL THE VOLK ANGB 🇺🇸",

    Default = ""

}):OnChanged(function(Value)

    CustomCommand = Value

end)

-- Comandos personalizados em memória
local CustomCommands = {}

Tabs.ComandoRP:AddButton({

    Title = "Adicionar Comando",

    Description =
        "Adiciona somente durante esta execução.",

    Callback = function()

        if not CustomCommand
            or CustomCommand:match("^%s*$") then

            Notify(
                "Gab RP Hub",
                "Digite um comando primeiro."
            )

            return
        end

        table.insert(
            CustomCommands,
            CustomCommand
        )

        Notify(
            "Gab RP Hub",
            "Comando adicionado!"
        )

        CustomCommand = ""

    end
})

Tabs.ComandoRP:AddParagraph({
    Title = "Me
