For NPC from addons

local soundPath = "sound/npc/death.wav" -- Specify the path to the sound file

hook.Add("Think", "NPCDeathSound", function()
    for _, npc in pairs(ents.FindByClass("npc_*")) do
        if npc:Health() <= 0 and npc.npcDeathSoundPlayed ~= true then
            npc.npcDeathSoundPlayed = true

            local addons = {
                "my_addon", -- replace with a list of your addons
                "another_addon"
            }

            local soundFileExists = false
            local addonPath = "sound/" -- path relative to addons folder

            for _, addon in ipairs(addons) do
                if file.Exists(addonPath .. addon .. "/" .. soundPath, "GAME") then
                    soundFileExists = true
                    soundPath = addonPath .. addon .. "/" .. soundPath
                    break
                end
            end

            if soundFileExists then
                if soundPath ~= "" then
                    sound.PlayFile(soundPath, "noplay", function(station, errorID, errorName)
                        if not IsValid(station) then
                            ErrorNoHalt("Error playing sound: " .. tostring(errorName) .. " (" .. tostring(errorID) .. ")")
                            return
                        end

                        station:SetPos(npc:GetPos())
                        station:SetVolume(1)
                        station:Play()

                        timer.Simple(5, function()
                            if IsValid(station) then
                                station:Stop()
                            end
                        end)
                    end)
                end
            end
        end
    end
end)
