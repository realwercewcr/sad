local plr = game.Players.LocalPlayer
local emotegui = Instance.new("ScreenGui")
emotegui.Parent = plr.PlayerGui
local textbox = Instance.new("TextBox")
textbox.Size =UDim2.new(0.194, 0,0.063, 0)
textbox.Position =UDim2.new(0.431, 0,0.1, 0)
textbox.Parent = emotegui
local function thign(emote)
	local emoteinfo = require(emote)
	local plr = game.Players.LocalPlayer
	local anim = Instance.new("Animation")
	anim.AnimationId = emoteinfo.AssetID 
	local success = pcall(function()
		local SFX = emoteinfo.SFX 
		local sfx2 = emoteinfo.SFXProperties 
	end)
	local soundend = nil
	if success then
		local SFX = emoteinfo.SFX 
		local sfx2 = emoteinfo.SFXProperties 
		local sound = Instance.new("Sound")
		sound.Parent = game.SoundService
		sound.SoundId = SFX
		pcall(function()
			sound.Volume = sfx2.Volume
		end)
		soundend = sound
		sound:Play()
		sound.Ended:Connect(function()
			sound:Destroy()
		end)
	end
	local anim = plr.Character:FindFirstChildWhichIsA("Humanoid"):LoadAnimation(anim)
	anim:Play()
	local emotespeed = Instance.new("NumberValue")
	emotespeed.Name = "Emoting"
	emotespeed.Parent = plr.Character:WaitForChild("SpeedMultipliers")
	local uis = game:GetService("UserInputService")
	local basespeed = plr.Character:FindFirstChildWhichIsA("Humanoid"):GetAttribute("BaseSpeed")
	if basespeed then
		local success = pcall(function()
			local speed = emoteinfo.Speed
		end)
		if success then
			local speed = emoteinfo.Speed
			local success = pcall(function()
				emotespeed.Value = basespeed/speed
			end)
			if success then
			else
				emotespeed.Value = 0
			end
		else
			emotespeed.Value = 0
		end
	end
	pcall(function()
		emoteinfo.Created({plr.Character})
	end)
	uis.InputBegan:Connect(function(keu,s)
		if s then return end
		if keu.KeyCode == Enum.KeyCode.Space then
			anim:Stop()
			emotespeed:Destroy()
			pcall(function()
				soundend:Stop()
			end)
			pcall(function()
				emoteinfo.Destroyed({plr.Character})

			end)
		end
	end)

end
textbox.FocusLost:Connect(function()
local STRING_EMOTENAME = textbox.Text
-- _SillyBilly, _MissTheQuiet, _Subterfuge, And More.
--s
--s
local e = game:GetService("ReplicatedStorage"):FindFirstChild("Assets"):FindFirstChild("Emotes")
local emote = e:FindFirstChild(STRING_EMOTENAME)
if emote then
		thign(emote)
else
local string2 = "_"..STRING_EMOTENAME
		emote = e:FindFirstChild(string2)
if  emote then
	thign(emote)
end
end
end)
