local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoid = char:WaitForChild("Humanoid")
local root = char:WaitForChild("HumanoidRootPart")

-- Aumentando a velocidade de movimento do personagem para 30
humanoid.WalkSpeed = 30

local empregos = workspace:WaitForChild("Floripa_workspace"):WaitForChild("Empregos")
local lixeiro = empregos:WaitForChild("Lixeiro")

local pegarFolder = lixeiro:FindFirstChild("Pegar")
local entregarFolder = lixeiro:FindFirstChild("Entregar")

-- Função para achar parte com ProximityPrompt dentro de uma pasta
local function acharParteComPrompt(folder)
	for _, item in ipairs(folder:GetDescendants()) do
		if item:IsA("ProximityPrompt") and item.Parent:IsA("BasePart") then
			return item.Parent
		end
	end
	return nil
end

local pegar = acharParteComPrompt(pegarFolder)

if not pegar then
	warn("Não foi possível encontrar uma parte com prompt em 'Pegar'.")
	return
end

-- Verificando se a pasta 'Entregar' possui uma parte válida
if not entregarFolder then
	warn("Não foi possível encontrar a pasta 'Entregar'.")
	return
end

-- Agora procuramos uma parte dentro da pasta 'Entregar' que tenha o ProximityPrompt
local entregar = acharParteComPrompt(entregarFolder)

if not entregar then
	warn("Não foi possível encontrar uma parte com prompt em 'Entregar'.")
	return
end

-- Função para andar e ativar o prompt de pegar e entregar rapidamente
local function andarEAcionarPromptRapid(alvo)
	-- Movendo para a posição do alvo sem esperar muito tempo
	humanoid:MoveTo(alvo.Position + Vector3.new(0, 0, -2))

	-- Esperando que a movimentação termine rapidamente (sem esperar por muito tempo)
	humanoid.MoveToFinished:Wait()

	-- Acionando o ProximityPrompt
	local prompt = alvo:FindFirstChildOfClass("ProximityPrompt")
	if prompt then
		fireproximityprompt(prompt)
		print("Prompt ativado em:", alvo.Name)
	else
		warn("Prompt não encontrado em:", alvo.Name)
	end
end

-- Loop infinito de pegar e entregar, sem pausas longas
while true do
	-- Andando e acionando o prompt de pegar
	andarEAcionarPromptRapid(pegar)

	-- Espera muito rápida entre as ações (0.3 segundos)
	task.wait(0.3) 

	-- Andando e acionando o prompt de entregar
	andarEAcionarPromptRapid(entregar)

	-- Espera muito rápida entre as ações (0.3 segundos)
	task.wait(0.3)
end
