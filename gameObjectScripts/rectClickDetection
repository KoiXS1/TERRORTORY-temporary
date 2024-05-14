local turnManagement = require(game:WaitForChild("ReplicatedStorage").turnManagement)
local gameManagement = require(game:WaitForChild("ReplicatedStorage").gameManagement)
local buildingGenerator = require(game:WaitForChild("ReplicatedStorage").buildingGenerator)

script.Parent.ClickDetector.MouseClick:Connect(function(player)
	
	local thisNode = script.Parent
	local nodeName = thisNode.Name

	local playerTeam = player.Team
	
	--The moveClickDetector should only be active here when a soldier has been clicked.
	if _G.decisionType == "movement" then
		
		--Code to restore color to the greenified ones as done by soldierSelect @StarterPlayerScripts
		--Destroys the moveClickDetector
		game.ReplicatedStorage.Events.troopDecisionEnd:FireClient(player)
			
		local soldier = game.Workspace.currentMap:FindFirstChild(_G.lastSoldierClicked, true)
		local soldierAllegiance = soldier:GetAttribute("allegiance")
		
		--Should set previousNode to the node the soldier was on.
		local newSoldierPreviousNode = soldier:GetAttribute("currentNode")
		
		soldier:Destroy()
		
		--A troop simply spawns there in its place.
		turnManagement.troopSpawn(script.Parent.Name, 1, soldierAllegiance, newSoldierPreviousNode)	

		--The user spends 1 chip to move a troop.
		local intValues = game:WaitForChild("ReplicatedStorage").intValues
		local Teams = game:GetService("Teams")
		if player.Team == Teams.A then
			intValues.AChips.Value = intValues.AChips.Value - 1
		elseif player.Team == Teams.B then
			intValues.BChips.Value = intValues.BChips.Value - 1
		end
		
		game.ReplicatedStorage.Events.updateCurrencyGUI:FireAllClients()
		
		_G.decisionType = ""
		gameManagement.verifyOccupancies()
		gameManagement.verifyTroopPopulations()
		gameManagement.maintainDestruction()
		
	elseif _G.decisionType == "buildTurret" then
		
		local lastSoldier = thisNode.PrimaryPart.Troops:GetChildren()[1]
		
		buildingGenerator.turretBuild(nodeName, lastSoldier:GetAttribute("allegiance"))
		
		_G.decisionType = ""
		
		script.Parent.Sounds.building:Play()
		
		game.ReplicatedStorage.Events.updateCurrencyGUI:FireAllClients()
		game.ReplicatedStorage.Events.buildDecisionEnd:FireClient(player)
	end
	
end)
