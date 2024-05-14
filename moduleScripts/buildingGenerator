local buildingGenerator = {}

local RepSto = game:GetService("ReplicatedStorage")
local currentMap = game.Workspace["currentMap"]
local mapParts = RepSto["Map Parts"]

local octaNodes = currentMap["octaNodes"]
local rectNodes = currentMap["rectNodes"]

_G.totalTroop = 0

--A pretty much exact copy of buildBarracks(nodeName, buildingNumber)
function buildingGenerator.octaBuild (nodeName, buildingNumber, buildingType, side, initial)

	--Looks in the currentMapFolder and tries to find an octaNode with the exact same name as nodeName.
	local nodeOfFocus = octaNodes:FindFirstChild(nodeName)
	local nodeCenter = nodeOfFocus.Platform 
	
	--Gets each coordinate of the node's center 
	local nodeX = nodeCenter.CFrame.X
	local nodeY = nodeCenter.CFrame.Y
	local nodeZ = nodeCenter.CFrame.Z

	local newBuilding
	local chipsDeduction

	--Assigns a model to newBuilding
	if buildingType == "factory" then

		newBuilding = mapParts.Factory:Clone()
		nodeOfFocus:SetAttribute(
			
			"factoryNumber",                                  --Attribute to be changed
			
			nodeOfFocus:GetAttribute("factoryNumber") + 1     --New attribute
			
			) 
		
		chipsDeduction = 8
		
	else -- The only other building that should be built on octa nodes is the barracks.

		newBuilding = mapParts.Barracks:Clone()
		
		nodeOfFocus:SetAttribute(
			
			"barrackNumber",

			nodeOfFocus:GetAttribute("barrackNumber") + 1

		) 
		
		chipsDeduction = 4
		
	end
	
	newBuilding.Parent = nodeOfFocus.PrimaryPart["Buildings"]

	local newCF

	--PLACES THE BUILDING INTO ITS CURRENT ORIENTATION
	if buildingNumber == 0 then

		newCF = 
			CFrame.new (nodeX - 7.5, nodeY + 2.5, nodeZ - 7.5) *  -- X, Y, and Z position 
			CFrame.fromEulerAngles(0, math.rad(45), 0)           -- X, Y, and Z orientation 
		
		--[[
			The code below should not run when there is 1 building as if there is
			a building already on the node, attribute sideWithBuildings should have
			already been set.
		]]
		nodeOfFocus:SetAttribute( 
			"sideWithBuildings",
			side
		)

	else   --THIS FUNCTION SHOULD NOT RUN IF THERE ARE 2 BUILDINGS IN A NODE ALREADY.

		newCF = 
			CFrame.new (nodeX + 7.5, nodeY + 2.5, nodeZ + 7.5) * 
			CFrame.fromEulerAngles(0, math.rad(45), 0)

	end
	
	newBuilding:SetPrimaryPartCFrame(newCF) 

	--Side specific changes
	local intValues = RepSto.intValues
		
	if side == "A" then
		newBuilding.PrimaryPart.Color = Color3.new(52, 84, 209)  -- Turns it blue
		if initial == false then
			intValues.AChips.Value = intValues.AChips.Value - chipsDeduction
		end
	else 
		newBuilding.PrimaryPart.Color = Color3.new(255, 186, 8)  -- Turns it orange
		if initial == false then
			intValues.BChips.Value = intValues.BChips.Value - chipsDeduction
		end
	end
end

function buildingGenerator.turretBuild (nodeName, side)
	
	local nodeOfFocus = rectNodes:FindFirstChild(nodeName).PrimaryPart
	
	--Refers to the troop whose "previousNode" attribute will be used to determine the direction the turrent points
	local engineer = nodeOfFocus["Troops"]:GetChildren()[1]
	
	--Gets the node that the turret will be built in.
	local currentNodeX = nodeOfFocus.CFrame.X
	local currentNodeY = nodeOfFocus.CFrame.Y
	local currentNodeZ = nodeOfFocus.CFrame.Z
	
	--The previous node that a soldier in a rectNode is in will always be an octaNode.
	local previousNode = octaNodes:FindFirstChild(engineer:GetAttribute("previousNode"))
	local previousNodeX = previousNode.PrimaryPart.CFrame.X
	local previousNodeZ = previousNode.PrimaryPart.CFrame.Z
	
	local turretCopy = mapParts.Turret:Clone()
	turretCopy.Parent = nodeOfFocus["Buildings"]
	
	CFrame.new(currentNodeX, currentNodeY + 2, currentNodeZ)
	
	local turretCFrame
	
	--This is for turret orientation use
	if currentNodeX - previousNodeX > 0 then
		turretCFrame = CFrame.new(currentNodeX, currentNodeY + 5, currentNodeZ)* CFrame.fromEulerAngles(0, math.rad(0), 0)
	elseif currentNodeX - previousNodeX < 0 then
		turretCFrame = CFrame.new(currentNodeX, currentNodeY + 5, currentNodeZ)* CFrame.fromEulerAngles(0, math.rad(180), 0)
	elseif currentNodeZ - previousNodeZ > 0 then
		turretCFrame = CFrame.new(currentNodeX, currentNodeY + 5, currentNodeZ)* CFrame.fromEulerAngles(0, math.rad(270), 0)	
	elseif currentNodeZ - previousNodeZ < 0 then
		turretCFrame = CFrame.new(currentNodeX, currentNodeY + 5, currentNodeZ)* CFrame.fromEulerAngles(0, math.rad(90), 0)
	end
	
	turretCopy:SetPrimaryPartCFrame(turretCFrame)
	turretCopy:SetAttribute("sideOwner", side)
	turretCopy.PrimaryPart.turretHealthIndicator.healthLabel.Text = turretCopy:GetAttribute("Health")
	--Side specific changes

	local intValues = RepSto.intValues

	if side == "A" then
		turretCopy.PrimaryPart.Color = Color3.new(52, 84, 209)  -- Turns it blue
		intValues.AChips.Value = intValues.AChips.Value - 5
	else 
		turretCopy.PrimaryPart.Color = Color3.new(255, 186, 8)  -- Turns it orange
		intValues.BChips.Value = intValues.BChips.Value - 5
	end

end

function buildingGenerator.tombstoneBuild (soldierCFrame, side)
	local thisTombstone = mapParts.Tombstone:Clone()
	thisTombstone.Parent = game.Workspace.currentMap
	thisTombstone:SetPrimaryPartCFrame(CFrame.new(soldierCFrame.X,soldierCFrame.Y-0.7, soldierCFrame.Z))
	
	if side == "A" then
		thisTombstone.Head.Color = Color3.fromRGB(255, 109, 11)
		thisTombstone.Slab.Color = Color3.fromRGB(255, 109, 11)
		
		thisTombstone.Head.FrontText.TextLabel.TextColor3 = Color3.fromRGB(0,0,0)
		thisTombstone.Head.BackText.TextLabel.TextColor3 = Color3.fromRGB(0,0,0)
	elseif side == "B" then
		thisTombstone.Head.Color = Color3.fromRGB(49, 172, 209)
		thisTombstone.Slab.Color = Color3.fromRGB(49, 172, 209)
	end
end

return buildingGenerator 
