# Pre-made skills

## <mark style="color:yellow;">Pre-made skills</mark>

The following skills are available in the personal category:

1. Run Faster
2. Swim Faster
3. More Stamina Regeneration
4. More Stamina
5. Time Underwater
6. Reduce Stamina On Bike
7. More Max HP
8. HP Regeneration
9. Melee Damage Multiplier
10. Damage Reducer
11. Ignore Taser
12. Feast Damage Multiplier
13. Better Accuracy
14. Faster Reload
15. Low HP Weapon Damage Multiplier

***

## <mark style="color:yellow;">Configuring Skill Effects</mark>

To modify the final effect of any skill in the personal category:



{% tabs %}
{% tab title="Exclusive version" %}
Use the skill generator UI to modify effect values or use `configs/skills.lua`.
{% endtab %}

{% tab title="Standard version" %}
Adjust the effect values in `configs/skills.lua`.
{% endtab %}
{% endtabs %}

> **Note**: The effect values directly influence the calculations shown below. Increasing or decreasing these values will proportionally change the final outcome of each skill.

***

## <mark style="color:yellow;">Skill Calculations</mark>

### Run Faster

* **Base**: `runSprintMultiplier = 1.0`
* **Effect Calculation**: `runSprintMultiplier = base + (0.03 * effect)`
* **Example**: Level 3 = 1.0 + (0.03 × 3) = 1.09 (9% faster)
* **Native:** SetRunSprintMultiplierForPlayer

### Swim Faster

* **Base**: `swimMultiplier = 1.0`
* **Effect Calculation**: `swimMultiplier = base + (0.15 * effect)`
* **Example**: Level 2 = 1.0 + (0.15 × 2) = 1.3 (30% faster)
* **Native:** SetSwimMultiplierForPlayer

### More Stamina Regeneration

* **Base**: `staminaRegenMultiplier = 0.00`
* **Effect Calculation**: `staminaRegenMultiplier = base + math.floor(effect)`
* **Example**: Level 3 = 0 + floor(3) = 3 points per tick
* **Native**: SetPlayerStamina

### More Stamina

* **Base**: `maxStamina = Config.SkillDefaultValues['moreStamina'] or 100.0`
* **Effect Calculation**: `maxStamina = base + math.floor(maxStamina * effect / 100)`
* **Example**: 10% effect = 100 + floor(100 × 10 / 100) = 110
* **Native**: SetPlayerMaxStamina

### Time Underwater

* **Base**: `timeUnderwater = Config.SkillDefaultValues['timeUnderwater'] or 10.0`
* **Effect Calculation**: `timeUnderwater = base + (5 * effect)`
* **Example**: Level 2 = 10 + (5 × 2) = 20 seconds
* **Native**: SetPedMaxTimeUnderwater

### More Max HP

* **Base**: `moreMaxHp = Config.SkillDefaultValues['moreMaxHp'] or 200`
* **Effect Calculation**: `moreMaxHp = base + math.floor(moreMaxHp * effect / 100)`
* **Example**: 5% effect = 200 + floor(200 × 5 / 100) = 210
* **Native**: SetPedMaxHealth

### HP Regeneration

* **Base**: `hpRegeneration = 0`
* **Effect Calculation**: `hpRegeneration = base + math.floor(1 * effect)`
* **Example**: Level 3 = 0 + floor(1 × 3) = 3 HP per tick
* **Native**: SetEntityHealth

### Melee Damage Multiplier

* **Base**: `meleeDamageMultiplier = 1.0`
* **Effect Calculation**: `meleeDamageMultiplier = base + (effect / 100)`
* **Example**: 25% effect = 1.0 + (25/100) = 1.25 (25% more damage)
* **Native**: SetWeaponDamageModifier

### Damage Reducer

* **Base**: `damageReducer = 0`
* **Effect Calculation**: Direct percentage reduction
* **Example**: 15% effect reduces damage by 15%
* **Native**: SetEntityHealth

### Better Accuracy

* **Base**: `betterAccuracy = 1.0`
* **Effect Calculation**: `betterAccuracy = base - (0.1 * effect)`
* **Example**: Level 3 = 1.0 - (0.1 × 3) = 0.7 (30% less recoil)
* **Native:** SetWeaponRecoilShakeAmplitude

### Faster Reload

* **Base**: `fasterReload = 0.0`
* **Effect Calculation**: `fasterReload = base + effect`
* **Animation Cutoff**: `138 - fasterReload`
* **Example**: Level 20 cuts animation at frame 118

### Low HP Weapon Damage

* **Base**: `lowHpWeaponDamageMultiplier = 1.0`
* **Effect Calculation**: `lowHpWeaponDamageMultiplier = base + (effect / 100)`
* **Activates Below**: 15% HP
* **Example**: 50% effect = 1.0 + (50/100) = 1.5 (50% more damage)
* **Native:**  SetWeaponDamageModifier

### Reduce Stamina Usage On Bike

* **Effect**: Binary (true/false)
* **Condition**: Activates if effect > 0
* **Result**: Adds 1 stamina point when active
* **Native:** SetPlayerStamina

### Ignore Taser

* **Effect Calculation**: Direct percentage chance
* **Example**: 25% effect gives 25% chance to resist taser

{% hint style="danger" %}
Skills using SetWeaponDamageModifier might be overwriten by your build in scripts
{% endhint %}

