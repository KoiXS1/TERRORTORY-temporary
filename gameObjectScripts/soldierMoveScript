local gameManagement = require(game:getService("ReplicatedStorage").gameManagement)
local Teams = game:GetService("Teams")
local thisSoldier = script.Parent

local troopDecisionStart = game.ReplicatedStorage.Events.troopDecisionStart

thisSoldier.ClickDetector.MouseClick:Connect(function(player)
	
	local intValues = game:WaitForChild("ReplicatedStorage").intValues

	if 

		(thisSoldier:GetAttribute("allegiance") == "A" and player.Team == Teams.A and intValues.AChips.Value >= 1) 
		or 
		(thisSoldier:GetAttribute("allegiance") == "B" and player.Team == Teams.B and intValues.BChips.Value >= 1)

	then

		local soldierNodeName = thisSoldier:GetAttribute("currentNode")
		
		_G.lastSoldierClicked = thisSoldier.Name
		_G.decisionType = "movement"

		--Event management is located in StarterPlayerScripts.soldierSelect
		troopDecisionStart:FireClient(player, thisSoldier, soldierNodeName)
		
	elseif

		(thisSoldier:GetAttribute("allegiance") == "A" and player.Team == Teams.A and intValues.AChips.Value == 0) 
		or 
		(thisSoldier:GetAttribute("allegiance") == "B" and player.Team == Teams.B and intValues.BChips.Value == 1) 
		
	then
		game:WaitForChild("ReplicatedStorage").Events.warning:FireClient(player, "Not enough money to move a troop.")
	end

end)
