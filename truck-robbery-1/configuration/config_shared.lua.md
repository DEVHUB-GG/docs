# config\_shared.lua

```lua
Shared = {}

Shared.MinPolice = 0 -- Minimum police required to start a crime scene mission

Shared.PedSettings = {
    enabled = true,
    peds = {
        {
            model = "a_m_y_juggalo_01",
            coords = vec4(-779.5085, -180.1830, 37.2836, 117.2973),
            blip = {
                enabled = false,
                sprite = 1,
                color = 2,
                scale = 0.8,
                label = "Crime Scene NPC",
            },
        },
    },
}

Shared.LaptopConfig = {
    enabled = true,
    traderId = 'trader_3',
    name = 'Crime Scenes',
    smallDescription = 'Clean up crime scenes and dispose of evidence.',
    description = 'Join the queue and wait for your turn. You will receive a vehicle location and crime scene coordinates. Clean up the scene, load evidence into the vehicle, and dispose of it.',
    bannerImage = 'https://upload.devhub.gg/dh_upload/laptop/appStore/appGallery/crimeTraders/crimeScenes/taskImage.webp',
    images = {
        'https://upload.devhub.gg/dh_upload/laptop/appStore/appGallery/crimeTraders/crimeScenes/img4.webp',
        'https://upload.devhub.gg/dh_upload/laptop/appStore/appGallery/crimeTraders/crimeScenes/img2.webp',
        'https://upload.devhub.gg/dh_upload/laptop/appStore/appGallery/crimeTraders/crimeScenes/taskImage.webp',
    },
    link = "https://store.devhub.gg",
    rewards = {
        items = {},
        money = 2000,
        crimePoints = 750,
        xp = 500,
    },
    tasks = {
        ['uid_1'] = {
            name = 'Join the queue',
            description = 'Join the queue and wait for your turn.',
            icon = 'fas fa-sign-in-alt',
            order = 1,
        },
        ['uid_2'] = {
            name = 'Pick up the vehicle',
            description = 'Go to the marked vehicle location on your map and pick up the assigned vehicle.',
            icon = 'fas fa-car',
            order = 2,
        },
        ['uid_3'] = {
            name = 'Drive to the crime scene',
            description = 'Drive the vehicle to the crime scene location marked on your map.',
            icon = 'fas fa-map-marker-alt',
            order = 3,
        },
        ['uid_4'] = {
            name = 'Clean up the scene',
            description = 'Pack up bodies, body parts, clean blood with UV light and mop, loot or burn crates.',
            icon = 'fas fa-broom',
            order = 4,
        },
        ['uid_5'] = {
            name = 'Load evidence into vehicle',
            description = 'Carry all packed objects and place them into the vehicle trunk.',
            icon = 'fas fa-box',
            order = 5,
        },
        ['uid_6'] = {
            name = 'Dispose of the vehicle',
            description = 'Drive to the disposal location and destroy the vehicle with all evidence inside.',
            icon = 'fas fa-fire',
            order = 6,
        },
    }
}

```
