--These scripts are supposed to fire when the player is allowed to click on a node to move a troop from one place to another.

local turnManagement = require(game:WaitForChild("ReplicatedStorage").turnManagement)
local gameManagement = require(game:WaitForChild("ReplicatedStorage").gameManagement)
local buildingGenerator = require(game:WaitForChild("ReplicatedStorage").buildingGenerator)

script.Parent.ClickDetector.MouseClick:Connect(function(player)

	local nodeName = script.Parent.Name
	
	if _G.decisionType == "movement" then
		
		--Code to restore color to the changed ones.
		game.ReplicatedStorage.Events.troopDecisionEnd:FireClient(player)
 
		local soldier = game.Workspace.currentMap:FindFirstChild(_G.lastSoldierClicked, true)
		local soldierAllegiance = soldier:GetAttribute("allegiance")

		--Should set previousNode to the node the soldier was on.
		local newSoldierPreviousNode = soldier:GetAttribute("currentNode")
		
		--The current soldier model is destroyed and a new one is made in its place.
		soldier:Destroy()
		
		--The user spends 1 chip to move a troop.
		local intValues = game:WaitForChild("ReplicatedStorage").intValues
		local Teams = game:GetService("Teams")
		if player.Team == Teams.A then
			intValues.AChips.Value = intValues.AChips.Value - 1
		elseif player.Team == Teams.B then
			intValues.BChips.Value = intValues.BChips.Value - 1
		end
		
		local playerTeam = player.Team
		
		turnManagement.troopSpawn(script.Parent.Name, 1, soldierAllegiance, newSoldierPreviousNode)	

		game.ReplicatedStorage.Events.updateCurrencyGUI:FireAllClients()
		
		--Refreshes the values and verifies correct-ness of attributes.
		_G.decisionType = ""
		gameManagement.verifyOccupancies()
		gameManagement.verifyTroopPopulations()
		gameManagement.maintainDestruction()
		
	elseif _G.decisionType == "buildFactory"  then
		
		--Sends information to the octaBuild function. If the node is full, it won't generate anything.
		local buildingNumber = script.Parent:GetAttribute("barrackNumber") + script.Parent:GetAttribute("factoryNumber")
		buildingGenerator.octaBuild(nodeName, buildingNumber, "factory" , player.Team.Name, false)
		
		--Building an actual building should finalize a "decision" and therefore, should refresh this.
		_G.decisionType = ""
		
		script.Parent.PrimaryPart.Color = script.Parent:GetAttribute("lastTeamColor")
		
		script.Parent.Sounds.building:Play()
		
		game.ReplicatedStorage.Events.updateCurrencyGUI:FireAllClients()
		game.ReplicatedStorage.Events.buildDecisionEnd:FireClient(player)
		
		
	elseif _G.decisionType == "buildBarracks" then
		
		--Sends information to the octaBuild function. If the node is full, it won't generate anything.
		local buildingNumber = script.Parent:GetAttribute("barrackNumber") + script.Parent:GetAttribute("factoryNumber")
		buildingGenerator.octaBuild(nodeName, buildingNumber, "barracks" , player.Team.Name, false)

		_G.decisionType = ""
		
		script.Parent.PrimaryPart.Color = script.Parent:GetAttribute("lastTeamColor")
		
		script.Parent.Sounds.building:Play()
		
		game.ReplicatedStorage.Events.updateCurrencyGUI:FireAllClients()
		game.ReplicatedStorage.Events.buildDecisionEnd:FireClient(player)
		
	end

end)
