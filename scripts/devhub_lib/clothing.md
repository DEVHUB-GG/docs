# 👕 Clothing

{% hint style="success" %}
File Path: <kbd>modules/systems/c.clothing.lua</kbd>
{% endhint %}

***

```lua
function SetPlayerOutfit(clothingConfig)
    SetPreviousClothing()
    local ped = PlayerPedId()
    for category, value in pairs(clothingConfig) do
        if ClothingData.variations[category] then
            PreviousClothing[category] = {GetPedDrawableVariation(ped, ClothingData.variations[category]), GetPedTextureVariation(ped, ClothingData.variations[category])}
            SetPedComponentVariation(ped, ClothingData.variations[category], value[1], value[2], 2)
        elseif ClothingData.props[category] then
            PreviousClothing[category] = {GetPedPropIndex(ped, ClothingData.props[category]), GetPedPropTextureIndex(ped, ClothingData.props[category])}
            SetPedPropIndex(ped, ClothingData.props[category], value[1], value[2], true)
        end
    end 
end

function SetPreviousClothing()
    local ped = PlayerPedId()
    for category, value in pairs(PreviousClothing) do
        if ClothingData.variations[category] then
            SetPedComponentVariation(ped, ClothingData.variations[category], value[1], value[2], 2)
        elseif ClothingData.props[category] then
            if value[1] == -1 then
                ClearPedProp(ped, ClothingData.props[category])
            else 
                SetPedPropIndex(ped, ClothingData.props[category], value[1], value[2]+1, true)
            end
        end
    end
    PreviousClothing = {}
end 
```
