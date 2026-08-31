--============================================================--
-- RP REVISTAR - GAB HUB
--============================================================--

local RevistarTab = Window:Tab({
    Title = "RP Revistar",
    Icon = "search"
})

local RevistarSection = RevistarTab:Section({
    Title = "Revista",
    Opened = true
})

local ResultadoSection = RevistarTab:Section({
    Title = "Resultado",
    Opened = true
})

local SelectedTarget = nil
local ResultadoTexto = "Nenhuma revista realizada."

-- Lista de jogadores
local function GetPlayersList()
    local list = {}

    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            table.insert(list, player.Name)
        end
    end

    table.sort(list)
    return list
end

-- Resultado visual
local Resultado = ResultadoSection:Paragraph({
    Title = "📋 Resultado da Revista",
    Desc = ResultadoTexto
})

-- Selecionar jogador
RevistarSection:Dropdown({
    Title = "Selecionar Jogador",
    Values = GetPlayersList(),
    SearchBarEnabled = true,

    Callback = function(value)
        SelectedTarget = Players:FindFirstChild(value)
    end
})

-- Comando /Revistar
RevistarSection:Button({
    Title = "🔎 Revistar Jogador",
    Icon = "search",

    Callback = function()

        if not SelectedTarget then
            Notify(
                "RP Revistar",
                "Selecione um jogador."
            )
            return
        end

        local username = SelectedTarget.Name

        -- Mostra o estado da revista
        ResultadoTexto =
            "Revista iniciada em @" ..
            username ..
            "\n\nConsultando inventário..."

        Resultado:Set({
            Title = "📋 Resultado da Revista",
            Desc = ResultadoTexto
        })

        -- Comando RP
        SendChat("/Revistar " .. username)

        Notify(
            "RP Revistar",
            "Revista iniciada em @" .. username
        )

        --================================================--
        -- IMPORTANTE:
        -- Aqui entraria a consulta à API do SEU jogo.
        -- Exemplo:
        --
        -- API → inventário do jogador
        -- API → retorna os itens
        -- Hub → mostra os itens
        --
        -- Não é possível consultar o inventário
        -- privado do Brookhaven dessa forma.
        --================================================--

    end
})

-- Atualizar lista
RevistarSection:Button({
    Title = "🔄 Atualizar Jogadores",
    Icon = "refresh-cw",

    Callback = function()

        Notify(
            "RP Revistar",
            "Lista de jogadores atualizada."
        )

    end
})

-- Limpar resultado
RevistarSection:Button({
    Title = "🗑️ Limpar Resultado",
    Icon = "trash",

    Callback = function()

        ResultadoTexto =
            "Nenhuma revista realizada."

        Resultado:Set({
            Title = "📋 Resultado da Revista",
            Desc = ResultadoTexto
        })

    end
})
